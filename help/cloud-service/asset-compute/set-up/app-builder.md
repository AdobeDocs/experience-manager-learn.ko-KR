---
title: asset compute 확장성을 위한 App Builder 설정
description: Asset compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서, 설정 및 배포하려면 Adobe Developer 콘솔에서 App Builder에 액세스해야 합니다.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# App Builder 설정

Asset compute 프로젝트는 특별히 정의된 App Builder 프로젝트로서, 설정 및 배포하려면 Adobe Developer 콘솔에서 App Builder에 액세스해야 합니다.

## Adobe Developer 콘솔에서 App Builder 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_upApp Builder 설정 클릭스루(오디오 없음)_

1. 에 로그인 [Adobe Developer 콘솔](https://console.adobe.io) 프로비저닝된 와 연관된 Adobe ID 사용 [계정 및 서비스](./accounts-and-services.md). 다음을 확인합니다. __시스템 관리자__ 또는 __개발자 역할__ 올바른 Adobe 조직용.
1. 을 탭하여 App Builder 프로젝트 만들기 __템플릿에서 새 프로젝트 만들기 > 프로젝트 만들기 > App Builder__

   _다음 중 하나의 경우__&#x200B;새 프로젝트 만들기&#x200B;__단추 또는__ App Builder __유형을 사용할 수 없습니다. 이는 Adobe 조직이 [App Builder로 프로비저닝됨](#request-adobe-project-app-builder)._

   + __프로젝트 제목__: `WKND AEM Asset Compute`
   + __앱 이름__: `wkndAemAssetCompute<YourName>`
      + 다음 __앱 이름__ 는 모든 FApp Builderely 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사나 조직의 이름을 접두사로 사용하고 의미 있는 접미사를 붙이는 것은 다음과 같은 좋은 방법입니다. `wkndAemAssetCompute`.
      + 자체 지원의 경우 종종 이름을 로 포스트픽스하는 것이 좋습니다. __앱 이름__, 예: `wkndAemAssetComputeJaneDoe` 다른 App Builder 프로젝트와의 충돌을 방지하기 위해
   + 아래 __작업 공간__ 이름이 인 새 환경 추가 `Development`
   + 아래 __Adobe I/O Runtime__ 확인 __각 작업 공간에 런타임 포함__ 이(가) 선택됨
   + 누르기 __저장__ 프로젝트를 저장하려면
1. App Builder 프로젝트에서 `Development` workspace 선택기에서
1. 누르기 __+ 서비스 추가 > API__ 을(를) 열려면 __API 추가__ 마법사에서 다음 API를 추가하려면 이 방법을 사용합니다.

   + __EXPERIENCE CLOUD > ASSET COMPUTE__
      + 선택 __키 쌍 생성__ 을 누릅니다. __키 쌍 생성__ 단추 및 다운로드한 파일 저장 `config.zip` 의 안전한 위치로 [나중에 사용](#private-key)
      + 누르기 __다음__
      + 제품 프로필 선택 __통합 - Cloud Service__ 및 탭 __구성된 API 저장__
   + __Adobe 서비스 > I/O 이벤트__ 및 탭 __구성된 API 저장__
   + __Adobe 서비스 > I/O 관리 API__ 및 탭 __구성된 API 저장__

## private.key 액세스{#private-key}

를 설정할 때 [Asset compute API 통합](#set-up) 새 키 쌍이 생성되고 `config.zip` 파일이 자동으로 다운로드되었습니다. 이 `config.zip` 생성된 공개 인증서 및 일치 항목 포함 `private.key` 파일.

1. 압축 풀기 `config.zip` 로 파일 시스템의 안전한 위치에 `private.key` 은(는) [나중에 사용](../develop/environment-variables.md)
   + 보안 문제로 인해 비밀과 개인 키를 Git에 추가해서는 안 됩니다.

## 서비스 계정(JWT) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬에서 사용됩니다. [Asset compute 개발 도구](../develop/development-tool.md) 는 Adobe I/O Runtime과 상호 작용해야 하며, Asset compute 프로젝트에 통합되어야 합니다. 서비스 계정(JWT) 자격 증명에 대해 숙지하십시오.

![Adobe Developer 서비스 계정 자격 증명](./assets/app-builder/service-account.png)

1. Adobe I/O 프로젝트 App Builder 프로젝트에서 `Development` 작업 영역이 선택됨
1. 탭 __서비스 계정(JWT)__ 아래에 __자격 증명__
1. 표시된 Adobe I/O 자격 증명을 검토합니다.
   + 다음 __공개 키__ 맨 아래에 나와 있는 것은 __private.key__ 의 상대 `config.zip` 다운로드 시 __ASSET COMPUTE API__ 이(가) 이 프로젝트에 추가되었습니다.
      + 개인 키가 손실되거나 손상되면 일치하는 공개 키를 제거하고 이 인터페이스를 사용하여 Adobe I/O에서 새 키 쌍을 생성하거나 업로드합니다.
