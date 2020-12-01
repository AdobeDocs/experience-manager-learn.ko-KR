---
title: Dispatcher 도구 디버깅
description: 디스패처 도구는 AEM을 Cloud Services AEM 게시 서비스의 로컬 디스패처로 시뮬레이션하는 데 사용할 수 있는 포함된 Apache 웹 서버 환경을 제공합니다. Dispatcher Tools의 로그 및 캐시 컨텐츠에 대한 디버그는 엔드 투 엔드 AEM 애플리케이션과 지원 캐시 및 보안 구성이 올바른지 확인하는 데 반드시 필요합니다.
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Dispatcher 도구 디버깅

디스패처 도구는 AEM을 Cloud Services AEM 게시 서비스의 로컬 디스패처로 시뮬레이션하는 데 사용할 수 있는 포함된 Apache 웹 서버 환경을 제공합니다.
Dispatcher Tools의 로그 및 캐시 컨텐츠에 대한 디버그는 엔드 투 엔드 AEM 애플리케이션과 지원 캐시 및 보안 구성이 올바른지 확인하는 데 반드시 필요합니다.

>[!NOTE]
>
>디스패처 도구는 컨테이너 기반이므로 다시 시작할 때마다 이전 로그와 캐시 컨텐츠가 삭제됩니다.

## 발송자 도구 로그

디스패처 도구 로그는 `stdout` 또는 `bin/docker_run` 명령을 통해 사용할 수도 있고 자세한 내용은 `/etc/https/logs`의 Docker 컨테이너에서 사용할 수 있습니다.

Dispatcher 도구의 Docker 컨테이너 로그에 직접 액세스하는 방법에 대한 지침은 [Dispatcher 로그](./logs.md#dispatcher-logs)를 참조하십시오.

## 발송자 도구 캐시

### Docker 컨테이너의 로그 액세스

디스패처 캐시는 ` /mnt/var/www/html`의 Docker 컨테이너에서 직접 액세스할 수 있습니다.

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

디스패처 로그는 Docker 컨테이너(`/mnt/var/www/html`)에서 로컬 파일 시스템으로 복사하여 자주 사용하는 도구를 사용하여 검사할 수 있습니다. 이 문서는 지정 시간 복사본이며 캐시에 대한 실시간 업데이트를 제공하지 않습니다.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

