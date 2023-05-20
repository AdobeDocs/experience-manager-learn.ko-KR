---
title: AEMas a Cloud Service 용 asset compute 마이크로서비스 확장성
description: 이 튜토리얼에서는 원본 에셋을 원으로 자르고 구성 가능한 대비 및 밝기를 적용하여 에셋 렌디션을 만드는 간단한 Asset compute 작업자를 만드는 과정을 안내합니다. 작업자 자체는 기본이지만 이 자습서에서는 이 작업자를 사용하여 AEMas a Cloud Service 에 사용할 사용자 지정 Asset compute 작업자를 만들고, 개발하고, 배포합니다.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Asset compute 마이크로서비스 확장성

AEM as Cloud Service의 Asset compute 마이크로 서비스는 사용자 정의 에셋 변환을 만들기 위해 AEM에 저장된 에셋의 이진 데이터를 읽고 조작하는 데 사용되는 사용자 정의 작업자의 개발 및 배포를 지원합니다.

AEM 6.x에서는 사용자 지정 AEM AEM Workflow 프로세스를 사용하여 에셋 렌디션을 읽고, 변형하고, 다시 작성하는 반면, as a Cloud Service Asset compute 작업자는 이 요구 사항을 충족합니다.

## 수행할 작업

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

이 튜토리얼에서는 원본 에셋을 원으로 자르고 구성 가능한 대비 및 밝기를 적용하여 에셋 렌디션을 만드는 간단한 Asset compute 작업자를 만드는 과정을 안내합니다. 작업자 자체는 기본이지만 이 자습서에서는 이 작업자를 사용하여 AEMas a Cloud Service 에 사용할 사용자 지정 Asset compute 작업자를 만들고, 개발하고, 배포합니다.

### 목표 {#objective}

1. asset compute 작업자를 구축하고 배포하는 데 필요한 계정 및 서비스를 프로비저닝하고 설정합니다.
1. asset compute 프로젝트 만들기 및 구성
1. 사용자 지정 렌디션을 생성하는 Asset compute 작업자 개발
1. 에 대한 테스트를 작성하고 사용자 지정 Asset compute 작업자를 디버깅하는 방법을 알아봅니다.
1. asset compute 작업자를 배포하고 처리 프로필을 통해 AEM as a Cloud Service Author 서비스를 통합합니다.

## 설정

asset compute 작업자 확장을 적절히 준비하는 방법을 알아보고, 프로비저닝 및 구성해야 하는 서비스와 계정, 개발을 위해 로컬에 설치되는 소프트웨어를 이해합니다.

### 계정 및 서비스 프로비저닝{#accounts-and-services}

다음 계정 및 서비스는에 대한 프로비저닝 및 액세스 권한이 있어야 자습서, AEM as a Cloud Service 개발 환경 또는 샌드박스 프로그램, App Builder 및 Microsoft Azure Blob Storage에 액세스할 수 있습니다.

+ [계정 및 서비스 프로비저닝](./set-up/accounts-and-services.md)

### 로컬 개발 환경

asset compute 프로젝트의 로컬 개발에는 기존 AEM 개발과는 다른 Microsoft Visual Studio Code, Docker Desktop, Node.js 및 지원 npm 모듈을 포함하는 특정 개발자 도구 세트가 필요합니다.

+ [로컬 개발 환경 설정](./set-up/development-environment.md)

### App Builder

Asset compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서, 설정 및 배포하려면 Adobe Developer 콘솔에서 App Builder에 액세스해야 합니다.

+ [App Builder 설정](./set-up/app-builder.md)

## 개발

asset compute 프로젝트를 만들고 구성한 다음 맞춤형 에셋 렌디션을 생성하는 사용자 지정 작업자를 개발하는 방법에 대해 알아봅니다.

### 새 Asset compute 프로젝트 만들기

하나 이상의 Asset compute 작업자가 포함된 asset compute 프로젝트는 대화형 Adobe I/O CLI를 사용하여 생성됩니다. Asset compute 프로젝트는 특별히 구성된 App Builder 프로젝트로서, Node.js 프로젝트입니다.

+ [새 Asset compute 프로젝트 만들기](./develop/project.md)

### 환경 변수 구성

환경 변수는 `.env` 로컬 개발용 파일이며, 로컬 개발에 필요한 Adobe I/O 자격 증명 및 클라우드 스토리지 자격 증명을 제공하는 데 사용됩니다.

+ [환경 변수 구성](./develop/environment-variables.md)

