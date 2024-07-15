---
title: Platform Web SDK와 AEM Sites 및 Adobe Analytics 통합
description: 최신 Platform Web SDK 접근 방식을 사용하여 AEM Sites 및 Adobe Analytics을 통합합니다.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# Platform Web SDK와 AEM Sites 및 Adobe Analytics 통합

Platform Web SDK를 사용하여 Adobe Experience Manager(AEM)와 Adobe Analytics을 통합하는 방법에 대한 **최신 접근 방식**&#x200B;을 알아봅니다. 이 포괄적인 튜토리얼은 [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 페이지 보기 및 CTA 클릭 데이터를 원활하게 수집하는 과정을 안내합니다. 다양한 지표와 차원을 탐색할 수 있는 Adobe Analysis Workspace에서 수집된 데이터를 시각화하여 중요한 통찰력을 얻으십시오. 또한 플랫폼 데이터 세트를 탐색하여 데이터를 확인하고 분석합니다. 이 여정에 참여하여 데이터 기반 의사 결정을 위한 AEM 및 Adobe Analytics의 기능을 활용하십시오.

## 개요

사용자 행동에 대한 통찰력을 얻는 것은 모든 마케팅 팀에 중요한 목표입니다. 팀은 사용자가 콘텐츠와 상호 작용하는 방법을 이해함으로써 정보에 입각한 결정을 내리고 전략을 최적화하며 더 나은 결과를 얻을 수 있습니다. 가상의 엔티티인 WKND 마케팅 팀은 이 목표를 달성하기 위해 웹 사이트에서 Adobe Analytics을 구현하도록 목표를 설정했습니다. 기본 목표는 페이지 보기 수와 홈페이지 콜 투 액션(CTA) 클릭수의 두 가지 주요 지표에 대한 데이터를 수집하는 것입니다.

페이지 보기를 추적하여 어떤 페이지가 사용자의 가장 큰 관심을 받는지 분석할 수 있습니다. 또한 홈페이지 CTA 클릭 수를 추적하면 팀의 콜 투 액션 요소의 효과에 대한 중요한 통찰력을 얻을 수 있습니다. 이 데이터는 어떤 CTA가 사용자와 공감 중이며, 조정이 필요한 CTA를 식별하며, 사용자 참여를 강화하고 전환을 유도하는 새로운 기회를 포착할 수 있습니다.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## 사전 요구 사항

Platform Web SDK를 사용하여 Adobe Analytics을 통합할 때 필요한 사항은 다음과 같습니다.

**[Experience Platform Web SDK 통합](./web-sdk.md)** 자습서에서 설정 단계를 완료했습니다.

**Cloud Service으로 AEM**&#x200B;에서:

+ [AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html)
+ Cloud Manager에 대한 배포 관리자 액세스
+ [WKND - 샘플 Adobe Experience Manager 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)를 복제하여 AEM as a Cloud Service 환경에 배포합니다.

**Adobe Analytics**&#x200B;에서:

+ **보고서 세트 만들기**&#x200B;에 액세스
+ **Analysis Workspace** 만들기 액세스

**Experience Platform**&#x200B;에서:

+ 기본 프로덕션 **Prod** 샌드박스에 액세스합니다.
+ 데이터 관리 아래의 **스키마**&#x200B;에 액세스
+ 데이터 관리에서 **데이터 세트**&#x200B;에 액세스
+ 데이터 수집 아래의 **데이터스트림**&#x200B;에 액세스
+ 데이터 수집 아래의 **태그**&#x200B;에 액세스

