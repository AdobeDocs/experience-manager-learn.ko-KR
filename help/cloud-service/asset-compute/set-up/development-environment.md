---
title: 에셋 계산 확장성을 위한 로컬 개발 환경 설정
description: Node.js JavaScript 애플리케이션인 자산 계산 작업자를 개발하려면 Node.js 및 다양한 npm 모듈부터 Docker Desktop 및 Microsoft Visual Studio 코드에 이르기까지 기존의 AEM 개발과는 다른 특정 개발 도구가 필요합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 53e4235c55d890765e9f13ffeb37a2c805fb307b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# 로컬 개발 환경 설정

Adobe 에셋 컴퓨팅 애플리케이션은 AEM SDK에서 제공하는 로컬 AEM 런타임과 통합할 수 없으며 AEM Maven 프로젝트 원형을 기반으로 하는 AEM 애플리케이션에서 요구하는 툴 체인과 별도로 고유한 도구 체인을 사용하여 개발됩니다.

자산 계산 마이크로서비스를 확장하려면 로컬 개발자 컴퓨터에 다음 도구를 설치해야 합니다.

## 요약된 안내

다음은 능선 설정 지침입니다. 이러한 개발 도구에 대한 자세한 내용은 아래 개별 섹션에 설명되어 있습니다.

1. [Docker Desktop을](https://www.docker.com/products/docker-desktop) 설치하고 필요한 Docker 이미지를 가져옵니다.

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio 코드 설치](https://code.visualstudio.com/download)
1. [Node.js 10+ 설치](../../local-development-environment/development-tools.md#node-js)
1. 명령줄에서 필요한 npm 모듈 및 Adobe I/O CLI 플러그인을 설치합니다.

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Visual Studio 코드 설치{#vscode}

[Microsoft Visual Studio 코드는](https://code.visualstudio.com/download) Asset Compute 응용 프로그램을 개발 및 디버깅하는 데 사용됩니다. 다른 [JavaScript 호환 IDE를](../../local-development-environment/development-tools.md#set-up-the-development-ide) 사용하여 응용 프로그램을 개발할 수 있지만 Visual Studio 코드만 통합하여 [자산 계산 응용](../test-debug/debug.md) 프로그램을 디버깅할 수 있습니다.

_Visual Studio 코드 1.48.x+가[필요합니다](#wskdebug)._

이 자습서에서는 Visual Studio 코드를 사용하여 에셋 계산 확장을 위한 최고의 개발자 환경을 제공합니다.

## Docker Desktop 설치{#docker}

로컬에서 에셋 계산 프로젝트를 [테스트](https://www.docker.com/products/docker-desktop)및 [디버깅하는](../test-debug/test.md) 데 [필요하므로](../test-debug/debug.md) 안정적인 최신 Docker Desktop을 다운로드하여설치할 수 있습니다.

Docker Desktop을 설치한 후 이를 시작하고 명령줄에서 다음 Docker 이미지를 설치합니다.

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows 시스템 개발자는 위의 이미지에 Linux 컨테이너를 사용해야 합니다. Linux 컨테이너로 전환하는 방법은 Windows용 [Docker 설명서에 설명되어 있습니다](https://docs.docker.com/docker-for-windows/).

## Node.js(및 npm) 설치{#node-js}

자산 계산 작업자는 [Node.js](https://nodejs.org/) 애플리케이션이므로 개발 및 빌드에 Node.js 10+(및 npm)가 필요합니다.

+ [기존 AEM 개발에서와 동일한 방식으로 Node.js(및 npm)](../../local-development-environment/development-tools.md#node-js) 를 설치합니다.

## Adobe I/O CLI 설치{#aio}

[Adobe I/O CLI를](../../local-development-environment/development-tools.md#aio-cli)설치하거나 __aio는 Adobe I/O 기술을 사용하고 상호 작용을 용이하게 하는 명령줄(CLI) npm 모듈이며 사용자 정의 Asset Compute Workers를 생성 및 로컬에서 개발하는 데 사용됩니다__ .

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/O CLI Asset Compute 플러그인 설치{#aio-asset-compute}

Adobe [I/O CLI Asset Compute plugin](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug 설치{#wskdebug}

Asset Compute 응용 프로그램의 [로컬 디버깅을 용이하게 하기 위해 Apache OpenWhisk 디버그](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈을 다운로드하여 설치합니다.

_Visual Studio 코드 1.48.x+가[필요합니다](#wskdebug)._

```
$ npm install -g @openwhisk/wskdebug
```

## 설치{#ngrok}

로컬 개발 시스템에 대한 [공개 액세스를 제공하는](https://www.npmjs.com/package/ngrok) ngroup npm 모듈을 다운로드하여 설치하면 Asset Compute 응용 프로그램의 로컬 디버깅을 쉽게 수행할 수 있습니다.

```
$ npm install -g ngrok --unsafe-perm=true
```
