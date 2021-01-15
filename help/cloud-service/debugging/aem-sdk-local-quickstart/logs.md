---
title: 로그를 사용하여 AEM SDK 디버깅
description: 로그는 AEM 응용 프로그램 디버깅에 있어 최전선 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# 로그를 사용하여 AEM SDK 디버깅

AEM SDK의 로그에 액세스하는 경우 AEM SDK 로컬 quickstart Jar 또는 Dispatcher Tools&#39;가 AEM 애플리케이션 디버깅에 대한 주요 통찰력을 제공할 수 있습니다.

## AEM 로그

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

로그는 AEM 응용 프로그램 디버깅에 있어 최전선 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다. Adobe은 AEM SDK의 로컬 quickstart 및 AEM에 대한 로그 가시성을 Cloud Service의 개발 환경으로 정규화하여 구성 위젯과 재배포를 줄이는 방법으로 로컬 개발 및 AEM을 Cloud Service 개발 로깅 구성으로 유지하는 것이 좋습니다.

[AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype)은

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

`error.log`에 로그인하는 경우.

기본 로깅이 로컬 개발에 충분하지 않은 경우 AEM SDK의 로컬 quickstart의 로그 지원 웹 콘솔([/system/console/slinglog](http://localhost:4502/system/console/slinglog))을 통해 애드혹 로깅을 구성할 수 있지만, Cloud Service 개발 환경에서도 이와 동일한 로그 구성이 필요하지 않은 한 Git에 애드혹 변경 사항이 권장되지 않습니다. 로그 지원 콘솔을 통해 변경한 내용은 AEM SDK의 로컬 quickstart 저장소에 바로 유지됩니다.

Java 로그 문은 `error.log` 파일에서 볼 수 있습니다.

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

터미널로 출력을 스트리밍하는 `error.log`을 &quot;꼬리&quot;하는 것이 유용한 경우가 많습니다.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows를 사용하려면 [3rd party 꼬리 응용 프로그램](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 또는 [Powershell의 Get-Content 명령](https://stackoverflow.com/a/46444596/133936)을 사용해야 합니다.

## 발송자 로그

디스패처 로그는 `bin/docker_run`을(를) 호출할 때 stdout으로 출력되지만 Docker 포함에서 로그에 직접 액세스할 수 있습니다.

### Docker 컨테이너의 로그 액세스{#dispatcher-tools-access-logs}

발송자 로그는 `/etc/httpd/logs`의 Docker 컨테이너에서 직접 액세스할 수 있습니다.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_in `<CONTAINER ID>` 을 명령에서 나열된 대상 Docker 컨테이너 ID로  `docker exec -it <CONTAINER ID> /bin/sh` 바꿔야  `docker ps` 합니다._


### Docker 로그를 로컬 파일 시스템{#dispatcher-tools-copy-logs}에 복사

디스패처 로그는 즐겨찾기 로그 분석 도구를 사용하여 검사를 위해 `/etc/httpd/logs`의 Docker 컨테이너에서 로컬 파일 시스템으로 복사할 수 있습니다. 이 문서는 지정 시간 사본이며 로그에 대한 실시간 업데이트를 제공하지 않습니다.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_in `<CONTAINER_ID>` 을 명령에서 나열된 대상 Docker 컨테이너 ID로  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 바꿔야  `docker ps` 합니다._
