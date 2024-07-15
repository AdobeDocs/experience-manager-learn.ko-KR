---
title: asset compute 확장성을 위한 로컬 개발 환경 설정
description: Node.js JavaScript 응용 프로그램인 Asset compute 작업자를 개발하려면 Node.js 및 다양한 npm 모듈부터 Docker Desktop 및 Microsoft Visual Studio 코드에 이르기까지 기존의 AEM 개발과는 다른 특정 개발 도구가 필요합니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# 로컬 개발 환경 설정

Adobe Asset compute 프로젝트는 AEM SDK에서 제공하는 로컬 AEM 런타임과 통합될 수 없으며 AEM Maven project archetype을 기반으로 하는 AEM 애플리케이션에서 필요로 하는 것과는 별도로 자체 도구 체인을 사용하여 개발됩니다.

asset compute 마이크로서비스를 확장하려면 로컬 개발자 컴퓨터에 다음 도구를 설치해야 합니다.

## 설치 지침 요약

다음은 설명을 설정하는 요약입니다. 이러한 개발 도구에 대한 자세한 내용은 아래 개별 섹션에 설명되어 있습니다.

1. [Docker Desktop 설치](https://www.docker.com/products/docker-desktop) 및 필요한 Docker 이미지를 가져옵니다.

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

간략한 설치 지침에 대한 자세한 내용은 아래 섹션을 참조하십시오.

## Visual Studio 코드 설치{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download)은(는) Asset compute 작업자를 개발하고 디버깅하는 데 사용됩니다. 다른 [JavaScript 호환 IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)를 사용하여 작업자를 개발할 수 있지만 Visual Studio 코드만 [debug](../test-debug/debug.md) Asset compute 작업자에 통합할 수 있습니다.

이 자습서에서는 Asset compute 확장을 위한 최상의 개발자 환경을 제공하는 Visual Studio Code를 사용한다고 가정합니다.

## Docker Desktop 설치{#docker}

로컬로 [테스트](../test-debug/test.md) 및 [디버그](../test-debug/debug.md) Asset compute 프로젝트를 테스트하는 데 필요하므로 안정적인 최신 [Docker 데스크톱](https://www.docker.com/products/docker-desktop)을(를) 다운로드하여 설치하십시오.

Docker Desktop을 설치한 후 시작하여 명령줄에서 다음 Docker 이미지를 설치합니다.

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows 시스템의 개발자는 위의 이미지에 Linux 컨테이너를 사용하고 있는지 확인해야 합니다. Linux 컨테이너로 전환하는 단계는 [Windows용 도커 설명서](https://docs.docker.com/docker-for-windows/)에 설명되어 있습니다.

## Node.js(및 npm) 설치{#node-js}

Asset compute 작업자는 [Node.js](https://nodejs.org/) 기반이므로 개발 및 빌드하려면 Node.js 10+(및 npm)가 필요합니다.

+ 기존 AEM 개발과 동일한 방식으로 [Node.js(및 npm)](../../local-development-environment/development-tools.md#node-js)을 설치하십시오.

## Adobe I/O CLI 설치{#aio}

[Adobe I/O CLI 설치](../../local-development-environment/development-tools.md#aio-cli) 또는 __aio__&#x200B;은(는) Adobe I/O 기술을 쉽게 사용하고 상호 작용하는 명령줄(CLI) npm 모듈이며 사용자 지정 Asset compute 작업자를 생성하고 로컬에서 개발하는 데 모두 사용됩니다.

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/O CLI Asset compute 플러그인 설치{#aio-asset-compute}

[Adobe I/O CLI Asset compute 플러그 인](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug 설치{#wskdebug}

asset compute 작업자의 로컬 디버깅을 용이하게 하려면 [Apache OpenWhsk 디버그](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈을 다운로드하여 설치하십시오.

_Visual Studio Code 1.48.x+가 있어야 [wskdebug](#wskdebug)이(가) 작동합니다._

```
$ npm install -g @openwhisk/wskdebug
```

## Ngrok 설치{#ngrok}

asset compute 작업자의 로컬 디버깅을 용이하게 하기 위해 로컬 개발 컴퓨터에 대한 공개 액세스를 제공하는 [ngrok](https://www.npmjs.com/package/ngrok) npm 모듈을 다운로드하여 설치하십시오.

```
$ npm install -g ngrok --unsafe-perm=true
```
