---
title: AEM as a Cloud Service의 Asset Compute 마이크로서비스 확장성
description: 이 튜토리얼에서는 원본 자산을 원형으로 자르고, 구성 가능한 대비와 명도를 적용하여 자산 렌디션을 생성하는 간단한 Asset Compute 작업자를 만드는 방법을 안내합니다. 작업자 자체는 기본적인 것이지만 이 튜토리얼에서는 이를 사용하여 AEM as a Cloud Service에서 사용할 사용자 정의 Asset Compute 작업자를 만들고, 개발하고, 배포하는 과정을 학습할 수 있습니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Asset Compute 마이크로서비스 확장성

AEM as a Cloud Service의 Asset Compute 마이크로서비스는 주로 사용자 정의 자산 렌디션을 생성하기 위해 AEM에 저장된 자산의 바이너리 데이터를 읽고 조작하는 데 사용되는 사용자 정의 작업자의 개발 및 배포를 지원합니다.

AEM 6.x에서는 사용자 정의 AEM Workflow 프로세스를 사용하여 자산 렌디션을 읽고, 변환하고, 다시 쓰는 반면, AEM as a Cloud Service에서는 Asset Compute 작업자가 이러한 요구 사항을 충족합니다.

## 수행할 작업

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

이 튜토리얼에서는 원본 자산을 원형으로 자르고, 구성 가능한 대비와 명도를 적용하여 자산 렌디션을 생성하는 간단한 Asset Compute 작업자를 만드는 방법을 안내합니다. 작업자 자체는 기본적인 것이지만 이 튜토리얼에서는 이를 사용하여 AEM as a Cloud Service에서 사용할 사용자 정의 Asset Compute 작업자를 만들고, 개발하고, 배포하는 과정을 학습할 수 있습니다.

### 목표 {#objective}

1. Asset Compute 작업자를 빌드하고 배포하는 데 필요한 계정 및 서비스를 프로비저닝하고 설정합니다.
1. Asset Compute 프로젝트 만들고 구성합니다.
1. 사용자 정의 렌디션을 생성하는 Asset Compute 작업자를 개발합니다.
1. 사용자 정의 Asset Compute 작업자에 대한 테스트를 작성하고 디버깅하는 방법을 알아봅니다.
1. Asset Compute 작업자를 배포하고 처리 프로필을 통해 이를 AEM as a Cloud Service 작성자 서비스로 통합합니다.

## 설정

Asset Compute 작업자 확장을 위해 필요한 사전 준비 과정을 알아보고, 어떤 서비스 및 계정을 프로비저닝하고 구성해야 하는지, 그리고 로컬 개발을 위해 어떤 소프트웨어를 설치해야 하는지 이해합니다.

### 계정 및 서비스 프로비저닝{#accounts-and-services}

튜토리얼을 완료하려면 다음 계정 및 서비스의 프로비저닝 및 액세스 권한이 필요합니다. AEM as a Cloud Service 개발자 환경 또는 샌드박스 프로그램, App Builder 및 Microsoft Azure Blob Storage 액세스 권한.

+ [계정 및 서비스 프로비저닝](./set-up/accounts-and-services.md)

### 로컬 개발 환경

Asset Compute 프로젝트를 로컬로 개발하려면 기존 AEM 개발과는 다른 특정 개발자 도구 세트가 필요합니다. 여기에는 Microsoft Visual Studio Code, Docker Desktop, Node.js 및 지원 npm 모듈이 포함됩니다.

+ [로컬 개발 환경 설정](./set-up/development-environment.md)

### App Builder

Asset Compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서 설정 및 배포하려면 Adobe Developer Console에서 App Builder에 액세스해야 합니다.

+ [App Builder 설정](./set-up/app-builder.md)

## 개발

Asset Compute 프로젝트를 생성하고 구성하는 방법과 맞춤형 자산 렌디션을 생성하는 사용자 정의 작업자를 개발하는 방법을 알아봅니다.

### 새 Asset Compute 프로젝트 만들기

하나 이상의 Asset Compute 작업자를 포함하는 Asset Compute 프로젝트는 대화형 Adobe I/O CLI를 사용하여 생성됩니다. Asset Compute 프로젝트는 특별히 구조화된 App Builder 프로젝트이며, 이는 결국 Node.js 프로젝트입니다.

+ [새 Asset Compute 프로젝트 만들기](./develop/project.md)

### 환경 변수 구성

