---
title: 로그를 사용하여 AEM SDK 디버깅
description: 로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# 로그를 사용하여 AEM SDK 디버깅

AEM SDK 로그에 액세스하면 AEM SDK 로컬 quickstart Jar 또는 Dispatcher 도구 가 AEM 애플리케이션 디버깅에 대한 주요 통찰력을 제공할 수 있습니다.

## AEM 로그

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다. Adobe은 AEM SDK의 로컬 빠른 시작과 AEM as a Cloud Service의 개발 환경에서 로그 가시성을 표준화하여 구성 트위들링과 재배포를 감소하므로 로컬 개발 및 AEM as a Cloud Service 개발 로깅 구성을 가능한 유사하게 유지하는 것을 권장합니다.

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)은(는) 다음 위치에 있는 Sling Logger OSGi 구성을 통해 로컬 개발을 위해 AEM 애플리케이션의 Java 패키지에 대한 디버그 수준에서 로깅을 구성합니다.

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

`error.log`에 로그인합니다.

기본 로깅이 로컬 개발에 충분하지 않은 경우 AEM SDK의 로컬 빠른 시작의 로그 지원 웹 콘솔을 통해 ([/system/console/slinglog](http://localhost:4502/system/console/slinglog))에 Ad Hoc 로깅을 구성할 수 있지만, AEM as a Cloud Service 개발 환경에서도 이러한 동일한 로그 구성이 필요하지 않으면 Ad Hoc 변경 사항이 Git에 지속되지 않는 것이 좋습니다. 로그 지원 콘솔을 통한 변경 사항은 AEM SDK의 로컬 빠른 시작의 저장소에 직접 유지됩니다.

Java 로그 문은 `error.log` 파일에서 볼 수 있습니다.

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

종종 해당 출력을 터미널에 스트리밍하는 `error.log`을(를) &quot;테일&quot;하는 것이 유용합니다.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows에서는 [타사 테일 응용 프로그램](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 또는 [Powershell의 Get-Content 명령](https://stackoverflow.com/a/46444596/133936)을 사용해야 합니다.

## Dispatcher 로그

`bin/docker_run`이(가) 호출되면 Dispatcher 로그가 stdout으로 출력되지만 Docker에 포함된 를 사용하여 로그에 직접 액세스할 수 있습니다.

### Docker 컨테이너의 로그 액세스{#dispatcher-tools-access-logs}

Dispatcher 로그는 `/etc/httpd/logs`의 Docker 컨테이너에서 직접 액세스할 수 있습니다.

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

_`docker exec -it <CONTAINER ID> /bin/sh`의 `<CONTAINER ID>`은(는) `docker ps` 명령에서 나열된 대상 도커 컨테이너 ID로 대체해야 합니다._


### 로컬 파일 시스템에 Docker 로그 복사{#dispatcher-tools-copy-logs}

즐겨찾는 로그 분석 도구를 사용하여 검사할 Dispatcher 로그를 `/etc/httpd/logs`의 Docker 컨테이너에서 로컬 파일 시스템으로 복사할 수 있습니다. 시점 복제본이며 로그에 대한 실시간 업데이트를 제공하지 않습니다.

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

_`docker cp <CONTAINER_ID>:/var/log/apache2 ./`의 `<CONTAINER_ID>`은(는) `docker ps` 명령에서 나열된 대상 도커 컨테이너 ID로 대체해야 합니다._
