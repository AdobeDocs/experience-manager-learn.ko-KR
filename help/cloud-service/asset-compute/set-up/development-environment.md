---
title: asset compute 확장성을 위한 로컬 개발 환경 설정
description: Node.js JavaScript 응용 프로그램인 Asset compute 작업자를 개발하려면 Node.js 및 다양한 npm 모듈부터 Docker Desktop 및 Microsoft Visual Studio 코드에 이르기까지 기존의 AEM 개발과는 다른 특정 개발 도구가 필요합니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: 53c20b9774c15b04a1c78c7c0c7b61a60996bf60
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---


# 로컬 개발 환경 설정

Adobe Asset compute 프로젝트는 AEM SDK에서 제공하는 로컬 AEM 런타임과 통합할 수 없으며 AEM Maven 프로젝트 원형을 기반으로 AEM 애플리케이션에서 필요로 하는 것과 별도로 고유한 도구 체인을 사용하여 개발됩니다.

asset compute 마이크로서비스를 확장하려면 로컬 개발자 시스템에 다음 도구를 설치해야 합니다.

## 요약된 설정 지침

다음은 리지 설정 지침입니다. 이러한 개발 도구에 대한 자세한 내용은 아래의 개별 섹션에서 설명합니다.

1. [Docker Desktop](https://www.docker.com/products/docker-desktop) 을 설치하고 필요한 Docker 이미지를 가져옵니다.

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio 코드 설치](https://code.visualstudio.com/download)
1. [Node.js 10+ 설치](../../local-development-environment/development-tools.md#node-js)
1. 명령줄에서 필요한 npm 모듈 및 Adobe I/O CLI 플러그인을 설치합니다.

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

설치 지침에 대한 자세한 내용은 아래 섹션을 참조하십시오.

## Visual Studio 코드 설치{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) 코드는 Asset compute 작업자를 개발 및 디버깅하는 데 사용됩니다. 다른 [JavaScript 호환 IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)를 사용하여 작업자를 개발할 수 있지만 Visual Studio 코드만 [debug](../test-debug/debug.md) Asset compute 작업자에 통합할 수 있습니다.

_Visual Studio Code 1.48.x+는  [](#wskdebug) wskdebugger가 작동하려면 필요합니다._

이 자습서에서는 Asset compute 확장에 가장 적합한 개발자 환경을 제공하므로 Visual Studio 코드를 사용하는 것으로 가정합니다.

## Docker Desktop 설치{#docker}

[테스트](../test-debug/test.md) 및 [디버그](../test-debug/debug.md) Asset compute 프로젝트를 로컬로 구현하려면 필요한 최신 안정적이고 안정적인 [Docker Desktop](https://www.docker.com/products/docker-desktop)을 다운로드하여 설치합니다.

Docker Desktop을 설치한 후 이를 시작하고 명령줄에서 다음 Docker 이미지를 설치합니다.

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows 시스템의 개발자는 위의 이미지에 Linux 컨테이너를 사용하고 있는지 확인해야 합니다. Linux 컨테이너로 전환하는 단계는 [Docker for Windows 설명서](https://docs.docker.com/docker-for-windows/)에 설명되어 있습니다.

## Node.js 설치(및 npm){#node-js}

asset compute 작업자는 [Node.js](https://nodejs.org/) 기반이므로 개발 및 빌드하려면 Node.js 10+(및 npm)가 필요합니다.

+ [기존 AEM 개발에서와 동일한 방식으로 Node.js(및 npm)](../../local-development-environment/development-tools.md#node-js) 를 설치합니다.

## Adobe I/O CLI 설치{#aio}

[Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) 또는 Adobe I/O 기술 ____ 의 사용 및 상호 작용을 용이하게 하고 사용자 정의 Asset compute 작업자를 생성하고 로컬에서 개발하는 데 사용되는 CLI(Aiois an Command-Line) npm 모듈을 설치합니다.

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/O CLI Asset compute 플러그인 설치{#aio-asset-compute}

[Adobe I/O CLI Asset compute 플러그인](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug 설치{#wskdebug}

asset compute 작업자의 로컬 디버깅을 용이하게 하기 위해 [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈을 다운로드하여 설치합니다.

_Visual Studio Code 1.48.x+는  [](#wskdebug) wskdebugger가 작동하려면 필요합니다._

```
$ npm install -g @openwhisk/wskdebug
```

## 설치 네트워크{#ngrok}

로컬 개발 컴퓨터에 대한 공개 액세스를 제공하는 [npm 모듈을 다운로드하여 설치하여 Asset compute 작업자의 로컬 디버깅을 용이하게 합니다.](https://www.npmjs.com/package/ngrok)

```
$ npm install -g ngrok --unsafe-perm=true
```