환경 변수는 로컬 개발을 위해 `.env` 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명 및 클라우드 스토리지 자격 증명을 제공하는 데 사용됩니다.

+ [환경 변수 구성](./develop/environment-variables.md)

### manifest.html 구성

Asset Compute 프로젝트에는 프로젝트 내에 포함된 모든 Asset Compute 작업자를 정의하는 매니페스트와, 실행을 위해 Adobe I/O Runtime에 배포할 때 사용할 수 있는 리소스가 포함됩니다.

+ [manifest.html 구성](./develop/manifest.md)

### 작업자 개발

Asset Compute 작업자 개발은 Asset Compute 마이크로서비스 확장의 핵심입니다. 작업자에는 결과 자산 렌디션을 생성하거나 그 생성을 조율하는 사용자 정의 코드가 포함됩니다.

+ [Asset Compute 작업자 개발](./develop/worker.md)

### Asset Compute 개발 도구 사용

Asset Compute 개발 도구는 작업자가 생성한 렌디션을 배포하고, 실행하고, 미리 볼 수 있는 로컬 웹 경험을 제공하여 빠르고 반복적인 Asset Compute 작업자 개발을 지원합니다.

+ [Asset Compute 개발 도구 사용](./develop/development-tool.md)

## 테스트 및 디버깅

사용자 정의 Asset Compute 작업자를 테스트하여 작업을 신뢰하고, Asset Compute 작업자를 디버깅하여 사용자 정의 코드가 실행되는 방식을 이해하고 문제를 해결하는 방법을 알아봅니다.

### 작업자 테스트

Asset Compute는 작업자용 테스트 세트를 생성하기 위한 테스트 프레임워크를 제공하여 적절한 동작을 보장하는 테스트를 쉽게 정의할 수 있도록 해 줍니다.

+ [작업자 테스트](./test-debug/test.md)

### 작업자 디버깅

Asset Compute 작업자는 기존 `console.log(..)` 출력부터 __VS 코드__ 및 __wskdebug__&#x200B;와의 통합까지 다양한 수준의 디버깅을 제공하여 개발자가 실시간으로 작업자 코드를 단계별로 실행해 볼 수 있게 합니다.

+ [작업자 디버깅](./test-debug/debug.md)

## 배포

사용자 정의 Asset Compute 작업자를 AEM as a Cloud Service와 통합하는 방법을 알아봅니다. 우선 이를 Adobe I/O Runtime에 배포한 후 AEM Assets의 처리 프로필 통해 AEM as a Cloud Service Author에서 호출합니다.

### Adobe I/O Runtime에 배포

Asset Compute 작업자는 AEM as a Cloud Service로 사용하려면 Adobe I/O Runtime에 배포되어야 합니다.

+ [처리 프로필 사용](./deploy/runtime.md)

### AEM 처리 프로필을 통해 작업자 통합

Adobe I/O Runtime에 배포한 후에는 AEM as a Cloud Service에서 [Assets 처리 프로필](../../assets/configuring/processing-profiles.md)을 통해 Asset Compute 작업자를 등록할 수 있습니다. 처리 프로필은 자산 폴더에 적용되어 해당 폴더 내의 자산에 적용됩니다.

+ [AEM 처리 프로필과 통합](./deploy/processing-profiles.md)

## 고급

이 간략화된 튜토리얼에서는 앞에서 다룬 기본적인 내용을 바탕으로 보다 고급 사용 사례를 다룹니다.

+ 메타데이터를 다시 작성할 수 있는 [Asset Compute 메타데이터 작업자를 개발](./advanced/metadata.md)합니다.

## Github의 코드 베이스

튜토리얼의 코드 베이스는 Github에서 확인할 수 있습니다.

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

소스 코드에는 필수 `.env` 또는 `config.json` 파일이 포함되어 있지 않습니다. 이들 파일은 [계정 및 서비스](#accounts-and-services) 정보를 사용하여 추가하고 구성해야 합니다.

## 추가 리소스

다음은 Asset Compute 작업자를 개발하는 데 필요한 추가 정보와 유용한 API 및 SDK를 제공하는 다양한 Adobe 리소스입니다.

### 설명서

+ [Asset Compute Service 설명서](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ko)
+ [Asset Compute 개발 도구 추가 정보](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute 예제 작업자](https://github.com/adobe/asset-compute-example-workers)

### API 및 SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper 라이브러리](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch Retry 라이브러리](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute 예제 작업자](https://github.com/adobe/asset-compute-example-workers)
