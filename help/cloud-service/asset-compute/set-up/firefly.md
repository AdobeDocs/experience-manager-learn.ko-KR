---
title: asset compute 확장성을 위해 Adobe Project Firefly 설정
description: asset compute 프로젝트는 특별히 정의된 Adobe Project Firefly 프로젝트로서, 설정 및 배포하려면 Adobe 개발자 콘솔에서 Project Firefly Adobe에 액세스해야 합니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# Firefly Adobe 프로젝트 설정

asset compute 프로젝트는 특별히 정의된 Adobe Project Firefly 프로젝트로서, 설정 및 배포하려면 Adobe 개발자 콘솔에서 Project Firefly Adobe에 액세스해야 합니다.

## Adobe 개발자 콘솔에서 Firefly Adobe 프로젝트 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Adobe Project Firefly 설정 클릭스루(오디오 없음)_

1. 제공된 [계정 및 서비스](./accounts-and-services.md)와 연결된 Adobe ID을 사용하여 [Developer Console](https://console.adobe.io)에 로그인합니다. 올바른 Adobe 조직에 대한 __시스템 관리자__ 또는 __개발자 역할__&#x200B;에 있는지 확인합니다.
1. __새 프로젝트 만들기 > 템플릿에서 프로젝트 > Project Firefly__&#x200B;를 탭하여 Firefly 프로젝트를 만듭니다.

   _새__&#x200B;프로젝트 만들기 __단추 또는__&#x200B;프로젝트 수정 __유형 [을 사용할 수 없는 경우, 이는 Adobe 조직에 Project Firefly가 ](#request-adobe-project-firefly)제공되지 않음을 의미합니다._

   + __프로젝트 제목__:  `WKND AEM Asset Compute`
   + __앱 이름__:  `wkndAemAssetCompute<YourName>`
      + __앱 이름__&#x200B;은 모든 Firefly 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사나 조직의 이름을 접두사로 사용하고 의미 있는 접미사를 사용하여 포스트픽스를 수정하는 것은 다음과 같은 좋은 방법입니다.`wkndAemAssetCompute`
      + 자체 활성화를 위해 다른 Project Firefly 프로젝트와의 충돌을 방지하기 위해 이름을 __앱 이름__(예: `wkndAemAssetComputeJaneDoe`)에 포스트픽스하는 것이 가장 좋습니다.
   + __작업 공간__&#x200B;에서 `Development`라는 새 환경을 추가합니다
   + __Adobe I/O Runtime__&#x200B;각 작업 공간에서 __런타임 포함__&#x200B;이 선택되었는지 확인합니다
   + __저장__&#x200B;을 눌러 프로젝트를 저장합니다
1. Firefly Adobe 프로젝트의 작업 영역 선택기에서 `Development` 을 선택합니다
1. __+ 서비스 추가 > API__&#x200B;를 눌러 __API__ 추가 마법사를 열고 이 접근 방식을 사용하여 다음 API를 추가합니다.

   + __Experience Cloud > Asset compute__
      + __키 쌍 생성__&#x200B;을 선택하고 __키 쌍 생성__ 단추를 탭하고 다운로드한 `config.zip`을(를) [나중에 사용](#private-key)할 수 있도록 안전한 위치에 저장합니다
      + __Next__ 탭하기
      + 제품 프로필 __통합 - Cloud Service__&#x200B;을 선택하고 __구성된 API 저장__&#x200B;을 탭합니다.
   + __Adobe 서비스 > I/O__ 이벤트 를 탭하고 구성된  __API 저장 을 탭합니다__
   + __Adobe 서비스 > I/O 관리__ API 및 구성된  __API 저장 탭하기__

## private.key{#private-key}에 액세스합니다.

[Asset compute API 통합](#set-up)을 설정할 때 새 키 쌍이 생성되고 `config.zip` 파일이 자동으로 다운로드되었습니다. 이 `config.zip`에는 생성된 공개 인증서가 포함되어 있으며 일치하는 `private.key` 파일이 포함되어 있습니다.

1. `private.key`이 [나중에 사용되므로 `config.zip`을 파일 시스템의 안전한 위치로 압축 해제합니다.](../develop/environment-variables.md)
   + 보안 때문에 Git에 보안 및 개인 키를 추가해서는 안 됩니다.

## 서비스 계정(JWT) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬 [Asset compute 개발 도구](../develop/development-tool.md)에서 Adobe I/O Runtime과 상호 작용하는 데 사용되며 Asset compute 프로젝트에 통합되어야 합니다. 서비스 계정(JWT) 자격 증명에 대해 숙지하십시오.

![Adobe 개발자 서비스 계정 자격 증명](./assets/firefly/service-account.png)

1. Project Firefly Adobe I/O 프로젝트에서 `Development` 작업 영역이 선택되어 있는지 확인합니다
1. __자격 증명__&#x200B;에서 __서비스 계정(JWT)__&#x200B;을 탭합니다.
1. 표시된 Adobe I/O 자격 증명을 검토합니다.
   + 맨 아래에 나열된 __공개 키__&#x200B;에는 __Asset compute API__&#x200B;를 이 프로젝트에 추가할 때 다운로드한 `config.zip`에 있는 __private.key__&#x200B;이 있습니다.
      + 개인 키가 손실되거나 손상된 경우 일치하는 공개 키를 제거하고 이 인터페이스를 사용하여 Adobe I/O에 생성하거나 업로드하는 새 키 쌍을 제거할 수 있습니다.
