---
title: AEM Sites 및 Experience Platform 웹 SDK 통합
description: AEM Sites as a Cloud Service을 Experience Platform 웹 SDK와 통합하는 방법을 알아봅니다. 이 기본 단계는 Adobe Analytics, Target과 같은 Adobe Experience Cloud 제품이나 Real-time Customer Data Platform, Customer Journey Analytics 및 Journey Optimizer과 같은 최신 혁신 제품을 통합하는 데 필수적입니다.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1303
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 1%

---

# AEM Sites 및 Experience Platform 웹 SDK 통합

AEMas a Cloud Service 을 Experience Platform과 통합하는 방법에 대해 알아봅니다 [웹 SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 이 기본 단계는 Adobe Analytics, Target과 같은 Adobe Experience Cloud 제품이나 Real-time Customer Data Platform, Customer Journey Analytics 및 Journey Optimizer과 같은 최신 혁신 제품을 통합하는 데 필수적입니다.

또한 수집 및 전송 방법을 배웁니다. [WKND - 샘플 Adobe Experience Manager 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 에서 페이지 보기 데이터 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

이 설정을 완료한 후 견고한 기반을 구현했습니다. 또한 다음과 같은 애플리케이션을 사용하여 Experience Platform 구현을 진행할 준비가 되었습니다 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), 및 [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). 고급 구현은 웹 및 고객 데이터를 표준화하여 더 나은 고객 참여를 유도하는 데 도움이 됩니다.

## 사전 요구 사항

Experience Platform Web SDK를 통합할 때 필요한 사항은 다음과 같습니다.

위치 **Cloud Service으로 AEM**:

+ AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스
+ Cloud Manager에 대한 배포 관리자 액세스
+ 복제 및 배포 [WKND - 샘플 Adobe Experience Manager 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM as a Cloud Service 환경으로

위치 **Experience Platform**:

+ 기본 프로덕션에 액세스, **Prod** 샌드박스.
+ 액세스 대상: **스키마** 데이터 관리 아래
+ 액세스 대상: **데이터 세트** 데이터 관리 아래
+ 액세스 대상: **데이터스트림** 데이터 수집 아래
+ 액세스 대상: **태그** 데이터 수집 아래

필요한 권한이 없는 경우 시스템 관리자는 [Adobe Admin Console](https://adminconsole.adobe.com/) 필요한 권한을 부여할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM 스키마 만들기 - Experience Platform

XDM(경험 데이터 모델) 스키마를 사용하면 고객 경험 데이터를 표준화할 수 있습니다. 을(를) 수집하려면 **WKND 페이지 보기** 데이터, XDM 스키마 생성 및 Adobe 제공 필드 그룹 사용 `AEP Web SDK ExperienceEvent` 웹 데이터 수집용.

소매, 금융 서비스, 의료 서비스 등과 같은 범용 및 업종별 참조 데이터 모델이 있습니다. [업계 데이터 모델 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) 추가 정보.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

에서 XDM 스키마 및 필드 그룹, 유형, 클래스 및 데이터 유형과 같은 관련 개념에 대해 알아봅니다. [XDM 시스템 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

다음 [XDM 시스템 개요](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 는 XDM 스키마 및 필드 그룹, 유형, 클래스 및 데이터 유형과 같은 관련 개념에 대해 학습할 수 있는 유용한 리소스입니다. XDM 데이터 모델과 XDM 스키마를 만들고 관리하여 기업 전반의 데이터를 표준화하는 방법에 대한 포괄적인 이해를 제공합니다. 이를 살펴보고 XDM 스키마와 이것이 데이터 수집 및 관리 프로세스에 도움이 되는 방법에 대해 더 깊이 이해하십시오.

## 데이터 스트림 만들기 - Experience Platform

데이터 스트림은 Platform Edge Network에 수집된 데이터를 보낼 위치를 지시합니다. 예를 들어 Experience Platform, Analytics 또는 Adobe Target으로 전송할 수 있습니다.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

다음을 방문하여 데이터스트림의 개념과 데이터 거버넌스 및 구성과 같은 관련 항목에 대해 숙지하십시오. [데이터스트림 개요](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 페이지를 가리키도록 업데이트하는 중입니다.

## 태그 만들기 속성 - Experience Platform

Experience Platform에서 태그 속성을 만들어 WKND 웹 사이트에 웹 SDK JavaScript 라이브러리를 추가하는 방법을 알아봅니다. 새로 정의된 태그 속성에는 다음 리소스가 있습니다.

+ 태그 확장 기능: [코어](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 및 [Adobe Experience Platform 웹 SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 데이터 요소: WKND 사이트의 Adobe 클라이언트 데이터 레이어를 사용하여 페이지 이름, 사이트 섹션 및 호스트 이름을 추출하는 사용자 지정 코드 유형의 데이터 요소입니다. 또한 이전에 새로 생성된 WKND XDM 스키마 빌드를 준수하는 XDM 개체 유형 데이터 요소입니다 [XDM 스키마 만들기](#create-xdm-schema---experience-platform) 단계.
+ 규칙: 트리거된 Adobe 클라이언트 데이터 레이어를 사용하여 WKND 웹 페이지를 방문할 때마다 플랫폼 Edge Network으로 데이터 전송 `cmp:show` 이벤트.

를 사용하여 태그 라이브러리를 빌드하고 게시하는 동안 **게시 플로우**, 다음을 사용할 수 있습니다. **변경된 모든 리소스 추가** 단추를 클릭합니다. 개별 리소스를 식별하고 선택하는 대신 데이터 요소, 규칙 및 태그 확장과 같은 모든 리소스를 선택합니다. 또한 개발 단계에서는 라이브러리를 _개발_ 환경을 확인한 다음 로 승격합니다. _단계_ 또는 _프로덕션_ 환경.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>비디오에 표시된 데이터 요소 및 규칙 이벤트 코드를 참조에 사용할 수 있습니다. **아래 아코디언 요소 펼치기**. 그러나 Adobe 클라이언트 데이터 레이어를 사용하지 않는 경우 아래 코드를 수정해야 하지만 데이터 요소를 정의하고 규칙 정의에 사용하는 개념이 여전히 적용됩니다.


+++ 데이터 요소 및 규칙 이벤트 코드

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

+ 다음 `all pages - on load` 규칙 이벤트 코드

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


다음 [태그 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 는 데이터 요소, 규칙 및 확장과 같은 중요한 개념에 대한 심층적인 지식을 제공합니다.

AEM 핵심 구성 요소와 Adobe 클라이언트 데이터 레이어 통합에 대한 자세한 내용은 [AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## Tag 속성을 AEM에 연결

AEM의 Adobe Experience Platform 구성에서 Adobe IMS 및 태그를 통해 최근에 만든 태그 속성을 AEM에 연결하는 방법을 알아봅니다. AEM as a Cloud Service 환경이 설정되면 태그를 비롯한 여러 Adobe IMS 기술 계정 구성이 자동으로 생성됩니다. 그러나 AEM 6.5 버전의 경우 수동으로 구성해야 합니다.

WKND 사이트는 태그 속성을 연결한 후 Adobe Experience Platform 클라우드 서비스 구성의 태그를 사용하여 웹 페이지에 태그 속성의 JavaScript 라이브러리를 로드할 수 있습니다.

### WKND에서 태그 속성 로드 확인

Adobe Experience Platform Debugger 사용 [크롬](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 확장에서 태그 속성이 WKND 페이지에 로드되는지 확인합니다. 확인할 수 있습니다.

+ 확장, 버전, 이름 등 태그 속성 세부 정보.
+ Platform Web SDK 라이브러리 버전, 데이터 스트림 ID
+ 부분으로서의 XDM 개체 `events` Experience Platform 웹 SDK의 속성

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 데이터 세트 만들기 - Experience Platform

Web SDK를 사용하여 수집된 페이지 보기 데이터는 Experience Platform 데이터 레이크에 데이터 세트로 저장됩니다. 데이터 집합은 스키마를 따르는 데이터베이스 테이블과 같은 데이터 수집을 위한 저장 및 관리 구성입니다. 데이터 세트를 만들고 이전에 만든 데이터 스트림을 구성하여 Experience Platform으로 데이터를 전송하는 방법을 알아봅니다.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

다음 [데이터 세트 개요](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 는 개념, 구성 및 기타 수집 기능에 대한 자세한 정보를 제공합니다.


## Experience Platform의 WKND 페이지 보기 데이터

특히 WKND 사이트에서 AEM으로 Web SDK를 설정한 후에는 사이트 페이지를 탐색하여 트래픽을 생성할 차례입니다. 그런 다음 페이지 보기 데이터가 Experience Platform 데이터 세트에 수집되는지 확인합니다. 데이터 세트 UI 내에서 총 레코드, 크기 및 수집된 일괄 처리와 같은 다양한 세부 정보가 시각적으로 호소력 있는 막대 그래프와 함께 표시됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 요약

좋습니다! 웹 사이트에서 데이터를 수집하고 수집하기 위해 Experience Platform Web SDK로 AEM 설정을 완료했습니다. 이러한 기반을 통해 이제 Analytics, Target, CJA(Customer Journey Analytics) 등과 같은 제품을 개선하고 통합하여 고객을 위한 풍부하고 개인화된 경험을 만들 수 있는 추가 가능성을 살펴볼 수 있습니다. Adobe Experience Cloud의 잠재력을 최대한 발휘할 수 있도록 계속해서 학습하고 탐색하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>원하는 경우 **엔드투엔드 비디오** 따라서 개별 설정 단계 비디오 대신 전체 통합 프로세스를 다루는 [여기](https://video.tv.adobe.com/v/3418905/) 액세스할 수 있습니다.

## 추가 리소스

+ [핵심 구성 요소로 Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [Experience Platform 데이터 수집 태그와 AEM 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK 및 Edge Network 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [데이터 수집 자습서](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
