---
title: 자산 계산 확장성을 위해 Adobe 프로젝트 Firefly 설정
description: 에셋 컴퓨팅 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefly 프로젝트입니다. 이와 같이 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefly에 액세스해야 설정 및 배포할 수 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Adobe 프로젝트 Firefly 설정

에셋 컴퓨팅 프로젝트는 특별히 정의된 Adobe 프로젝트 Firefly 프로젝트입니다. 이와 같이 Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefly에 액세스해야 설정 및 배포할 수 있습니다.

## Adobe 개발자 콘솔에서 Adobe 프로젝트 Firefly 만들기 및 설정{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_Adobe 프로젝트 Firefly(오디오 없음)를 설정하는 클릭스루_

1. 제공된 [계정 및 서비스와](https://console.adobe.io) 연결된 Adobe ID을 사용하여 Adobe 개발자 콘솔에 [로그인합니다](./accounts-and-services.md). 시스템 __관리자나__ 올바른 Adobe 조직의 __개발자 역할__ 에 있는지 확인합니다.
1. 새 프로젝트 만들기 > 템플릿에서 프로젝트 __만들기 > Project Firefox를 눌러 Firefox 프로젝트 만들기__

   _새 프로젝트__&#x200B;만들기&#x200B;__단추 또는__&#x200B;프로젝트 Firefly __유형을 사용할 수 없는 경우 이는 Adobe 조직이 Project Firefly로[제공되지 않음을](#request-adobe-project-firefly)의미합니다._

   + __프로젝트 제목__: `WKND AEM Asset Compute`
   + __앱 이름__: `wkndAemAssetCompute<YourName>`
      + 앱 이름 __은__ 모든 Firefox 프로젝트에서 고유해야 하며 나중에 수정할 수 없습니다. 회사나 조직의 이름을 접두사로 사용하고, 접미사를 붙인 포스트픽스는 다음과 같은 좋은 방법입니다. `wkndAemAssetCompute`.
      + 자체 지원을 위해 다른 Project Firefly 프로젝트와의 충돌을 피하는 등 __앱 이름에__&#x200B;이름을 `wkndAemAssetComputeJaneDoe` 게시하는 것이 가장 좋습니다.
   + 작업 __공간__ 아래에서 `Development`
   + [ __Adobe I/O Runtime__ ] 아래에서 __각 작업 영역에 [런타임 포함]을__ 선택합니다.
   + 저장을 __눌러__ 프로젝트 저장
1. Adobe Firefly 프로젝트의 작업 영역 선택기 `Development` 에서 선택합니다
1. Add __+ Service > API를__ 눌러 API __추가__ 마법사를 열고 다음 API를 추가합니다.

   + __Experience Cloud > 자산 계산__
      + 키 쌍 __생성을__ 선택하고 키 쌍 ____ 생성 `config.zip` 단추를 누른 다음 다운로드된 내용을 [나중에 사용할 수 있도록 안전한 위치에 저장합니다.](#private-key)
      + 다음 __탭__
      + 제품 프로필 통합 - Cloud Service __를__ 선택하고 __구성된 API 저장을 탭합니다.__
   + __Adobe 서비스 > I/O 이벤트__ 및 __구성된 API 저장을 탭합니다.__
   + __Adobe 서비스 > I/O 관리 API__ 및 __구성된 API 저장을 탭합니다.__

## private.key 액세스{#private-key}

자산 계산 API 통합 [을](#set-up) 설정할 때 새 키 쌍이 생성되고 `config.zip` 파일이 자동으로 다운로드되었습니다. 생성된 공용 인증서와 일치하는 `config.zip` `private.key` 파일이 포함되어 있습니다.

1. 파일 시스템 `config.zip` 의 안전한 위치에 압축 `private.key` [해제](../develop/environment-variables.md)
   + 보안 문제로 Git에 기밀 및 개인 키를 추가하지 않아야 합니다.

## JWT(서비스 계정) 자격 증명 검토

이 Adobe I/O 프로젝트의 자격 증명은 로컬 [에셋 컴퓨팅 개발 도구에서](../develop/development-tool.md) Adobe I/O Runtime과 상호 작용하기 위해 사용되므로 에셋 컴퓨팅 프로젝트에 통합되어야 합니다. JWT(서비스 계정) 자격 증명을 숙지하십시오.

![Adobe 개발자 서비스 계정 자격 증명](./assets/firefly/service-account.png)

1. Adobe I/O 프로젝트 Firefly 프로젝트에서 작업 영역이 `Development` 선택되었는지 확인합니다
1. 자격 증명 아래의 __서비스 계정(JWT)__ 을 __탭합니다.__
1. 표시된 Adobe I/O 자격 증명 검토
   + 맨 아래에 나열된 __공개__ 키는 __자산 계산 API가__ 이 프로젝트에 추가될 때 다운로드된 `config.zip` 공개 키에 __private.key__ 가있습니다.
      + 개인 키가 손실되거나 손상되면 일치하는 공개 키를 제거할 수 있으며 이 인터페이스를 사용하여 Adobe I/O에 생성되거나 업로드되는 새 키 쌍입니다.
