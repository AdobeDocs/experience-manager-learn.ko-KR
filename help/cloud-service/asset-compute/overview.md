---
title: Cloud Service으로 AEM용 asset compute 마이크로서비스 확장성
description: 이 자습서에서는 원본 에셋을 원형으로 자른 다음 구성 가능한 대비와 밝기를 적용하여 에셋 변환을 만드는 간단한 Asset compute 작업자 생성 과정을 안내합니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 AEM과 함께 Cloud Service에 사용할 사용자 정의 Asset compute 작업자 만들기, 개발 및 배포에 대해 살펴봅니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# asset compute 마이크로서비스 확장성

AEM의 Cloud Service Asset compute 마이크로서비스는 맞춤형 에셋 표현물을 만들기 위해 AEM에 저장된 에셋의 바이너리 데이터를 읽고 조작하는 데 사용되는 사용자 정의 직원의 개발 및 배포를 지원합니다.

반면 AEM 6.x 사용자 정의 AEM 워크플로우 프로세스는 AEM에서 Cloud Service Asset compute 근로자로서 자산 표현물을 읽고, 변환하고, 다시 쓰는 데 사용되어 이러한 요구 사항을 충족했습니다.

## 무엇을 할 것인가

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

이 자습서에서는 원본 에셋을 원형으로 자른 다음 구성 가능한 대비와 밝기를 적용하여 에셋 변환을 만드는 간단한 Asset compute 작업자 생성 과정을 안내합니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 AEM과 함께 Cloud Service에 사용할 사용자 정의 Asset compute 작업자 만들기, 개발 및 배포에 대해 살펴봅니다.

### 목표 {#objective}

1. asset compute 근로자를 구축하고 배포하기 위해 필요한 계정 및 서비스 제공 및 설정
1. asset compute 프로젝트 만들기 및 구성
1. 사용자 정의 변환을 생성하는 am Asset compute 작업자 개발
1. 사용자 지정 Asset compute 작업자에 대한 테스트를 작성하고 디버깅하는 방법을 알아봅니다.
1. asset compute 작업자를 배포하고 처리 프로필을 통해 AEM을 Cloud Service 작성자 서비스로 통합할 수 있습니다.

## 설정

asset compute 직원 확장을 적절하게 준비하는 방법을 살펴보고, 개발 목적으로 로컬에 설치되어 있는 서비스 및 계정을 프로비저닝하고 구성해야 하는 내용과 계정을 파악합니다.

### 계정 및 서비스 프로비저닝{#accounts-and-services}

다음 계정 및 서비스는 Cloud Service 개발 환경 또는 샌드박스 프로그램으로 AEM의 자습서를 완료하고 Adobe 프로젝트 Firefly 및 Microsoft Azure Blob 저장소에 액세스하려면 프로비저닝 및 액세스 권한이 필요합니다.

+ [계정 및 서비스 제공](./set-up/accounts-and-services.md)

### 로컬 개발 환경

asset compute 프로젝트의 로컬 개발을 위해서는 다음과 같은 기존의 AEM 개발과는 다른 특정 개발자 도구 세트가 필요합니다.Microsoft Visual Studio 코드, Docker Desktop, Node.js 및 지원 npm 모듈입니다.

+ [로컬 개발 환경 설정](./set-up/development-environment.md)

### Adobe 프로젝트 Firefly

asset compute 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefly 프로젝트입니다. 따라서 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefly에 액세스해야 프로젝트를 설정하고 배포할 수 있습니다.

+ [Adobe 프로젝트 Firefly 설정](./set-up/firefly.md)

## 개발

asset compute 프로젝트를 만들고 구성한 다음 맞춤형 에셋 변환을 생성하는 사용자 정의 작업자를 개발하는 방법을 알아봅니다.

### 새 Asset compute 프로젝트 만들기

하나 이상의 Asset compute 작업자가 포함된 asset compute 프로젝트는 대화형 Adobe I/O CLI를 사용하여 생성됩니다. asset compute 프로젝트는 특별히 구조화된 Adobe Project Firefly 프로젝트이며, 이 프로젝트는 Node.js 프로젝트를 차례대로 진행합니다.

+ [새 Asset compute 프로젝트 만들기](./develop/project.md)

### 환경 변수 구성

환경 변수는 로컬 개발을 위해 `.env` 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명 및 클라우드 저장소 자격 증명을 제공하는 데 사용됩니다.

