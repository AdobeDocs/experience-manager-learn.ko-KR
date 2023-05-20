---
title: Cloud Services을 사용하여 Adobe Experience Manager과 Adobe Target 통합
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: AEM Cloud Service을 사용하여 Adobe Experience Manager(AEM)를 Adobe Target과 통합하는 방법에 대한 단계별 연습
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---

# AEM 기존 Cloud Services 사용

이 섹션에서는 레거시 Cloud Services을 사용하여 Adobe Target과 Adobe Experience Manager(AEM)를 설정하는 방법에 대해 설명합니다.

>[!NOTE]
>
> Adobe Target이 있는 AEM 레거시 Cloud Service은 **전용** AEM에서 Target으로 컨텐츠를 게시하기 위해 Adobe Target 작성자와 직접 AEM 백엔드 연결을 설정하는 데 사용됩니다. Adobe Launch는 AEM에서 제공하는 공개 웹 사이트 경험에 Adobe Target을 노출하는 데 사용됩니다.

AEM Experience Fragment 오퍼를 사용하여 개인화 활동을 지원하기 위해, 다음 장으로 이동하고 레거시 클라우드 서비스를 사용하여 AEM을 Adobe Target과 통합합니다. 이 통합은 AEM에서 경험 조각을 Target/JSON 오퍼로 HTML에 푸시하고 Target 오퍼를 AEM과 동기화 상태로 유지하는 데 필요합니다. 이 통합은 구현을 위해 필요합니다. [개요 섹션에서 설명한 시나리오 1](./overview.md#personalization-using-aem-experience-fragment).

## 사전 요구 사항

* **AEM**

   * 이 자습서를 완료하려면 AEM 작성자 및 게시 인스턴스가 필요합니다. AEM 인스턴스를 아직 설정하지 않은 경우 단계를 따를 수 있습니다 [여기](./implementation.md#set-up-aem).

* **Experience Cloud**
   * 조직에 대한 액세스 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 프로비저닝된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 고객은 의 Experience Platform Launch 및 Adobe I/O을 제공받아야 합니다. [Adobe 지원](https://helpx.adobe.com/kr/contact/enterprise-support.ec.html) 또는 시스템 관리자에게 문의하십시오


### AEM과 Adobe Target 통합

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe IMS 인증을 사용하여 Adobe Target Cloud Service 만들기(*Adobe Target API 사용*) (00:34)
2. Adobe Target 클라이언트 코드 얻기(01:50)
3. Adobe Target에 대한 Adobe IMS 구성 만들기(02:08)
4. Adobe I/O 콘솔 내에서 Target API에 액세스하기 위한 기술 계정 만들기(02:08)
5. AEM Experience Fragments에 Adobe Target Cloud Service 추가(04:12)

이 시점에서 을(를) 통합했습니다. [이전 Cloud Services을 사용하는 Adobe Target과 AEM](./using-aem-cloud-services.md#integrating-aem-target-options) 옵션 2에 자세히 설명되어 있습니다. 이제 AEM 내에서 경험 조각을 만들고 HTML 오퍼 또는 JSON 오퍼로서 경험 조각을 Adobe Target에 게시한 다음 활동을 만드는 데 사용할 수 있습니다.