### manifest.yml 구성

Asset compute 프로젝트에는 프로젝트 내에 포함된 모든 Asset compute 작업자와, 실행을 위해 Adobe I/O Runtime에 배포될 때 사용할 수 있는 리소스를 정의하는 매니페스트가 포함되어 있습니다.

+ [manifest.yml 구성](./develop/manifest.md)

### 작업자 개발

asset compute 작업자 개발은 결과 에셋 렌디션의 생성을 생성하거나 조정하는 사용자 정의 코드를 포함하고 있으므로 Asset compute 마이크로서비스 확장의 핵심입니다.

+ [asset compute 작업자 개발](./develop/worker.md)

### asset compute 개발 도구 사용

asset compute 개발 도구는 작업자 생성 표현물을 배포, 실행 및 미리 보기하기 위한 로컬 웹 하네스를 제공하여 빠르고 반복적인 Asset compute 작업자 개발을 지원합니다.

+ [asset compute 개발 도구 사용](./develop/development-tool.md)

## 테스트 및 디버그

사용자 정의 Asset compute 작업자가 작업에 자신감이 있는지 테스트하고 Asset compute 작업자를 디버깅하여 사용자 정의 코드가 실행되는 방식을 이해하고 문제를 해결하는 방법에 대해 알아봅니다.

### 작업자 테스트

Asset compute은 작업자를 위한 테스트 세트를 만들기 위한 테스트 프레임워크를 제공하여 올바른 동작이 보장되도록 테스트를 정의할 수 있도록 합니다.

+ [작업자 테스트](./test-debug/test.md)

### 작업자 디버깅

Asset compute 작업자는 기존 디버깅을 다양한 수준으로 제공합니다 `console.log(..)` 출력, 와 통합 __VS 코드__ 및  __wskdebug__&#x200B;를 사용하면 개발자가 실시간으로 실행될 때 작업자 코드를 단계별로 진행할 수 있습니다.

+ [작업자 디버깅](./test-debug/debug.md)

## 배포

사용자 정의 Asset compute 작업자를 AEM에 배포한 다음 AEM Assets의 처리 프로필을 통해 AEM Author에서 호출하여 Adobe I/O RuntimeAEM as a Cloud Service 과 as a Cloud Service을 통합하는 방법을 알아봅니다.

### Adobe I/O Runtime에 배포

as a Cloud Service으로 사용하려면 asset compute 작업자를 Adobe I/O RuntimeAEM 에 배포해야 합니다.

+ [처리 프로필 사용](./deploy/runtime.md)

### AEM 처리 프로필을 통해 작업자 통합

Adobe I/O Runtime에 배포되면 Asset compute AEM 작업자를 as a Cloud Service으로 등록할 수 있습니다. [자산 처리 프로필](../../assets/configuring/processing-profiles.md). 처리 프로필은 에셋에 적용되는 에셋 폴더에 적용됩니다.

+ [AEM 처리 프로필과 통합](./deploy/processing-profiles.md)

## 고급

이러한 간략한 튜토리얼은 이전 장에서 수립한 기초 학습을 기반으로 구축되는 보다 고급 사용 사례를 다룹니다.

+ [asset compute 메타데이터 작업자 개발](./advanced/metadata.md) 메타데이터를에 다시 쓸 수 있습니다

## Github의 코드베이스

자습서의 코드베이스는 Github의 다음 위치에서 사용할 수 있습니다.

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 마스터 분기

소스 코드에 필수 항목이 포함되어 있지 않습니다. `.env` 또는 `config.json` 파일. 다음을 사용하여 추가하고 구성해야 합니다. [계정 및 서비스](#accounts-and-services) 정보.

## 추가 리소스

다음은 Asset compute 작업자 개발을 위한 추가 정보와 유용한 API 및 SDK를 제공하는 다양한 Adobe 리소스입니다.

### 설명서

+ [Asset compute 서비스 설명서](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Asset compute 개발 도구 추가 정보](https://github.com/adobe/asset-compute-devtool)
+ [Asset compute 예제 작업자](https://github.com/adobe/asset-compute-example-workers)

### API 및 SDK

+ [ASSET COMPUTE SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset compute 커먼즈](https://github.com/adobe/asset-compute-commons)
   + [Asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe 클라우드 Blobstore 래퍼 라이브러리](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe 노드 가져오기 다시 시도 라이브러리](https://github.com/adobe/node-fetch-retry)
+ [Asset compute 예제 작업자](https://github.com/adobe/asset-compute-example-workers)
