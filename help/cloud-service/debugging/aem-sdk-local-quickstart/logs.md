---
title: 로그를 사용하여 AEM SDK 디버깅
description: 로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 455
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# 로그를 사용하여 AEM SDK 디버깅

AEM SDK의 로그에 액세스하면 AEM SDK 로컬 quickstart Jar 또는 Dispatcher 도구 가 AEM 애플리케이션 디버깅에 대한 주요 통찰력을 제공할 수 있습니다.

## AEM 로그

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

로그는 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다. AEM SDK의 로컬 빠른 시작 및 AEMas a Cloud Service 의 개발 환경에서 로그 가시성을 표준화하여 구성 트위들링과 재배포를 감소하므로 Adobe은 로컬 개발 및 AEM as a Cloud Service 개발 로깅 구성을 가능한 유사하게 유지할 것을 권장합니다.

다음 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 에서 찾은 Sling Logger OSGi 구성을 통해 로컬 개발을 위해 AEM 애플리케이션의 Java 패키지에 대한 디버그 수준에서 로깅을 구성합니다.

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

다음에 로그인합니다. `error.log`.

기본 로깅이 로컬 개발에 충분하지 않은 경우 AEM SDK의 로컬 빠른 시작의 로그 지원 웹 콘솔을 통해 ( )에서 애드혹 로깅을 구성할 수 있습니다.[/system/console/slinglog](http://localhost:4502/system/console/slinglog)) 그러나 AEM as a Cloud Service 개발 환경에서도 이러한 동일한 로그 구성이 필요하지 않는 한 임시 변경 사항을 Git에 지속하는 것은 권장되지 않습니다. 로그 지원 콘솔을 통한 변경 사항은 AEM SDK의 로컬 빠른 시작의 저장소에 직접 유지됩니다.

Java 로그 문은 `error.log` 파일:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

종종 다음을 &quot;꼬리&quot;를 매는 것이 유용합니다. `error.log` 출력을 터미널로 스트리밍합니다.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows에는 [타사 테일 애플리케이션](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 또는 의 사용 [Powershell의 Get-Content 명령](https://stackoverflow.com/a/46444596/133936).

## Dispatcher 로그

Dispatcher 로그는 다음과 같은 경우 stdout으로 출력됩니다. `bin/docker_run` 가 호출되지만 가 포함된 도커에서 로 로그에 직접 액세스할 수 있습니다.

### Docker 컨테이너의 로그 액세스{#dispatcher-tools-access-logs}

Dispatcher 로그는 의 도커 컨테이너에서 직접 액세스할 수 있습니다. `/etc/httpd/logs`.

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

_다음 `<CONTAINER ID>` 위치: `docker exec -it <CONTAINER ID> /bin/sh` 은(는) 의 목록에서 대상 도커 컨테이너 ID로 교체해야 합니다. `docker ps` 명령입니다._


### 로컬 파일 시스템에 Docker 로그 복사{#dispatcher-tools-copy-logs}

Dispatcher 로그는 의 도커 컨테이너에서 복사할 수 있습니다. `/etc/httpd/logs` 자주 사용하는 로그 분석 도구를 사용하여 검사할 로컬 파일 시스템으로 이동합니다. 시점 복제본이며 로그에 대한 실시간 업데이트를 제공하지 않습니다.

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

_다음 `<CONTAINER_ID>` 위치: `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 은(는) 의 목록에서 대상 도커 컨테이너 ID로 교체해야 합니다. `docker ps` 명령입니다._
