---
title: Adobe Experience Cloud와의 AEM as a Cloud Service 통합
description: AEM as a Cloud Service이 다른 Adobe Experience Cloud 제품과의 지원되는 통합에 대해 알아봅니다.
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.jpeg
mini-toc-levels: 1
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: 4a902d838c99b3452581066ee568876ad16ec1a3
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 11%

---

# Adobe Experience Cloud와의 AEM as a Cloud Service 통합

AEM as a Cloud Service이 다른 Adobe Experience Cloud 제품과의 지원되는 통합에 대해 알아봅니다.
통합을 구성하고 사용하는 방법에 대한 설명서를 보려면 Experience Cloud 제품을 클릭합니다.

|  | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |  |  | ✔ |
| [분석](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |  |  |  |
| [Campaign Classic](#adobe-campaign-classic) | ✔ |  |  |
| Campaign Standard |  |  |  |
| [상거래](#adobe-commerce) | ✔ | ✔ |  |
| Customer Journey Analytics |  |  |  |
| [Experience Platform 태그](#adobe-experience-platform-tags) | ✔ |  | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |  | ✔ |  |
| Learning Manager |  |  |  |
| Marketo Engage |  |  |  |
| 실시간 CDP |  |  |  |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [대상](#adobe-target) | ✔ |  |  |
| [Workfront](#adobe-workfront) |  | ✔ |  |


## Adobe Acrobat Sign

Adobe Acrobat Sign(이전 Adobe Sign)을 사용하면 법률, 판매, 급여, HR 및 기타 영역에 대한 문서를 처리하는 워크플로우를 개선하여 AEM Forms의 적응형 양식에 대해 전자 서명 워크플로우를 사용할 수 있습니다.

### AEM Forms

+ [Adobe Acrobat Sign 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM Forms 및 Adobe Acrobat Sign 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

Adobe Analytics과 AEM as a Cloud Service을 통합하여 컨텐츠 활동을 추적하고 고객 여정의 어디에서나 데이터를 분석할 수 있도록 합니다. 또한 다양한 보고, 예측 인텔리전스 등을 활용할 수 있습니다.

### AEM Sites

+ [Adobe Analytics 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sites 및 Analytics 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html)
+ Adobe 클라이언트 데이터 레이어(ACDL)

   + [AEM WCM 코어 구성 요소에서 ACDL 확장](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [AEM WCM 핵심 구성 요소와 ACDL 통합](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [ACDL을 사용한 이벤트 기반 데이터 처리](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [ACDL(Adobe 클라이언트 데이터 레이어) 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)

### AEM Assets

+ [자산 통찰력 개요](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [자산 통찰력 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [자산 통찰력 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [Adobe Analytics 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

## Adobe Campaign Classic

Adobe Campaign Classic과 AEM as a Cloud Service을 통합하면 Adobe Campaign Classic을 사용하여 이메일을 개인화하고 전달하는 동안 Adobe Experience Manager에서 직접 이메일 게재 콘텐츠 및 양식을 관리할 수 있습니다.

### AEM Sites

+ [Adobe Campaign Classic 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaignonpremise.html)
   + __AEM 6.5에 대한 설명서 링크가 있지만 단계는 AEM as a Cloud Service에서 동일합니다__
+ [AEM Sites과 Adobe Campaign Classic 통합](https://github.com/adobe/aem-core-email-components/wiki/Integrating-AEM-with-ACC)
+ [Adobe Campaign Classic을 사용하여 AEM Sites에서 이메일 보내기](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/aem-adobe-campaign/campaign.html)
+ [AEM 이메일 핵심 구성 요소 설명서](https://github.com/adobe/aem-core-email-components#aem-email-core-components)


## Adobe Commerce

Adobe Commerce은 AEM as a Cloud Service과 통합되므로 기업이 확장 및 혁신을 통해 전자 상거래 경험을 차별화하고 온라인 비용을 신속하게 캡처할 수 있습니다. AEM with Commerce는 Experience Manager의 몰입, 옴니채널 및 개인화된 경험을 다양한 상거래 솔루션과 결합하여 구매 여정의 모든 부분에 차별화된 경험을 제공하고 가치 창출 시간을 단축하고 높은 전환을 유도합니다.

### AEM Sites

+ [AEM Content and Commerce 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platform 태그

Adobe Experience Platform 태그(이전 Adobe Launch, DTM)가 AEM과 원활하게 통합되므로 간편하게 배포하고 관리할 수 있습니다 [analytics](#adobe-analytics), [타겟팅](#adobe-target)고객 경험을 수행하는 데 필요한 , 마케팅 및 광고 태그입니다.

### AEM Sites

+ [Experience Platform 태그 사용 안내서](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform 태그 튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

### AEM Forms

+ [Experience Platform 태그 사용 안내서](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform 태그 튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)


## Adobe Workfront

Adobe Workfront과 AEM의 Cloud Service은 디지털 자산 생성, 공동 작업 및 라이프사이클 관리 프로세스를 간소화합니다.

### AEM Assets

+ [Workfront 고급 커넥터 구성](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
+ [Workfront 향상된 커넥터 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentials 사용 안내서](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront 및 Assets Essentials 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)

## Adobe Sensei

Adobe Sensei은 스마트 태그, 스마트 자르기, 시각적 검색 등을 통해 콘텐츠 관리 프로세스를 변환하는 AI 및 시스템 학습 기술을 제공합니다.

### AEM Sites

+ [컨텐츠 조각으로 텍스트 요약](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [이미지용 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [이미지용 사용자 정의 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [비디오용 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [스마트 자르기](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [시각적 검색](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [automated forms conversion 서비스](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target은 AEM as a Cloud Service과 통합되어 AEM의 컨텐츠를 기반으로 모든 최종 사용자에게 최적화된 웹 경험을 제공합니다.

### AEM Sites

+ [Adobe Target 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ 경험 조각에서 Target

   + [Target에 경험 조각 게시](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [경험 조각을 Target에 JSON으로 게시](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [Target에서 AEM Context Hub 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sites 및 Target 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)
