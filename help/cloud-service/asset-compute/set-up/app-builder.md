---
title: Asset Compute용 App Builder 확장성 설정
description: Asset Compute 프로젝트는 특별히 정의된 App Builder 프로젝트이며, 따라서 프로젝트를 설정하고 배포하려면 Adobe Developer Console의 App Builder에 액세스해야 합니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# App Builder 설정

Asset Compute 프로젝트는 특별히 정의된 App Builder 프로젝트이며, 따라서 프로젝트를 설정하고 배포하려면 Adobe Developer Console의 App Builder에 액세스해야 합니다.

## Adobe Developer Console에서 App Builder 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_설정 중에 클릭스루(오디오 없음)_

1. 제공된 [계정 및 서비스](./accounts-and-services.md)와(과) 연결된 Adobe ID을 사용하여 [Adobe Developer Console](https://console.adobe.io)에 로그인합니다. 올바른 Adobe 조직의 __시스템 관리자__ 또는 __개발자 역할__&#x200B;에 있는지 확인하십시오.
1. __새 프로젝트 만들기 > 템플릿에서 프로젝트 만들기 > App Builder__&#x200B;를 탭하여 App Builder 프로젝트를 만듭니다.

   _새 프로젝트 만들기__ 단추 또는 __App Builder__ 형식을 사용할 수 없는 경우, 이는 Adobe 조직이 [App Builder으로 프로비저닝되지 않음](#request-adobe-project-app-builder)._을 의미합니다.__

   + __프로젝트 제목__: `WKND AEM Asset Compute`
   + __앱 이름__: `wkndAemAssetCompute<YourName>`
      + __앱 이름__&#x200B;은(는) 모든 FApp Builderely 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사 또는 조직의 이름을 접두사로 사용하고 의미 있는 접미사를 붙여 넣는 것이 좋습니다. 예: `wkndAemAssetCompute`
      + 자체 활성화의 경우 다른 App Builder 프로젝트와의 충돌을 방지하기 위해 `wkndAemAssetComputeJaneDoe`과(와) 같이 __앱 이름__(으)로 이름을 후정하는 것이 가장 좋습니다.
   + __작업 공간__&#x200B;에 `Development`(이)라는 새 환경을 추가합니다.
   + __Adobe I/O Runtime__&#x200B;에서 __각 작업 영역에 런타임 포함__&#x200B;을 선택하십시오.
   + 프로젝트를 저장하려면 __저장__&#x200B;을 탭하세요.
1. App Builder 프로젝트의 작업 영역 선택기에서 `Development`을(를) 선택합니다.
1. __+ 서비스 추가 > API__&#x200B;를 눌러 __API 추가__ 마법사를 엽니다. 이 접근 방식을 사용하여 다음 API를 추가하십시오.

   + __Experience Cloud > Asset Compute__
      + __키 쌍 생성__&#x200B;을 선택하고 __키 쌍 생성__ 단추를 탭한 다음 다운로드한 `config.zip`을(를) [나중에 사용할 수 있도록 안전한 위치에 저장하십시오](#private-key)
      + __다음__ 탭
      + 제품 프로필 __통합 - Cloud Service__&#x200B;을 선택하고 __구성된 API 저장__&#x200B;을 탭하세요.
   + __Adobe 서비스 > I/O 이벤트__ 및 __구성된 API 저장__&#x200B;을 탭하세요.
   + __Adobe 서비스 > I/O 관리 API__ 및 __구성된 API 저장__&#x200B;을 누릅니다.

## private.key 액세스{#private-key}

[Asset Compute API 통합](#set-up)을(를) 설정할 때 새 키 쌍이 생성되고 `config.zip` 파일이 자동으로 다운로드되었습니다. 이 `config.zip`에는 생성된 공개 인증서와 일치하는 `private.key` 파일이 포함되어 있습니다.

1. `private.key`이(가) [나중에 사용](../develop/environment-variables.md)되므로 `config.zip`을(를) 파일 시스템의 안전한 위치로 압축 해제합니다.
   + 보안 문제로 인해 비밀과 개인 키를 Git에 추가해서는 안 됩니다.

## 서비스 계정(JWT) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬 [Asset Compute 개발 도구](../develop/development-tool.md)에서 Adobe I/O Runtime과 상호 작용하는 데 사용되며 Asset Compute 프로젝트에 통합되어야 합니다. 서비스 계정(JWT) 자격 증명에 대해 숙지하십시오.

![Adobe Developer 서비스 계정 자격 증명](./assets/app-builder/service-account.png)

1. Adobe I/O 프로젝트 App Builder 프로젝트에서 `Development` 작업 영역을 선택했는지 확인합니다.
1. __자격 증명__&#x200B;에서 __서비스 계정(JWT)__&#x200B;을 탭하세요.
1. 표시된 Adobe I/O 자격 증명을 검토합니다.
   + 맨 아래에 나열된 __공개 키__&#x200B;은(는) __Asset Compute API__&#x200B;이(가) 이 프로젝트에 추가되었을 때 다운로드된 `config.zip`의 __private.key__ 상대가 있습니다.
      + 개인 키가 유실되거나 훼손된 경우 일치하는 공개 키를 제거하고 이 인터페이스를 사용하여 Adobe I/O에서 새 키 쌍을 생성하거나 업로드합니다.
