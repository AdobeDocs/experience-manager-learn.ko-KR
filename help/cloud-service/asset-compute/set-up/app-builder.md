---
title: asset compute 확장성을 위한 App Builder 설정
description: asset compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서, 설정 및 배포하려면 Adobe 개발자 콘솔에서 App Builder에 액세스해야 합니다.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# 앱 빌더 설정

asset compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서, 설정 및 배포하려면 Adobe 개발자 콘솔에서 App Builder에 액세스해야 합니다.

## Adobe 개발자 콘솔에서 App Builder 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_앱 빌더 설정 클릭스루(오디오 없음)_

1. 에 로그인합니다. [Adobe 개발자 콘솔](https://console.adobe.io) 프로비저닝된 사용자와 연결된 Adobe ID 사용 [계정 및 서비스](./accounts-and-services.md). 자신이 __시스템 관리자__ 또는 __개발자 역할__ 를 입력합니다.
1. 탭하여 App Builder 프로젝트 만들기 __새 프로젝트 만들기 > 템플릿에서 프로젝트 > 앱 빌더__

   _다음 중 한 경우__&#x200B;새 프로젝트 만들기&#x200B;__버튼 또는__ App Builder __유형을 사용할 수 없습니다. 이는 Adobe 조직이 [App Builder로 프로비저닝됨](#request-adobe-project-app-builder)._

   + __프로젝트 제목__: `WKND AEM Asset Compute`
   + __앱 이름__: `wkndAemAssetCompute<YourName>`
      + 다음 __앱 이름__ 모든 FApp Builder 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사나 조직의 이름을 접두사로 사용하고 의미 있는 접미사를 사용하여 포스트픽스를 수정하는 것은 다음과 같은 좋은 방법입니다. `wkndAemAssetCompute`.
      + 자체 사용을 위해 이름을 __앱 이름__, 예 `wkndAemAssetComputeJaneDoe` 다른 App Builder 프로젝트와의 충돌을 피하려면 다음을 수행하십시오.
   + 아래 __작업 공간__ 라는 새 환경 추가 `Development`
   + 아래 __Adobe I/O Runtime__ 확인합니다. __각 작업 공간에 런타임 포함__ 이(가) 선택됨
   + 탭 __저장__ 프로젝트를 저장하려면
1. App Builder 프로젝트에서 를 선택합니다. `Development` 작업 공간 선택기에서
1. 탭 __+ 서비스 추가 > API__ 열다 __API 추가__ 마법사에서 이 접근 방식을 사용하여 다음 API를 추가합니다.

   + __Experience Cloud > Asset compute__
      + 선택 __키 쌍 생성__ 을(를) 탭하고 __키 쌍 생성__ 단추를 누르고 다운로드한 파일을 저장합니다 `config.zip` 안전한 장소로 [나중에 사용](#private-key)
      + 탭 __다음__
      + 제품 프로필 선택 __통합 - Cloud Service__ 탭 __구성된 API 저장__
   + __Adobe Services > I/O 이벤트__ 탭 __구성된 API 저장__
   + __Adobe 서비스 > I/O 관리 API__ 탭 __구성된 API 저장__

## private.key에 액세스{#private-key}

설정 시 [asset compute API 통합](#set-up) 새 키 쌍이 생성되어 `config.zip` 파일이 자동으로 다운로드되었습니다. 이 `config.zip` 생성된 공개 인증서 및 일치 항목 포함 `private.key` 파일.

1. 압축 해제 `config.zip` 파일 시스템에서 `private.key` is [나중에 사용](../develop/environment-variables.md)
   + 보안 때문에 Git에 보안 및 개인 키를 추가해서는 안 됩니다.

## 서비스 계정(JWT) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬 [asset compute 개발 도구](../develop/development-tool.md) Adobe I/O Runtime과 상호 작용하려면 및 을 Asset compute 프로젝트에 통합해야 합니다. 서비스 계정(JWT) 자격 증명에 대해 숙지하십시오.

![Adobe 개발자 서비스 계정 자격 증명](./assets/app-builder/service-account.png)

1. Adobe I/O 프로젝트 앱 빌더 프로젝트에서 다음을 확인합니다. `Development` 작업 영역이 선택되어 있습니다.
1. 탭 __서비스 계정(JWT)__ 아래에 __자격 증명__
1. 표시된 Adobe I/O 자격 증명을 검토합니다.
   + 다음 __공개 키__ 맨 아래에 나열돼 있는데 __private.key__ 의 상대 `config.zip` 다운로드 시점 __asset compute API__ 이 프로젝트에 추가되었습니다.
      + 개인 키가 손실되거나 손상된 경우 일치하는 공개 키를 제거하고 이 인터페이스를 사용하여 Adobe I/O에 생성하거나 업로드하는 새 키 쌍을 제거할 수 있습니다.
