---
title: Dispatcher 도구 디버깅
description: Dispatcher 도구는 로컬에서 AEM을 Cloud Service AEM Publish 서비스의 Dispatcher로 시뮬레이션하는 데 사용할 수 있는 컨테이너화된 Apache 웹 서버 환경을 제공합니다. Dispatcher 도구의 로그 및 캐시 콘텐츠를 디버깅하면 엔드 투 엔드 AEM 애플리케이션 및 지원 캐시 및 보안 구성이 올바른지 확인하는 데 매우 중요한 역할을 할 수 있습니다.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 67
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Dispatcher 도구 디버깅

Dispatcher 도구는 로컬에서 AEM을 Cloud Service AEM Publish 서비스의 Dispatcher로 시뮬레이션하는 데 사용할 수 있는 컨테이너화된 Apache 웹 서버 환경을 제공합니다.

Dispatcher 도구의 로그 및 캐시 콘텐츠를 디버깅하면 엔드 투 엔드 AEM 애플리케이션 및 지원 캐시 및 보안 구성이 올바른지 확인하는 데 매우 중요한 역할을 할 수 있습니다.

>[!NOTE]
>
>Dispatcher 도구는 컨테이너를 기반으로 하므로 다시 시작할 때마다 이전 로그 및 캐시 콘텐츠가 제거됩니다.

## Dispatcher 도구 로그

Dispatcher 도구 로그는 다음을 통해 사용할 수 있습니다. `stdout` 또는 `bin/docker_run` 명령 또는 자세한 내용을 보려면 다음 Docker 컨테이너에 `/etc/https/logs`.

다음을 참조하십시오 [Dispatcher 로그](./logs.md#dispatcher-logs) Dispatcher 도구의 도커 컨테이너 로그에 직접 액세스하는 방법에 대한 지침입니다.

## Dispatcher 도구 캐시

### Docker 컨테이너의 로그 액세스

Dispatcher 캐시는 다음 도커 컨테이너에서 직접 액세스할 수 있습니다. ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### 로컬 파일 시스템에 Docker 로그 복사

Dispatcher 로그는 의 도커 컨테이너에서 복사할 수 있습니다. `/mnt/var/www/html` 원하는 도구를 사용하여 검사할 로컬 파일 시스템으로 이동합니다. 시점 복제본이며 캐시에 대한 실시간 업데이트를 제공하지 않습니다.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
