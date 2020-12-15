---
title: Cloud Services을 사용하여 Adobe Experience Manager과 Adobe Target 통합
seo-title: 레거시 Cloud Services을 사용하여 Adobe Experience Manager(AEM)과 Adobe Target 통합
description: AEM Cloud Service을 사용하여 Adobe Experience Manager(AEM)과 Adobe Target을 통합하는 방법에 대한 단계별 연습
seo-description: AEM Cloud Service을 사용하여 Adobe Experience Manager(AEM)과 Adobe Target을 통합하는 방법에 대한 단계별 연습
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# AEM 레거시 Cloud Services 사용

이 섹션에서는 기존 Cloud Services을 사용하여 Adobe Target과 Adobe Experience Manager(AEM)을 설정하는 방법에 대해 설명합니다.

>[!NOTE]
>
> Adobe Target이 있는 AEM 이전 Cloud Service은 **만 AEM 작성자와 Adobe Target 백엔드 연결을 직접 설정하여 AEM에서 Target으로 콘텐츠를 게시하는 데 사용됩니다.** Adobe Launch는 AEM에서 제공하는 공개 웹 사이트 경험에 Adobe Target을 노출하는 데 사용됩니다.

개인화 활동에 힘을 실어주는 AEM 경험 조각 오퍼를 사용하려면 다음 장으로 진행하며 기존 클라우드 서비스를 사용하여 AEM을 Adobe Target과 통합할 수 있습니다. 이 통합은 HTML/JSON 오퍼로 경험 조각을 AEM에서 Target으로 푸시하고 대상 오퍼를 AEM과 동기화하도록 유지하는 데 필요합니다. 이 통합은 개요 섹션](./overview.md#personalization-using-aem-experience-fragment)에서 설명한 [시나리오 1을 구현하는 데 필요합니다.

## 전제 조건

* **AEM**

   * 이 자습서를 완료하려면 AEM 작성자 및 게시 인스턴스가 필요합니다. AEM 인스턴스를 아직 설정하지 않은 경우 [여기](./implementation.md#set-up-aem) 단계를 따를 수 있습니다.

* **Experience Cloud**
   * 조직 Adobe Experience Cloud 액세스 - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 다음 솔루션으로 제공된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 고객은 [Adobe 지원](https://helpx.adobe.com/kr/contact/enterprise-support.ec.html)에서 Experience Platform Launch 및 Adobe I/O을 공급받거나 시스템 관리자에게 문의해야 합니다.



### Adobe Target과 AEM 통합

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe IMS 인증을 사용하여 Adobe Target Cloud Service 만들기(*Adobe Target API 사용*)(00:34)
2. Adobe Target 클라이언트 코드 받기(01:50)
3. Adobe Target용 Adobe IMS 구성 만들기(02:08)
4. Adobe I/O 콘솔 내에서 Target API에 액세스하기 위한 기술 계정 만들기(02:08)
5. AEM 경험 조각에 Adobe Target Cloud Service 추가(04:12)

이 시점에서 옵션 2에 자세히 설명된 대로 레거시 Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)을 사용하여 [AEM을 Adobe Target과 성공적으로 통합했습니다. 이제 AEM 내에서 경험 조각을 만들고 경험 조각을 HTML 오퍼 또는 JSON 오퍼로 Adobe Target에 게시한 다음 활동을 만드는 데 사용할 수 있어야 합니다.
