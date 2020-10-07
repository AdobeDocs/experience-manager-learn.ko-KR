---
title: Cloud Service의 AEM용 에셋 컴퓨팅 마이크로서비스 확장성
description: 이 자습서에서는 원본 자산을 서클로 자르면 자산 변환을 만들고 구성 가능한 대비와 밝기를 적용하는 간단한 에셋 계산 작업자 만들기에 대해 설명합니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 AEM과 함께 Cloud Service으로 사용할 사용자 정의 에셋 컴퓨팅 작업자 만들기, 개발 및 배포를 탐색합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 0%

---


# 자산 컴퓨팅 마이크로서비스 확장성

AEM as Cloud Service(Asset Compute microservices)는 사용자 정의 에셋 변환을 만들기 위해 일반적으로 AEM에 저장된 에셋의 이진 데이터를 읽고 조작하는 데 사용되는 사용자 정의 작업자의 개발 및 배포를 지원합니다.

반면 AEM 6.x 사용자 정의 AEM 워크플로우 프로세스는 자산 표현물을 읽고, 변환하고, 다시 쓰는 데 사용되었지만, AEM에서는 Cloud Service 자산 계산 직원이 이러한 요구를 충족합니다.

## 무엇을 할 것인가

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

이 자습서에서는 원본 자산을 서클로 자르면 자산 변환을 만들고 구성 가능한 대비와 밝기를 적용하는 간단한 에셋 계산 작업자 만들기에 대해 설명합니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 AEM과 함께 Cloud Service으로 사용할 사용자 정의 에셋 컴퓨팅 작업자 만들기, 개발 및 배포를 탐색합니다.

### 목표 {#objective}

1. 자산 계산 작업자를 작성하고 배포하기 위해 필요한 계정 및 서비스를 제공 및 설정합니다.
1. 자산 계산 프로젝트 만들기 및 구성
1. 사용자 지정 변환을 생성하는 am Asset Compute worker 개발
1. 사용자 지정 자산 계산 작업자에 대한 테스트를 작성하고 디버깅하는 방법을 알아봅니다.
1. Asset Compute Worker를 배포하고 처리 프로필을 통해 AEM을 Cloud Service 작성자 서비스로 통합합니다.

## 설정

에셋 컴퓨팅 작업자 확장을 적절하게 준비하는 방법을 알아보고, 배포하고 구성해야 하는 서비스 및 계정 및 개발을 위해 로컬로 설치되는 소프트웨어를 파악합니다.

### 계정 및 서비스 프로비저닝{#accounts-and-services}

다음 계정 및 서비스는 Cloud Service 개발 환경 또는 샌드박스 프로그램으로 AEM의 자습서를 완료하고 Adobe 프로젝트 Firefly 및 Microsoft Azure Blob 저장소에 액세스하려면 프로비저닝 및 액세스 권한이 필요합니다.

+ [계정 및 서비스 제공](./set-up/accounts-and-services.md)

### 로컬 개발 환경

에셋 컴퓨팅 프로젝트의 로컬 개발을 위해서는 다음과 같은 기존의 AEM 개발과는 다른 특정 개발자 도구 세트가 필요합니다.Microsoft Visual Studio 코드, Docker Desktop, Node.js 및 지원 npm 모듈입니다.

+ [로컬 개발 환경 설정](./set-up/development-environment.md)

### Adobe 프로젝트 Firefly

에셋 컴퓨팅 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefly 프로젝트입니다. 이와 같이 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefly에 액세스해야 설정 및 배포할 수 있습니다.

+ [Adobe 프로젝트 Firefly 설정](./set-up/firefly.md)

## 개발

자산 계산 프로젝트를 만들고 구성한 다음 맞춤형 자산 변환을 생성하는 사용자 지정 작업자를 개발하는 방법을 알아봅니다.

### 새 자산 계산 프로젝트 만들기

하나 이상의 자산 계산 작업자가 포함된 자산 계산 프로젝트는 대화형 Adobe I/O CLI를 사용하여 생성됩니다. 에셋 컴퓨팅 프로젝트는 특별히 구조화된 Adobe 프로젝트 Firefly 프로젝트이며, 이 프로젝트는 Node.js 프로젝트를 차례대로 진행합니다.

+ [새 자산 계산 프로젝트 만들기](./develop/project.md)

### 환경 변수 구성

