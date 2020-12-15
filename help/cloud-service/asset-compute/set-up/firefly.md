---
title: asset compute 확장성을 위해 Adobe 프로젝트 Firefox 설정
description: asset compute 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefox 프로젝트입니다. 따라서 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefox에 액세스해야 설정 및 배포할 수 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Adobe 프로젝트 Firefox 설정

asset compute 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefox 프로젝트입니다. 따라서 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefox에 액세스해야 설정 및 배포할 수 있습니다.

## Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefox 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Adobe 프로젝트 반딧불 설정 클릭스루(오디오 없음)_

1. 제공된 [ 계정 및 서비스](./accounts-and-services.md)와 연결된 Adobe ID을 사용하여 [Adobe 개발자 콘솔](https://console.adobe.io)에 로그인합니다. __시스템 관리자__ 또는 올바른 Adobe 조직의 __개발자 역할__&#x200B;에 있는지 확인합니다.
1. __새 프로젝트 만들기 > 템플릿에서 프로젝트 만들기 > Project Firefox__&#x200B;를 눌러 Firefox 프로젝트 만들기

   _새 프로젝트__&#x200B;만들기 __단추 또__&#x200B;는 프로젝트 수정 유형&#x200B;__을 사용할 수 없는 경우, 이것은 Adobe 조직이 Project Firefly로  [제공되지 않음을 의미합니다](#request-adobe-project-firefly)._

   + __프로젝트 제목__:  `WKND AEM Asset Compute`
   + __앱 이름__:  `wkndAemAssetCompute<YourName>`
      + __앱 이름__&#x200B;은 모든 Firefox 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사 또는 조직의 이름을 접두사로 사용하고, 접미사를 붙이면 다음과 같은 좋은 방법이 있습니다.`wkndAemAssetCompute`.
      + 자체 지원의 경우, 다른 Project Firefox 프로젝트와의 충돌을 피하기 위해 __앱 이름__&#x200B;에 이름을 게시합니다(예: `wkndAemAssetComputeJaneDoe`).
   + __작업 영역__&#x200B;에서 `Development`라는 새 환경 추가
   + __Adobe I/O Runtime__&#x200B;각 작업 영역&#x200B;__에__&#x200B;런타임 포함 선택
   + __저장__&#x200B;을 눌러 프로젝트를 저장합니다.
1. Adobe Firefly 프로젝트의 작업 영역 선택기에서 `Development`을 선택합니다.
1. __+ 서비스 추가 > API__&#x200B;를 눌러 __API__ 추가 마법사를 열고 이 방법을 사용하여 다음 API를 추가합니다.

   + __Experience Cloud > Asset compute__
      + __키 쌍 생성__&#x200B;을 선택하고 __키 쌍 생성__ 단추를 누르고 다운로드한 `config.zip`을 [나중에 사용](#private-key)할 수 있는 안전한 위치에 저장합니다.
      + __다음__&#x200B;을 탭합니다.
      + 제품 프로필 __통합 - Cloud Service__&#x200B;을 선택하고 __구성된 API 저장__&#x200B;을 탭합니다.
   + __Adobe 서비스 > I/O 이벤트__ 를 탭하고 구성된  __API 저장을 탭합니다.__
   + __Adobe 서비스 > I/O 관리__ API를 클릭하고 구성된  __API 저장을 탭합니다.__

## private.key{#private-key} 액세스

[Asset compute API 통합](#set-up)을 설정할 때 새 키 쌍이 생성되고 `config.zip` 파일이 자동으로 다운로드되었습니다. 이 `config.zip`에는 생성된 공용 인증서와 일치하는 `private.key` 파일이 포함되어 있습니다.

1. `private.key`이(가) 나중에 [사용되므로 `config.zip`을(를) 파일 시스템의 안전한 장소에 압축 해제하십시오](../develop/environment-variables.md)
   + 보안 문제로 Git에 비밀과 개인 키를 추가해서는 안 됩니다.

## JWT(서비스 계정) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬 [Asset compute 개발 도구](../develop/development-tool.md)에서 Adobe I/O Runtime과 상호 작용하기 위해 사용되며, Asset compute 프로젝트에 통합되어야 합니다. JWT(서비스 계정) 자격 증명을 숙지하십시오.

![Adobe 개발자 서비스 계정 자격 증명](./assets/firefly/service-account.png)

1. Adobe I/O 프로젝트 Firefly 프로젝트에서 `Development` 작업 영역이 선택되어 있는지 확인합니다
1. __자격 증명__ 아래의 __서비스 계정(JWT)__&#x200B;을 탭합니다.
1. 표시된 Adobe I/O 자격 증명을 검토합니다.
   + 맨 아래에 나열된 __공개 키__&#x200B;에는 __Asset compute API__&#x200B;이(가) 이 프로젝트에 추가되었을 때 다운로드한 `config.zip`의 __private.key__&#x200B;이(가) 있습니다.
      + 개인 키가 손실되거나 손상되지 않은 경우 일치하는 공개 키를 제거할 수 있으며 이 인터페이스를 사용하여 새 키 쌍을 Adobe I/O에 생성하거나 업로드할 수 있습니다.
