---
title: Adobe Target 내에서 AEM 경험 조각 오퍼 사용
seo-title: Adobe Target 내에서 AEM 경험 조각 오퍼 사용
description: Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 혁신합니다. 이제 AEM에서 만든 경험을 HTML 오퍼로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트하고 개인화할 수 있습니다.
seo-description: Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 혁신합니다. 이제 AEM에서 만든 경험을 HTML 오퍼로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트하고 개인화할 수 있습니다.
sub-product: 컨텐츠 서비스
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# Adobe Target{#using-experience-fragment-offers-within-adobe-target} 내에서 경험 조각 오퍼 사용

Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 혁신합니다. 이제 AEM에서 만든 경험을 HTML 오퍼로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트하고 개인화할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>at.js 클라이언트 라이브러리를 사용하는 것이 권장되며 최상의 방법은 Launch by Adobe, Adobe DTM 또는 타사 태그 관리 솔루션과 같은 태그 관리 솔루션을 사용하여 사이트 페이지에 대상 라이브러리를 추가하는 것입니다

>[!NOTE]
>
>ADOBE TARGET 내의 AEM 경험 조각 오퍼는 AEM 6.3 사용자를 위한 기능 팩으로도 사용할 수 있습니다. 기능 팩 및 종속성에 대해서는 아래 섹션을 참조하십시오.


* Adobe Experience Manager의 사용이 간편하고 강력한 컨텐츠 제작 메커니즘(Adobe Target AI) 및 머신 러닝)을 통해 컨텐츠 작성자는 중앙에서 모든 채널에 배포할 컨텐츠를 제작하고 관리할 수 있습니다. 경험 조각을 HTML 오퍼로 Adobe Target으로 내보낼 수 있는 기능을 통해 마케터는 이러한 오퍼를 사용하여 보다 개인화된 경험을 제작할 수 있고, 각 경험을 테스트하고 확장할 수 있습니다.
* HTML 오퍼와 경험 조각 오퍼의 주요 차이점은 나중 버전의 편집은 AEM에서만 수행한 다음 Adobe Target과 동기화할 수 있다는 것입니다
* 경험 조각 폴더에 적용된 Target 클라우드 서비스 구성은 상위 폴더 바로 아래에 만들어진 모든 경험 조각으로 상속됩니다. 하위 폴더는 상위 클라우드 서비스 구성을 상속하지 않습니다.
* 개인화된 제안을 만들기 위해 이제 AEM에 저장된 컨텐츠를 손쉽게 활용할 수 있습니다.
* 자동 할당, 자동 Target 및 Automated Personalization과 같은 Sensei 기반의 활동을 비롯한 Target 활동 유형을 만들 수 있습니다

## AEM 6.3 기능 팩 및 종속성 {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | 종속성 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-2442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## 추가 리소스 {#additional-resources}

* [경험 조각 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [경험 조각 사용](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
