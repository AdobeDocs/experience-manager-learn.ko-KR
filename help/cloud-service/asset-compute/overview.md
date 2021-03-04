---
title: Cloud Service의 AEM용 asset compute 마이크로서비스 확장성
description: 이 자습서에서는 원래 에셋을 원형으로 잘라내고 구성 가능한 대비와 명도를 적용하여 에셋 변환을 만드는 간단한 Asset compute 작업자를 만드는 방법을 살펴봅니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 이 프레임워크를 사용하여 AEM과 함께 Cloud Service에 사용할 사용자 정의 Asset compute 작업자 만들기, 개발 및 배포를 살펴봅니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 통합, 개발
role: 개발자
level: 중간, 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# asset compute 마이크로서비스 확장성

AEM Cloud Service의 Asset compute 마이크로서비스로서 사용자 정의 에셋 변환을 만들기 위해 AEM에 저장된 에셋의 이진 데이터를 읽고 조작하는 데 사용되는 사용자 정의 워커의 개발 및 배포를 지원합니다.

AEM 6.x 사용자 정의 AEM 워크플로우 프로세스는 자산 변환을 읽고, 변환하고, 다시 쓰는 데 사용되었지만, AEM에서는 Cloud Service Asset compute 근로자로서 이러한 요구 사항을 충족합니다.

## 무엇을 할 것인가

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

이 자습서에서는 원래 에셋을 원형으로 잘라내고 구성 가능한 대비와 명도를 적용하여 에셋 변환을 만드는 간단한 Asset compute 작업자를 만드는 방법을 살펴봅니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 이 프레임워크를 사용하여 AEM과 함께 Cloud Service에 사용할 사용자 정의 Asset compute 작업자 만들기, 개발 및 배포를 살펴봅니다.

### 목표 {#objective}

1. asset compute Worker를 구축 및 배포하는 데 필요한 계정 및 서비스 제공 및 설정
1. asset compute 프로젝트 만들기 및 구성
1. 사용자 정의 변환을 생성하는 am Asset compute 작업자 개발
1. 사용자 지정 Asset compute 워커에 대한 테스트를 작성하고 디버깅하는 방법을 알아봅니다.
1. asset compute Worker를 배포하고 처리 프로필을 통해 AEM을 Cloud Service 작성자 서비스로 통합할 수 있습니다.

## 설정

asset compute 직원 확장을 적절히 준비하고, 어떤 서비스 및 계정이 제공되고 구성되어야 하는지 파악하고, 개발을 위해 로컬에 설치된 소프트웨어를 이해할 수 있습니다.

### 계정 및 서비스 제공{#accounts-and-services}

다음 계정 및 서비스는 Cloud Service 개발 환경 또는 샌드박스 프로그램으로 AEM 자습서를 완료하고 Adobe Project Firefly 및 Microsoft Azure Blob 저장소에 액세스하려면 프로비저닝 및 액세스 권한이 필요합니다.

+ [계정 및 서비스 제공](./set-up/accounts-and-services.md)

### 로컬 개발 환경

asset compute 프로젝트의 로컬 개발을 위해서는 다음과 같은 기존의 AEM 개발과는 다른 특정 개발자 도구 세트가 필요합니다.Microsoft Visual Studio 코드, Docker Desktop, Node.js 및 지원 npm 모듈입니다.

+ [로컬 개발 환경 설정](./set-up/development-environment.md)

### Adobe 프로젝트 Firefly

asset compute 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefox 프로젝트입니다. 따라서 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefox에 액세스해야 설정 및 배포할 수 있습니다.

+ [Adobe 프로젝트 Firefox 설정](./set-up/firefly.md)

## 개발

asset compute 프로젝트를 만들고 구성한 다음 맞춤화된 에셋 변환을 생성하는 사용자 정의 워커를 개발하는 방법을 알아봅니다.

### 새 Asset compute 프로젝트 만들기

하나 이상의 Asset compute 작업자가 포함된 asset compute 프로젝트는 대화형 Adobe I/O CLI를 사용하여 생성됩니다. asset compute 프로젝트는 특별히 구조화된 Adobe Project Firefly 프로젝트입니다. 이 프로젝트는 Node.js 프로젝트를 차례로 수행합니다.

+ [새 Asset compute 프로젝트 만들기](./develop/project.md)

### 환경 변수 구성

환경 변수는 로컬 개발을 위해 `.env` 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명과 클라우드 저장소 자격 증명을 제공하는 데 사용됩니다.

