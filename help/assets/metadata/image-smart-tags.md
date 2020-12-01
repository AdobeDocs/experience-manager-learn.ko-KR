---
title: AEM Assets을 사용한 이미지 스마트 태그
description: 이미지에 대한 스마트 태그는 이미지의 컨텐츠를 기반으로 이미지 자산에 메타데이터 태그를 자동으로 지능적으로 추가하여 AEM 검색 기능을 향상시킵니다.
feature: smart-tags
topics: metadata, search
audience: all
doc-type: feature video
activity: understand
version: 6.3, 6.4, 6.5
kt: 5244
thumbnail: 17019.jpg
translation-type: tm+mt
source-git-commit: 159e9d7ea363768f202db91a1d0555c2e57ebb13
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 8%

---


# 이미지의 스마트 태그

이미지에 대한 AEM Assets의 스마트 태그는 파생된 메타데이터 태그를 이미지 자산에 자동으로 추가하여 AEM Assets의 검색을 향상시켜 올바른 이미지를 보다 쉽고 빠르게 찾을 수 있도록 함으로써 제작 환경을 개선합니다.

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## AEM 6.x{#set-up}에 대해 설정

>[!NOTE]
> 이미지에 대한 스마트 태그는 AEM에 Cloud Service으로 자동으로 제공됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

스마트 콘텐츠 서비스를 사용하려면 먼저 다음을 확인하여 Adobe I/O에서 통합을 만드십시오.

* 조직에 대한 관리자 권한이 부여된 Adobe ID 계정이 있습니다
* 조직에서 스마트 콘텐츠 서비스 서비스를 사용할 수 있습니다.

비디오에서는 스마트 태그 이미지에 사용되는 Adobe I/O 스마트 콘텐츠 서비스를 구성하는 데 필요한 다음 작업을 자세히 설명합니다.

* AEM에서 스마트 콘텐츠 서비스 구성을 만들어 공개 키를 생성합니다. OAuth 통합을 위한 공개 인증서를 받습니다.
* Adobe I/O에서 통합을 만들고 생성된 공개 키를 업로드합니다.
* Adobe I/O의 API 키 및 기타 자격 증명을 사용하여 AEM 인스턴스를 구성합니다.
* 자산 업로드 시 자동 태그 지정을 활성화합니다(선택 사항).

## 추가 리소스

* [AEM Assets 스마트 태그 설명서](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