필요한 권한이 없는 경우 [Adobe Admin Console](https://adminconsole.adobe.com/)을(를) 사용하는 시스템 관리자가 필요한 권한을 부여할 수 있습니다.

Platform Web SDK를 사용하여 AEM과 Analytics의 통합 프로세스를 살펴보기 전에 [Experience Platform Web SDK 통합](./web-sdk.md) 자습서에서 설정한 필수 구성 요소와 주요 요소를 _다시 살펴보도록 하겠습니다._ 이는 통합을 위한 견고한 기반을 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

XDM 스키마, 데이터스트림, 데이터 세트, 태그 속성 및 AEM과 태그 속성 연결을 다시 정리한 후 통합 여정을 시작하겠습니다.

## Analytics 솔루션 디자인 참조(SDR) 문서 정의

구현 프로세스의 일부로 솔루션 디자인 참조(SDR) 문서를 만드는 것이 좋습니다. 이 문서는 비즈니스 요구 사항을 정의하고 효과적인 데이터 수집 전략을 설계하는 청사진으로서 중요한 역할을 합니다.

SDR 문서는 구현 계획에 대한 포괄적인 개요를 제공하여 모든 이해 당사자가 일치하고 프로젝트의 목표와 범위를 이해할 수 있도록 합니다.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

SDR 문서에 포함되어야 하는 개념 및 다양한 요소에 대한 자세한 내용은 [솔루션 디자인 참조(SDR) 문서 만들기 및 유지 관리](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html)를 참조하십시오. 샘플 Excel 템플릿을 다운로드할 수도 있지만 WKND별 버전은 [여기](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx)에서 사용할 수 있습니다.

## Analytics 설정 - 보고서 세트, Analysis Workspace

첫 번째 단계는 Adobe Analytics, 특히 전환 변수(또는 eVar)와 성공 이벤트를 포함하는 보고서 세트를 설정하는 것입니다. 전환 변수는 원인과 결과를 측정하는 데 사용됩니다. 성공 이벤트는 작업을 추적하는 데 사용됩니다.

이 자습서에서는 `eVar5, eVar6, and eVar7`이(가) _WKND 페이지 이름, WKND CTA ID 및 WKND CTA 이름_&#x200B;을(를) 각각 추적하고 `event7`을(를) 사용하여 _WKND CTA 클릭 이벤트_&#x200B;를 추적합니다.

수집된 데이터에서 분석, 통찰력 수집 및 다른 사람과 그러한 통찰력을 공유하려면 Analysis Workspace의 프로젝트가 만들어집니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Analytics 설정 및 개념에 대해 자세히 알아보려면 다음 리소스를 사용하는 것이 좋습니다.

+ [보고서 세트](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [전환 변수](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [성공 이벤트](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## 데이터스트림 업데이트 - Analytics 서비스 추가

데이터 스트림은 Platform Edge Network에 수집된 데이터를 보낼 위치를 지시합니다. [이전 자습서](./web-sdk.md)에서 데이터 스트림이 데이터를 Experience Platform에 보내도록 구성되어 있습니다. 이 데이터 스트림은 [위](#setup-analytics---report-suite-analysis-workspace) 단계에서 구성된 Analytics 보고서 세트로 데이터를 보내도록 업데이트됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## XDM 스키마 만들기

XDM(경험 데이터 모델) 스키마를 사용하면 수집된 데이터를 표준화할 수 있습니다. [이전 자습서](./web-sdk.md)에서 필드 그룹이 `AEP Web SDK ExperienceEvent`인 XDM 스키마가 만들어집니다. 또한 이 XDM 스키마를 사용하여 Experience Platform에 수집된 데이터를 저장하는 데이터 세트가 만들어집니다.

단, 해당 XDM 스키마에는 eVar, 이벤트 데이터를 전송할 Adobe Analytics 관련 필드 그룹이 없습니다. 플랫폼에 eVar, 이벤트 데이터가 저장되지 않도록 기존 스키마를 업데이트하는 대신 새 XDM 스키마를 만듭니다.

새로 만든 XDM 스키마에 `AEP Web SDK ExperienceEvent` 및 `Adobe Analytics ExperienceEvent Full Extension` 필드 그룹이 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## 태그 속성 업데이트

[이전 자습서](./web-sdk.md)에서 태그 속성이 만들어지고 페이지 보기 데이터를 수집, 매핑 및 전송하는 데이터 요소와 규칙이 있습니다. 다음을 위해 향상되어야 합니다.

+ 페이지 이름을 `eVar5`에 매핑
+ **pageview** Analytics 호출 트리거(또는 비콘 보내기)
+ Adobe 클라이언트 데이터 레이어를 사용하여 CTA 데이터 수집
+ CTA ID 및 이름을 각각 `eVar6` 및 `eVar7`에 매핑합니다. 또한 `event7`에 대한 CTA 클릭 수입니다.
+ **링크 클릭** Analytics 호출 트리거(또는 비콘 보내기)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>비디오에 표시된 데이터 요소 및 규칙 이벤트 코드를 참조할 수 있습니다. **아래 아코디언 요소를 확장**. 그러나 Adobe 클라이언트 데이터 레이어를 사용하지 않는 경우 아래 코드를 수정해야 하지만 데이터 요소를 정의하고 규칙 정의에 사용하는 개념이 여전히 적용됩니다.

+++ 데이터 요소 및 규칙 이벤트 코드

+ `Component ID` 데이터 요소 코드입니다.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ `Component Name` 데이터 요소 코드입니다.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ `all pages - on load` **Rule-Condition** 코드

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ `home page - cta click` **Rule-Event** 코드

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ `home page - cta click` **Rule-Condition** 코드

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

AEM 핵심 구성 요소와 Adobe 클라이언트 데이터 레이어 통합에 대한 자세한 내용은 [AEM 핵심 구성 요소와 Adobe 클라이언트 데이터 레이어 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)를 참조하십시오.


>[!INFO]
>
>SDR(솔루션 디자인 참조) 문서의 **변수 맵** 탭 속성 세부 정보에 대한 포괄적인 이해를 위해 [여기](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx)에서 다운로드할 완료된 WKND별 버전에 액세스하십시오.



## WKND에서 업데이트된 태그 속성 확인

업데이트된 태그 속성이 빌드, 게시되고 WKND 사이트 페이지에서 올바르게 작동하는지 확인합니다. Google Chrome 웹 브라우저의 [Adobe Experience Platform Debugger 확장](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 사용:

+ 태그 속성이 최신 버전인지 확인하려면 빌드 날짜를 확인합니다.

+ PageView와 HomePage CTA 클릭 모두에 대한 XDM 이벤트 데이터를 확인하려면 확장 프로그램 내의 웹 SDK Experience Platform 메뉴 옵션을 사용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## 웹 트래픽 시뮬레이션 - Selenium 자동화

테스트 목적으로 의미 있는 양의 트래픽을 생성하기 위해 Selenium 자동화 스크립트가 개발됩니다. 이 사용자 지정 스크립트는 페이지 보기 및 CTA 클릭과 같은 WKND 웹 사이트와의 사용자 상호 작용을 시뮬레이션합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## 데이터 세트 확인 - WKND 페이지 보기, CTA 데이터

데이터 집합은 스키마를 따르는 데이터베이스 테이블과 같은 데이터 수집을 위한 저장 및 관리 구성입니다. [이전 자습서](./web-sdk.md)에서 만든 데이터 집합을 다시 사용하여 페이지 보기 및 CTA 클릭 데이터가 Experience Platform 데이터 집합에 수집되는지 확인합니다. 데이터 세트 UI 내에서 총 레코드, 크기 및 수집된 일괄 처리와 같은 다양한 세부 정보가 시각적으로 호소력 있는 막대 그래프와 함께 표시됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND 페이지 보기, CTA 데이터 시각화

Analysis Workspace은 유연하고 대화형 방식으로 데이터를 탐색하고 시각화할 수 있는 Adobe Analytics의 강력한 도구입니다. 사용자 지정 보고서를 만들고, 고급 세분화를 수행하고, 다양한 데이터 시각화를 적용할 수 있는 드래그 앤 드롭 인터페이스를 제공합니다.

[Analytics 설정](#setup-analytics---report-suite-analysis-workspace) 단계에서 만든 Analysis Workspace 프로젝트를 다시 열겠습니다. **상위 페이지** 섹션에서 방문 횟수, 고유 방문자 수, 시작 횟수, 바운스 비율 등과 같은 다양한 지표를 검사합니다. WKND 페이지 및 홈 페이지 CTA의 성능을 평가하려면 WKND별 차원(WKND 페이지 이름, WKND CTA 이름) 및 지표(WKND CTA 클릭 이벤트)를 드래그 앤 드롭합니다. 이러한 통찰력은 마케터가 어떤 CTA가 더 효과적인지 이해하고 비즈니스 목표에 맞게 데이터 중심의 결정을 내리는 데 유용합니다.

사용자 여정을 시각화하려면 **WKND 페이지 이름**&#x200B;부터 시작하여 다양한 경로로 확장하는 흐름 시각화를 사용하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## 요약

좋습니다! Platform Web SDK를 사용하여 페이지 보기 및 CTA 클릭 데이터를 수집하고, 분석하기 위한 AEM 및 Adobe Analytics 설정을 완료했습니다.

마케팅 팀이 콘텐츠를 최적화하고 데이터 기반 결정을 내릴 수 있도록 하기 위해, Adobe Analytics 구현은 사용자 행동에 대한 통찰력을 얻고 정보에 입각한 결정을 내리는 데 매우 중요합니다.

권장 단계를 구현하고 솔루션 디자인 참조(SDR) 문서와 같은 제공된 리소스를 사용하며 주요 Analytics 개념을 이해하면 마케터가 데이터를 효과적으로 수집하고 분석할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>개별 설정 단계 비디오 대신 전체 통합 프로세스를 다루는 **종단 간 비디오**&#x200B;를 선호하는 경우 [여기](https://video.tv.adobe.com/v/3419889/)를 클릭하여 액세스할 수 있습니다.


## 추가 리소스

+ [Experience Platform Web SDK 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [Experience Platform 데이터 수집 태그와 AEM 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform 웹 SDK 및 Edge Network 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [데이터 수집 튜토리얼](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger 개요](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