+ [환경 변수 구성](./develop/environment-variables.md)

### manifest.html 구성

asset compute 프로젝트에는 프로젝트 내에 포함된 모든 Asset compute 작업자를 정의하는 매니페스트와 실행을 위해 Adobe I/O Runtime에 배포할 때 사용할 수 있는 리소스가 포함되어 있습니다.

+ [manifest.html 구성](./develop/manifest.md)

### 근로자 개발

asset compute 작업자 개발은 Asset compute 마이크로서비스를 확장하는 핵심입니다. 인기에는 결과 에셋 표현물의 생성을 생성하거나 조정하는 사용자 지정 코드가 포함되어 있기 때문입니다.

+ [asset compute 작업자 개발](./develop/worker.md)

### asset compute 개발 도구 사용

asset compute 개발 도구는 작업자가 생성한 표현물을 배포, 실행 및 미리 볼 수 있는 로컬 웹 하네스를 제공하며 신속한 반복적인 Asset compute 작업자 개발을 지원합니다.

+ [asset compute 개발 도구 사용](./develop/development-tool.md)

## 테스트 및 디버그

사용자 정의 Asset compute 작업자를 테스트하여 자신의 작업을 자신있게 하는 방법을 알아보고, Asset compute 작업자를 디버그하여 사용자 정의 코드가 실행되는 방법을 이해하고 문제를 해결합니다.

### 작업자 테스트

asset compute은 작업자를 위한 테스트 세트를 만들기 위한 테스트 프레임워크를 제공하므로 적절한 행동을 쉽게 보장할 수 있는 테스트를 정의할 수 있습니다.

+ [작업자 테스트](./test-debug/test.md)

### 작업자 디버그

asset compute 작업자는 기존의 `console.log(..)` 출력에서 __VS 코드__ 및 __wskdebug__&#x200B;와의 통합까지 다양한 수준의 디버깅을 제공하므로 개발자는 실시간으로 실행하면서 작업자 코드를 단계별로 실행할 수 있습니다.

+ [작업자 디버그](./test-debug/debug.md)

## 배포

사용자 정의 Asset compute 작업자를 먼저 Adobe I/O Runtime에 배포한 다음 AEM 자산의 처리 프로필을 통해 Cloud Service 작성자로 AEM에서 호출하여 사용자 정의 AEM 작업자를 Cloud Service으로 통합하는 방법을 알아봅니다.

### Adobe I/O Runtime에 배포

asset compute 직원들은 AEM과 Cloud Service을 함께 사용하려면 Adobe I/O Runtime에 배치되어야 한다.

+ [처리 프로필 사용](./deploy/runtime.md)

### AEM 처리 프로필을 통해 직원 통합

Adobe I/O Runtime에 배포되면 Asset compute 작업자는 [자산 처리 프로필](../../assets/configuring/processing-profiles.md)을 통해 AEM에 Cloud Service으로 등록할 수 있습니다. 처리 프로필은 해당 자산에 적용되는 자산 폴더에 적용됩니다.

+ [AEM 처리 프로필과 통합](./deploy/processing-profiles.md)

## 고급

이 요약된 자습서는 이전 장에 확립된 기초 학습 자료를 바탕으로 보다 진보적인 활용 사례를 제공합니다.

+ [Develop a Asset compute metadata ](./advanced/metadata.md) workers that can write metadata back to the

## Codebase on Github

튜토리얼의 코드베이스는 다음 위치의 Github에서 사용할 수 있습니다.

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 마스터 분기

소스 코드에 필수 `.env` 또는 `config.json` 파일이 없습니다. 이러한 항목은 [계정 및 서비스](#accounts-and-services) 정보를 사용하여 추가하고 구성해야 합니다.

## 추가 리소스

다음은 Asset compute 작업자를 개발하기 위한 추가 정보 및 유용한 API 및 SDK를 제공하는 다양한 Adobe 리소스입니다.

### 설명서

+ [asset compute 서비스 설명서](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute 개발 도구 읽어보기](https://github.com/adobe/asset-compute-devtool)
+ [asset compute 예제 근로자](https://github.com/adobe/asset-compute-example-workers)

### API 및 SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe 클라우드 블로브스토어 래퍼 라이브러리](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe 노드 가져오기 재시도 라이브러리](https://github.com/adobe/node-fetch-retry)
+ [asset compute 예제 근로자](https://github.com/adobe/asset-compute-example-workers)
