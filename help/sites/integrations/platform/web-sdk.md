---
title: Experience Platform 웹 SDK 통합
description: AEM as a Cloud Service을 Experience Platform 웹 SDK와 통합하는 방법을 알아봅니다. 이 기본 단계는 Adobe Analytics, Target 또는 Real-time Customer Data Platform, Customer Journey Analytics, Journey Optimizer과 같은 최신 혁신적인 제품과 같은 Adobe Experience Cloud 제품을 통합하는 데 필수적인 단계입니다.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 63afa03de70d6f8f695d552018344d53a5cec6f5
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 3%

---

# Experience Platform 웹 SDK 통합

AEM as a Cloud Service을 Experience Platform과 통합하는 방법을 알아봅니다 [웹 SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 이 기본 단계는 Adobe Analytics, Target 또는 Real-time Customer Data Platform, Customer Journey Analytics, Journey Optimizer과 같은 최신 혁신적인 제품과 같은 Adobe Experience Cloud 제품을 통합하는 데 필수적인 단계입니다.

또한 수집 및 전송 방법을 학습합니다 [WKND - 샘플 Adobe Experience Manager 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 페이지 보기 데이터의 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

이 설정을 완료한 후 솔리드 파운데이션을 구현했습니다. 또한 와 같은 애플리케이션을 사용하여 Experience Platform 구현을 개선할 준비가 되어 있습니다. [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ko), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), 및 [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). 고급 구현은 웹 및 고객 데이터를 표준화하여 고객 참여를 높이는 데 도움이 됩니다.

## 사전 요구 사항

Experience Platform Web SDK를 통합할 때 다음이 필요합니다.

in **AEM as Cloud Service**:

+ AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스 권한
+ Cloud Manager에 대한 배포 관리자 액세스
+ 복제 및 배포 [WKND - 샘플 Adobe Experience Manager 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM as a Cloud Service 환경으로 이동하십시오.

in **Experience Platform**:

+ 기본 프로덕션에 액세스, **Prod** 샌드박스
+ 액세스 권한 **스키마** 데이터 관리
+ 액세스 권한 **데이터 세트** 데이터 관리
+ 액세스 권한 **데이터 스트림** 데이터 수집에서
+ 액세스 권한 **태그** (이전에는 Launch라고 함)

필요한 권한이 없는 경우 시스템 관리자는 [Adobe Admin Console](https://adminconsole.adobe.com/) 필요한 권한을 부여할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM 스키마 만들기 - Experience Platform

XDM(경험 데이터 모델) 스키마를 사용하면 고객 경험 데이터를 표준화할 수 있습니다. 를 수집하려면 **WKND 페이지 보기** 데이터, XDM 스키마 만들기 및 Adobe 제공 필드 그룹 사용 `AEP Web SDK ExperienceEvent` 을 참조하십시오.

소매, 금융 서비스, 의료 등 특정 범용 및 산업마다 참조 데이터 모델 세트가 있습니다. [업계 데이터 모델 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) 추가 정보.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

XDM 스키마 및 필드 그룹, 유형, 클래스 및 데이터 유형과 같은 관련 개념에 대해 알아봅니다. [XDM 시스템 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

다음 [XDM 시스템 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 는 XDM 스키마 및 필드 그룹, 유형, 클래스 및 데이터 유형과 같은 관련 개념에 대해 학습하기 위한 훌륭한 리소스입니다. XDM 데이터 모델에 대한 포괄적인 이해를 제공하고 XDM 스키마를 만들어 관리하여 기업 전체의 데이터를 표준화하는 방법을 제공합니다. XDM 스키마와 데이터 수집 및 관리 프로세스에 어떤 이점을 줄 수 있는지 자세히 알아보려면 이 스키마를 살펴보십시오.

## 데이터 스트림 만들기 - Experience Platform

데이터 스트림은 수집된 데이터를 보낼 플랫폼 에지 네트워크에 지시합니다. 예를 들어 Experience Platform, Analytics 또는 Adobe Target으로 전송할 수 있습니다.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

를 방문하여 데이터 저장소의 개념과 데이터 거버넌스 및 구성과 같은 관련 항목에 대해 숙지하십시오 [데이터 스트림 개요](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 페이지.

## 태그 속성 만들기 - Experience Platform

웹 SDK JavaScript 라이브러리를 WKND 웹 사이트에 추가하기 위해 Experience Platform에서 태그(이전의 Launch) 속성을 만드는 방법을 알아봅니다. 새로 정의된 태그 속성에는 다음 리소스가 있습니다.

+ 태그 확장: [코어](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 및 [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 데이터 요소: WKND 사이트의 Adobe 클라이언트 데이터 레이어를 사용하여 페이지 이름, 사이트 섹션 및 호스트 이름을 추출하는 사용자 지정 코드 유형의 데이터 요소입니다. 또한, 이전에 새로 만든 WKND XDM 스키마 빌드를 따르는 XDM 개체 유형 데이터 요소입니다 [XDM 스키마 만들기](#create-xdm-schema---experience-platform) 단계.
+ 규칙: 트리거된 Adobe 클라이언트 데이터 레이어를 사용하여 WKND 웹 페이지를 방문할 때마다 Platform Edge Network에 데이터를 보냅니다 `cmp:show` 이벤트.

를 사용하여 태그 라이브러리를 빌드하고 게시하는 동안 **게시 흐름**&#x200B;를 사용하여 다음을 수행할 수 있습니다. **변경된 모든 리소스 추가** 버튼을 클릭합니다. 개별 리소스를 식별하고 선택하는 대신 데이터 요소, 규칙 및 태그 확장과 같은 모든 리소스를 선택합니다. 또한 개발 단계 동안 라이브러리를에 게시할 수 있습니다. _개발_ 환경을 확인한 다음 을(를) 확인 및 프로모션합니다 _단계_ 또는 _프로덕션_ 환경.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>비디오에 표시된 데이터 요소 및 규칙 이벤트 코드를 참조에 사용할 수 있습니다. **아래 아코디언 요소를 확장합니다.**. 그러나 Adobe 클라이언트 데이터 계층을 사용하지 않는 경우에는 아래 코드를 수정해야 하지만, 데이터 요소를 정의하고 규칙 정의에서 사용하는 개념이 계속 적용됩니다.


+++ 데이터 요소 및 규칙-이벤트 코드

+ 다음 `Page Name` 데이터 요소 코드.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ 다음 `Site Section` 데이터 요소 코드.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ 다음 `Host Name` 데이터 요소 코드.

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ 다음 `all pages - on load` 규칙-이벤트 코드

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


다음 [태그 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 는 데이터 요소, 규칙 및 확장과 같은 중요한 개념에 대한 심층적인 지식을 제공합니다.

AEM 코어 구성 요소와 Adobe 클라이언트 데이터 레이어 통합에 대한 자세한 내용은 [AEM 핵심 구성 요소로 Adobe 클라이언트 데이터 레이어 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## AEM에 태그 속성 연결

AEM에서 Adobe IMS 및 Adobe Launch 구성을 통해 최근에 만든 태그 속성을 AEM에 연결하는 방법을 알아봅니다. AEM as a Cloud Service 환경이 설정되면 Adobe Launch를 포함하여 여러 Adobe IMS 기술 계정 구성이 자동으로 생성됩니다. 그러나 AEM 6.5 버전의 경우 수동으로 구성해야 합니다.

태그 속성을 연결한 후 WKND 사이트는 Launch 클라우드 서비스 구성을 사용하여 태그 속성의 JavaScript 라이브러리를 웹 페이지에 로드할 수 있습니다.

### WKND에서 태그 속성 로드 확인

Adobe Experience Platform Debugger 사용 [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 또는 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 확장 프로그램에서 태그 속성이 WKND 페이지에서 로드되는지 확인합니다. 확인할 수 있습니다

+ 확장, 버전, 이름 등의 태그 속성 세부 사항입니다.
+ Platform Web SDK 라이브러리 버전, 데이터 스트림 ID
+ XDM 개체의 일부 `events` 웹 SDK의 속성

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 데이터 집합 만들기 - Experience Platform

웹 SDK를 사용하여 수집한 페이지 보기 데이터는 Experience Platform 데이터 레이크에 데이터 세트로 저장됩니다. 데이터 집합은 스키마를 따르는 데이터베이스 테이블과 같은 데이터 수집을 위한 저장 및 관리 구성입니다. 데이터 세트를 만들고 이전에 만든 데이터 스트림을 구성하여 Experience Platform으로 데이터를 보내는 방법을 알아봅니다.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

다음 [데이터 세트 개요](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 개념, 구성 및 기타 수집 기능에 대한 자세한 내용을 제공합니다.


## Experience Platform의 WKND 페이지 보기 데이터

AEM, 특히 WKND 사이트에서를 사용하여 웹 SDK를 설정한 후에는 사이트 페이지를 탐색하여 트래픽을 생성할 차례입니다. 그런 다음 페이지 보기 데이터를 Experience Platform 데이터 세트에 수집하는 중인지 확인합니다. 데이터 세트 UI 내에서 총 레코드, 크기 및 수집된 일괄 처리와 같은 다양한 세부 정보가 시각적으로 호소력 있는 막대 그래프와 함께 표시됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 요약

좋습니다! 웹 사이트에서 데이터를 수집하고 수집하기 위해 Experience Platform Web SDK와 함께 AEM 설정을 완료했습니다. 이제 이 기반을 통해 Analytics, Target, CJA(Customer Journey Analytics) 및 기타 여러 제품과 같은 제품을 개선하고 통합하여 고객을 위한 개인화된 풍부한 경험을 만들 수 있습니다. 계속해서 Adobe Experience Cloud의 잠재력을 최대한 발휘하기 위해 학습하고 탐구하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## 추가 리소스

+ [핵심 구성 요소로 함께 Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [Experience Platform 데이터 수집 태그와 AEM 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK 및 Edge 네트워크 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [데이터 수집 튜토리얼](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
