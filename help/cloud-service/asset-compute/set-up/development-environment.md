---
title: asset compute 확장성을 위한 로컬 개발 환경 설정
description: Node.js JavaScript 애플리케이션인 Asset compute 작업자를 개발하려면 Node.js 및 다양한 npm 모듈, Docker Desktop 및 Microsoft Visual Studio 코드에 이르기까지 기존의 AEM 개발과는 다른 특정 개발 도구가 필요합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# 로컬 개발 환경 설정

Adobe Asset compute 프로젝트는 AEM SDK에서 제공하는 로컬 AEM 런타임과 통합할 수 없으며 AEM Maven 프로젝트 원형을 기반으로 하는 AEM 애플리케이션에서 필요로 하는 것과 별도로 고유한 도구 체인을 사용하여 개발됩니다.

asset compute 마이크로서비스를 확장하려면 로컬 개발자 컴퓨터에 다음 도구를 설치해야 합니다.

## 요약된 안내

다음은 능선 설정 지침입니다. 이러한 개발 도구에 대한 자세한 내용은 아래 개별 섹션에 설명되어 있습니다.

1. [Docker ](https://www.docker.com/products/docker-desktop) Desktop을 설치하고 필요한 Docker 이미지를 가져옵니다.

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Visual Studio 코드 설치](https://code.visualstudio.com/download)
1. [Node.js 10+ 설치](../../local-development-environment/development-tools.md#node-js)
1. 명령줄에서 필수 npm 모듈 및 Adobe I/O CLI 플러그인을 설치합니다.

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

요약된 설치 지침에 대한 자세한 내용은 아래 섹션을 참조하십시오.

## Visual Studio 코드 설치{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) 코드는 Asset compute 작업자를 개발 및 디버깅하는 데 사용됩니다. 다른 [JavaScript 호환 IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)는 작업자 개발에 사용할 수 있지만 Visual Studio 코드만 [debug](../test-debug/debug.md) Asset compute 작업자에 통합할 수 있습니다.

_wskdebugto가 작동하려면 Visual Studio 코드 1.48.x+가  [](#wskdebug) 필요합니다._

이 자습서에서는 Asset compute 확장을 위한 최고의 개발자 환경을 제공하므로 Visual Studio 코드를 사용하는 것을 가정합니다.

## Docker Desktop 설치{#docker}

로컬에서 [test](../test-debug/test.md) 및 [debug](../test-debug/debug.md) Asset compute 프로젝트를 사용하려면 이 요구 사항이 필요하므로 안정적이고 최신 [Docker Desktop](https://www.docker.com/products/docker-desktop)을 다운로드하여 설치하십시오.

Docker Desktop을 설치한 후 이를 시작하고 명령줄에서 다음 Docker 이미지를 설치합니다.

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows 시스템 개발자는 위의 이미지에 Linux 컨테이너를 사용해야 합니다. Linux 컨테이너로 전환하는 방법은 [Docker for Windows 설명서](https://docs.docker.com/docker-for-windows/)에 설명되어 있습니다.

## Node.js(및 npm){#node-js} 설치

asset compute 작업자는 [Node.js](https://nodejs.org/) 기반이므로 개발 및 빌드에 Node.js 10+(및 npm)가 필요합니다.

+ [기존의 AEM 개발에서와 ](../../local-development-environment/development-tools.md#node-js) 같은 방식으로 Node.js(및 npm)를 설치합니다.

## Adobe I/O CLI 설치{#aio}

[Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) 또는  ____ CLI(Command-Line) npm 모듈을 설치하여 Adobe I/O 기술을 사용하고 상호 작용하며 사용자 정의 Asset compute 작업자를 생성 및 로컬에서 개발하는 데 사용됩니다.

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/O CLI Asset compute 플러그인{#aio-asset-compute} 설치

[Adobe I/O CLI Asset compute 플러그인](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug{#wskdebug} 설치

asset compute 근로자의 로컬 디버깅을 용이하게 하려면 [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈을 다운로드하여 설치하십시오.

_wskdebugto가 작동하려면 Visual Studio 코드 1.48.x+가  [](#wskdebug) 필요합니다._

```
$ npm install -g @openwhisk/wskdebug
```

## ngrok{#ngrok} 설치

로컬 개발 시스템에 대한 공용 액세스를 제공하는 [npm 모듈을 다운로드하여 설치하면 Asset compute 작업자의 로컬 디버깅을 용이하게 할 수 있습니다.](https://www.npmjs.com/package/ngrok)

```
$ npm install -g ngrok --unsafe-perm=true
```
