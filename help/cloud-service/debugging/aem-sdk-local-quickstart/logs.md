---
title: 로그를 사용하여 AEM SDK 디버깅
description: 로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# 로그를 사용하여 AEM SDK 디버깅

AEM SDK 로그에 액세스하는 경우 AEM SDK 로컬 빠른 시작 Jar 또는 Dispatcher Tools&#39;가 AEM 애플리케이션 디버깅에 대한 주요 통찰력을 제공할 수 있습니다.

## AEM 로그

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다. Adobe은 AEM SDK의 로컬 quickstart 및 AEM에 대한 로그 가시성을 Cloud Service 개발 환경으로 정규화하여 구성 위젯을 줄이고 재배포를 줄이면서 로컬 개발 및 AEM을 가능한 유사한 Cloud Service 개발 로깅 구성으로 유지하는 것이 좋습니다.

AEM [프로젝트 원형](https://github.com/adobe/aem-project-archetype) 포맷은

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

에 `error.log`로그인합니다.

로컬 개발에 기본 로깅이 충분하지 않은 경우 AEM SDK의 로컬 빠른 시작 로그 지원 웹 콘솔에서([/system/console/slinglog](http://localhost:4502/system/console/slinglog))의 임시 로깅을 구성할 수 있지만 AEM에서 Cloud Service 개발 환경처럼 동일한 로그 구성이 필요하지 않으면 임시 변경 사항이 Git에 적용되지 않는 것이 좋습니다. 로그 지원 콘솔을 통해 변경한 내용은 AEM SDK의 로컬 quickstart 저장소에 바로 유지됩니다.

Java 로그 문은 `error.log` 파일에서 볼 수 있습니다.

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

종종 출력물을 터미널로 `error.log` 스트리밍하는 &quot;꼬리&quot;에 유용하다.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows를 사용하려면 [타사 테일 응용 프로그램](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 또는 [Powershell의 Get-Content 명령을 사용해야 합니다](https://stackoverflow.com/a/46444596/133936).

## 디스패처 로그

디스패처 로그는 호출될 때 stdout으로 출력되지만 Docker 포함에서 로그 `bin/docker_run` 에 직접 액세스할 수 있습니다.

### Docker 컨테이너의 로그 액세스

발송자 로그는 의 Docker 컨테이너에서 직접 액세스할 수 있습니다 `/etc/httpd/logs`.

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

### Docker 로그를 로컬 파일 시스템에 복사

발송자 로그는 자주 사용하는 로그 분석 도구를 사용하여 로컬 파일 시스템 `/etc/httpd/logs` 에 있는 Docker 컨테이너에서 복사할 수 있습니다. 이 데이터는 특정 시점으로 기록되며 로그에 대한 실시간 업데이트를 제공하지 않습니다.

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

