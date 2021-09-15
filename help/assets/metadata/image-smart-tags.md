---
title: AEM Assets을 사용한 이미지용 스마트 태그
description: 이미지용 스마트 태그는 이미지의 컨텐츠를 기반으로 이미지 자산에 메타데이터 태그를 자동으로 지능적으로 추가하여 AEM 검색 기능을 향상시킵니다.
topic: Content Management
feature: Smart Tags
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, Cloud Service
kt: 645
thumbnail: 17019.jpg
exl-id: c72dc489-70e6-48ca-99a8-663d4c0652ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 8%

---

# 이미지용 스마트 태그

이미지에 대한 AEM Assets의 스마트 태그는 파생된 메타데이터 태그를 이미지 자산에 자동으로 추가하여 AEM Assets의 검색을 강화하며 올바른 이미지를 보다 쉽고 빠르게 찾을 수 있도록 함으로써 작성 환경을 개선합니다.

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## AEM 6.x 설정{#set-up}

>[!NOTE]
> 이미지용 스마트 태그는 AEM에 Cloud Service으로 자동으로 제공됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

스마트 컨텐츠 서비스를 사용하려면 먼저 Adobe I/O에 대한 통합을 만들기 위해 다음을 확인하십시오.

* 조직에 대한 관리자 권한이 부여된 Adobe ID 계정이 있습니다
* 조직에서 스마트 컨텐츠 서비스 서비스를 사용할 수 있습니다

비디오에서는 스마트 태그 이미지에 사용되는 Adobe I/O 스마트 컨텐츠 서비스를 구성하는 데 필요한 다음 작업에 대해 자세히 설명합니다.

* AEM에서 스마트 컨텐츠 서비스 구성을 만들어 공개 키를 생성합니다. OAuth 통합을 위한 공개 인증서를 받습니다.
* Adobe I/O에서 통합을 만들고 생성된 공개 키를 업로드합니다.
* Adobe I/O의 API 키 및 기타 자격 증명을 사용하여 AEM 인스턴스를 구성합니다.
* 자산 업로드에서 자동 태깅을 활성화합니다(선택 사항).

## 추가 리소스

* [AEM Assets 스마트 태그 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