환경 변수는 로컬 개발을 위해 `.env` 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명과 클라우드 스토리지 자격 증명을 제공하는 데 사용됩니다.

+ [환경 변수 구성](./develop/environment-variables.md)

### manifest.html 구성

자산 계산 프로젝트에는 프로젝트 내에 포함된 모든 자산 계산 작업자를 정의하는 매니페스트와 실행을 위해 Adobe I/O Runtime에 배포할 때 사용할 수 있는 리소스가 포함되어 있습니다.

+ [manifest.html 구성](./develop/manifest.md)

### 근로자 개발

인부에는 결과 자산 표현물의 생성을 생성 또는 조정하는 사용자 지정 코드가 포함되어 있으므로, 자산 계산 작업자 개발은 자산 계산 마이크로서비스를 확장하기 위한 핵심입니다.

+ [자산 계산 작업자 개발](./develop/worker.md)

### 자산 계산 개발 도구 사용

자산 계산 개발 도구는 작업자가 생성한 표현물을 배포, 실행 및 미리 볼 수 있는 로컬 웹 도구를 제공하며, 속성 계산 작업자 개발을 지원합니다.

+ [자산 계산 개발 도구 사용](./develop/development-tool.md)

## 테스트 및 디버그

사용자 지정 자산 계산 작업자를 테스트하여 자신의 작업에 확신을 갖는 방법을 알아보고, 자산 계산 작업자를 디버그하여 사용자 지정 코드가 어떻게 실행되는지 파악하고 문제를 해결하는 방법을 알아봅니다.

### 작업자 테스트

자산 계산에서는 작업자를 위한 테스트 세트를 만들기 위한 테스트 프레임워크를 제공하므로 적절한 행동을 쉽게 할 수 있도록 테스트를 정의할 수 있습니다.

+ [작업자 테스트](./test-debug/test.md)

### 작업자 디버그

에셋 컴퓨팅 작업자는 기존 `console.log(..)` 출력에서 __VS 코드__ 및 __wskdebug와의 통합__&#x200B;등 다양한 수준의 디버깅을 제공하므로 개발자는 실시간으로 작업자 코드를 단계별로 실행할 수 있습니다.

+ [작업자 디버그](./test-debug/debug.md)

## 배포

사용자 지정 에셋 컴퓨팅 작업자를 Cloud Service으로 통합한 다음, 먼저 Adobe I/O Runtime에 배포한 다음 AEM Assets 처리 프로필을 통해 Cloud Service 작성자로 AEM에서 불러오면 됩니다.

### Adobe I/O Runtime에 배포

AEM과 Cloud Service을 함께 사용하려면 자산 계산 작업자를 Adobe I/O Runtime에 배치해야 합니다.

+ [처리 프로필 사용](./deploy/runtime.md)

### AEM 처리 프로필을 통해 직원 통합

Adobe I/O Runtime에 배포되면 자산 계산 작업자는 자산 처리 프로필을 통해 AEM에 Cloud Service으로 [등록할 수 있습니다](../../assets/configuring/processing-profiles.md). 처리 프로필은 해당 자산에 적용되는 Assets 폴더에 적용됩니다.

+ [AEM 처리 프로필과 통합](./deploy/processing-profiles.md)

## Github에 대한 자습서 코드

자습서 코드 베이스는 다음 위치의 Github에서 사용할 수 있습니다.

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 마스터 분기

소스 코드에 필수 `.env` 또는 `config.json` 파일이 없습니다. 계정 및 서비스 [](#accounts-and-services) 정보를 사용하여 이러한 정보를 추가하고 구성해야 합니다.

## 추가 리소스

다음은 자산 계산 작업자 개발을 위한 추가 정보 및 유용한 API 및 SDK를 제공하는 다양한 Adobe 리소스입니다.

### 설명서

+ [자산 계산 서비스 설명서](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [자산 계산 개발 도구 읽어보기](https://github.com/adobe/asset-compute-devtool)

### 기타 코드 샘플

+ [자산 계산 예제 근로자](https://github.com/adobe/asset-compute-example-workers)

### API 및 SDK

+ [자산 계산 SDK](https://github.com/adobe/asset-compute-sdk)
   + [에셋 컴퓨팅 공용](https://github.com/adobe/asset-compute-commons)
+ [Adobe 클라우드 블로브스토어 래퍼 라이브러리](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe 노드 가져오기 재시도 라이브러리](https://github.com/adobe/node-fetch-retry)
