---
title: Dispatcher 도구 디버깅
description: Dispatcher 도구는 AEM을 Cloud Services의 AEM 게시 서비스의 Dispatcher로 로컬로 시뮬레이션하는 데 사용할 수 있는 컨테이너화된 Apache 웹 서버 환경을 제공합니다. Dispatcher 도구의 로그 및 캐시 콘텐츠를 디버깅하는 것은 종단 간 AEM 응용 프로그램과 캐시 및 보안 구성이 올바른지 확인하는 데 매우 중요합니다.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: 개발
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# Dispatcher 도구 디버깅

Dispatcher 도구는 AEM을 Cloud Services의 AEM 게시 서비스의 Dispatcher로 로컬로 시뮬레이션하는 데 사용할 수 있는 컨테이너화된 Apache 웹 서버 환경을 제공합니다.
Dispatcher 도구의 로그 및 캐시 콘텐츠를 디버깅하는 것은 종단 간 AEM 응용 프로그램과 캐시 및 보안 구성이 올바른지 확인하는 데 매우 중요합니다.

>[!NOTE]
>
>Dispatcher 도구는 컨테이너를 기반으로 하므로 다시 시작될 때마다 이전 로그 및 캐시 콘텐츠가 제거됩니다.

## Dispatcher 도구 로그

Dispatcher 도구 로그는 `stdout` 또는 `bin/docker_run` 명령을 통해 사용하거나, 자세한 내용은 `/etc/https/logs`의 Docker 컨테이너에 있습니다.

Dispatcher 도구의 Docker 컨테이너의 로그에 직접 액세스하는 방법에 대한 지침은 [Dispatcher 로그](./logs.md#dispatcher-logs)를 참조하십시오.

## Dispatcher 도구 캐시

### Docker 컨테이너의 로그 액세스

Dispatcher 캐시는 ` /mnt/var/www/html`의 Docker 컨테이너에 직접 액세스할 수 있습니다.

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

### Docker 로그를 로컬 파일 시스템에 복사

Dispatcher 로그는 Docker 컨테이너(`/mnt/var/www/html`)에서 로컬 파일 시스템으로 복사하여 즐겨찾는 도구를 사용하여 확인할 수 있습니다. 이 복사본은 시점 복제본이며 캐시에 실시간 업데이트를 제공하지 않습니다.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