+ [환경 변수 구성](./develop/environment-variables.md)

### manifest.yml 구성

asset compute 프로젝트에는 프로젝트에 포함된 모든 Asset compute 작업자를 정의하는 매니페스트뿐만 아니라 실행을 위해 Adobe I/O Runtime에 배포할 때 사용할 수 있는 리소스가 포함되어 있습니다.

+ [manifest.yml 구성](./develop/manifest.md)

### 근로자 개발

워커는 결과 에셋 표현물 생성을 생성하거나 조정하는 사용자 지정 코드를 포함하므로 Asset compute Microservices를 확장하기 위한 핵심입니다.

+ [asset compute 작업자 개발](./develop/worker.md)

### asset compute 개발 도구 사용

asset compute 개발 도구는 작업자가 생성한 표현물을 배포, 실행 및 미리 볼 수 있는 로컬 웹 도구를 제공하며, 신속한 반복적인 Asset compute 작업자 개발을 지원합니다.

+ [asset compute 개발 도구 사용](./develop/development-tool.md)

## 테스트 및 디버그

사용자 정의 Asset compute 작업자를 테스트하여 자신의 작업을 자신있게 하는 방법을 알아보고, 사용자 정의 코드가 실행되는 방법을 이해하고 문제를 해결하도록 Asset compute 작업자를 디버그합니다.

### 작업자 테스트

asset compute은 작업자를 위한 테스트 세트를 만들기 위한 테스트 프레임워크를 제공하므로 적절한 비헤이비어를 쉽게 보장하는 테스트를 정의할 수 있습니다.

+ [작업자 테스트](./test-debug/test.md)

### 작업자 디버그

asset compute 작업자는 기존 `console.log(..)` 출력에서 __VS 코드__ 및 __wskdebug__&#x200B;와의 통합으로 다양한 수준의 디버깅을 제공하므로 개발자는 실시간으로 실행하면서 작업자 코드를 단계별로 실행할 수 있습니다.

+ [작업자 디버그](./test-debug/debug.md)

## 배포

사용자 지정 Asset compute 작업자를 Cloud Service으로 통합한 다음 먼저 Adobe I/O Runtime에 배포한 다음 AEM Assets의 처리 프로필을 통해 Cloud Service 작성자로 AEM에서 호출함으로써 사용자 지정 AEM 작업자를으로 통합하는 방법을 알아봅니다.

### Adobe I/O Runtime에 배포

Cloud Service으로 AEM과 함께 사용하려면 asset compute 작업자를 Adobe I/O Runtime에 배포해야 합니다.

+ [처리 프로필 사용](./deploy/runtime.md)

### AEM 처리 프로필을 통해 직원 통합

Adobe I/O Runtime에 배포되면 Asset compute 근로자는 [자산 처리 프로필](../../assets/configuring/processing-profiles.md)을 통해 AEM에 Cloud Service으로 등록할 수 있습니다. 처리 프로필은 포함된 자산에 적용되는 자산 폴더에 적용됩니다.

+ [AEM 처리 프로필과 통합](./deploy/processing-profiles.md)

## 고급

이 요약된 자습서는 이전 장에 확립된 기본 학습 내용에 따라 보다 진보된 활용 사례를 제공합니다.

+ [메타데이터를 ](./advanced/metadata.md) 다시

## Github 온 Codebase

자습서의 코드 베이스는 Github(

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 마스터 분기

소스 코드에 필수 `.env` 또는 `config.json` 파일이 없습니다. [ 계정 및 서비스](#accounts-and-services) 정보를 사용하여 이러한 항목을 추가하고 구성해야 합니다.

## 추가 리소스

다음은 Asset compute 작업자 개발을 위한 추가 정보 및 유용한 API 및 SDK를 제공하는 다양한 Adobe 리소스입니다.

### 설명서

+ [asset compute 서비스 설명서](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute 개발 도구 읽어보기](https://github.com/adobe/asset-compute-devtool)
+ [asset compute 예제 근로자](https://github.com/adobe/asset-compute-example-workers)

### API 및 SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute 커먼스](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe 클라우드 Blobstore 래퍼 라이브러리](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe 노드 가져오기 다시 시도 라이브러리](https://github.com/adobe/node-fetch-retry)
+ [asset compute 예제 근로자](https://github.com/adobe/asset-compute-example-workers)
