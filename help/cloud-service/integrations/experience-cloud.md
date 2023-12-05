---
title: Adobe Experience Cloud과 AEM as a Cloud Service 통합
description: AEM as a Cloud Service과 다른 Adobe Experience Cloud 제품과의 지원되는 통합에 대해 알아봅니다.
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
jira: KT-10718
thumbnail: KT-10718.png
last-substantial-update: 2022-11-17T00:00:00Z
mini-toc-levels: 1
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM as a Cloud Service" before-title="false"
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
duration: 211
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 11%

---

# Adobe Experience Cloud과 AEM as a Cloud Service 통합

AEM as a Cloud Service과 다른 Adobe Experience Cloud 제품과의 지원되는 통합에 대해 알아봅니다.
통합 구성 및 사용 방법에 대한 설명서를 보려면 Experience Cloud 제품을 클릭하십시오.

|                                                                   | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |           |            | ✔ |
| 광고 |           |            |          |
| [분석](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |           |            |          |
| Campaign Classic |           |            |          |
| Campaign Standard |           |            |          |
| [상거래](#adobe-commerce) | ✔ | ✔ |          |
| Customer Journey Analytics |           |            |          |
| [Experience Platform 태그](#adobe-experience-platform-tags) | ✔ |            | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |           | ✔ |          |
| [Learning Manager](#adobe-learning-manager) | ✔ |            |          |
| Marketo Engage |           |            |          |
| Real-time CDP |           |            |          |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [대상](#adobe-target) | ✔ |            |          |
| [Workfront](#adobe-workfront) |           | ✔ |          |


## Adobe Acrobat Sign

Adobe Acrobat Sign(이전의 Acrobat Sign)는 법률, 판매, 급여, HR 및 기타 영역의 문서를 처리하는 워크플로를 개선하여 AEM Forms의 적응형 양식을 위한 전자 서명 워크플로를 활성화합니다.

### AEM Forms

+ [Adobe Acrobat Sign 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM Forms 및 Adobe Acrobat Sign 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

AEMas a Cloud Service 과 Adobe Analytics을 통합하면 고객 여정의 어디에서든 컨텐츠 활동을 추적하고 데이터를 분석할 수 있습니다. 또한 다양한 보고 기능, 예측 인텔리전스 등을 활용할 수 있습니다.

### AEM Sites

+ [Adobe Analytics 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sites 및 Analytics 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html)
+ Adobe 클라이언트 데이터 레이어(ACDL)

   + [AEM WCM 핵심 구성 요소에서 ACDL 확장](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [AEM WCM 핵심 구성 요소와 ACDL 통합](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [ACDL을 통한 이벤트 기반 데이터 처리](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [ACDL(Adobe 클라이언트 데이터 레이어) 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)

### AEM Assets

+ [자산 통찰력 개요](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [자산 통찰력 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [자산 통찰력 튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [Adobe Analytics 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

### AEM Sites

+ [Adobe Campaign Classic과 통합](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html#configure-user)
+ [Adobe Experience Manager 뉴스레터 만들기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html)
+ [AEM 이메일 핵심 구성 요소 설명서](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

Adobe Commerce은 AEM as a Cloud Service과 통합되므로 브랜드가 상거래 경험을 차별화하고 온라인 지출을 빠르게 포착하기 위해 더 빠르게 확장 및 혁신할 수 있습니다. AEM with Commerce는 Experience Manager에 있는 몰입형, 옴니채널 및 개인화된 경험을 다양한 상거래 솔루션과 결합하여 쇼핑 여정의 모든 부분에 차별화된 경험을 제공하여 가치 창출 시간을 줄이고 높은 전환을 유도합니다.

### AEM Sites

+ [AEM Content 및 Commerce 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platform 태그

Adobe Experience Platform 태그(이전 Adobe Launch, DTM)는 AEM과 원활하게 통합되므로 배포 및 관리를 위한 간단한 방법을 제공합니다 [analytics](#adobe-analytics), [타겟팅](#adobe-target)고객 경험을 사로잡는 데 필요한 , 마케팅 및 광고 태그.

### AEM Sites

+ [Experience Platform 태그 사용 안내서](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform 태그 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

### AEM Forms

+ [Experience Platform 태그 사용 안내서](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform 태그 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

## Adobe Journey Optimizer

Adobe Journey Optimizer은 단일 애플리케이션에서 수백만 고객과 함께하는 옴니채널 캠페인과 일대일 순간을 예약하도록 도와주며, 지능적인 의사 결정과 통찰력으로 전체 여정을 최적화합니다.

### AEM Assets

+ [AEM Assets Essentials를 Adobe Journey Optimizer과 통합](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html?lang=ko-KR)

## Adobe 학습 관리자

Adobe Learning Manager(이전 Adobe Captivate Prime)는 고객 및 직원에게 개인화된 학습을 제공합니다.

### AEM Sites

+ [AEM Sites과 Adobe Learning Manager 통합](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html)

## Adobe Sensei

Adobe Sensei은 스마트 태그, 스마트 자르기, 시각적 검색 등을 통해 콘텐츠 관리 프로세스를 변환하는 AI 및 머신 러닝 기술을 제공합니다!

### AEM Sites

+ [콘텐츠 조각의 텍스트 요약](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [이미지용 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [이미지용 사용자 지정 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [비디오용 스마트 태그](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [스마트 자르기](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [시각적 검색](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [Automated forms conversion 서비스](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target은 AEM as a Cloud Service과 통합되어 AEM의 콘텐츠를 기반으로 모든 최종 사용자에게 최적화된 웹 경험을 제공합니다.

### AEM Sites

+ [Adobe Target 통합 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ Target에 경험 조각

   + [Target에 경험 조각 게시](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [경험 조각을 JSON으로 Target에 게시](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [Target에서 AEM Context Hub 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sites 및 Target 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)

## Adobe Workfront

Adobe Workfront과 AEM s a Cloud Service의 통합을 통해 디지털 에셋 생성, 공동 작업 및 라이프사이클 관리 프로세스를 간소화할 수 있습니다.

### AEM Assets

+ [Workfront 강화 커넥터 구성](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
+ [Workfront 강화 커넥터 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets 기본 사항

   + [Adobe Workfront for Assets Essentials 사용 안내서](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront 및 Assets Essentials 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
