---
title: AEM Sites에서 리소스 상태 개발
description: 'Adobe Experience Manager의 리소스 상태 API는 AEM의 다양한 편집기 웹 UI에서 상태 메시징을 노출할 수 있는 연결 가능한 프레임워크입니다. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 리소스 상태 개발 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager의 리소스 상태 API는 AEM의 다양한 편집기 웹 UI에서 상태 메시징을 노출할 수 있는 연결 가능한 프레임워크입니다.

## 개요 {#overview}

편집자 리소스 상태 프레임워크는 표준 및 균일한 방식으로 편집기 상태를 표시하고 상호 작용할 수 있는 서버측 및 클라이언트측 API를 제공합니다.

편집기 상태 표시줄은 AEM의 페이지, 경험 조각 및 템플릿 편집기에서 기본적으로 사용할 수 있습니다.

사용자 지정 리소스 상태 공급자에 대한 사용 사례는 다음과 같습니다.

* 페이지가 예약된 활성화 후 2시간 이내인 경우 작성자에게 알림
* 작성자에게 지난 15분 내에 페이지가 활성화되었음을 알림
* 작성자에게 지난 5분 이내에 페이지가 편집되었음을 알리며 누가 편집했는지 알림

![AEM 편집기 리소스 상태 개요](assets/sample-editor-resource-status-screenshot.png)

## 리소스 상태 공급자 프레임워크 {#resource-status-provider-framework}

사용자 지정 리소스 상태를 개발할 때 개발 작업은 다음과 같이 구성됩니다.

1. 상태를 필수 항목인지 확인하는 책임을 지는 ResourceStatusProvider 구현과 상태에 대한 기본 정보:제목, 메시지, 우선 순위, 변형, 아이콘 및 사용 가능한 작업
2. 사용 가능한 모든 작업의 기능을 구현하는 GraniteUI JavaScript를 사용할 수도 있습니다.

   ![리소스 상태 아키텍처](assets/sample-editor-resource-status-application-architecture.png)

3. 페이지, 경험 조각 및 템플릿 편집기의 일부로 제공되는 상태 리소스는 리소스 &quot;[!DNL statusType]&quot; 속성을 통해 유형을 제공합니다.

   * Page editor: `editor`
   * Experience Fragment editor: `editor`
   * 템플릿 편집기: `template-editor`

4. 상태 리소스 `statusType` 가 등록된 `CompositeStatusType` OSGi 구성 `name` 속성과 일치합니다.

   모든 일치 항목에 대해 유형은 수집되고, 을 통해 이 유형을 갖는 `CompositeStatusType's` 구현을 수집하는 데 사용됩니다 `ResourceStatusProvider` `ResourceStatusProvider.getType()`.

5. 일치 `ResourceStatusProvider` 는 편집기에 `resource` 전달되며 상태를 표시할 `resource` 것인지 확인합니다. 상태가 필요한 경우, 이 구현은 표시할 상태를 나타내는 0개 또는 여러 `ResourceStatuses` 개를 빌드해야 합니다.

   일반적으로 `ResourceStatusProvider` 값은 `ResourceStatus` 1당 0 또는 1을 `resource`반환합니다.

6. ResourceStatus는 고객이 구현하거나 상태를 구성하는 데 도움이 될 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 수 있는 인터페이스입니다. 상태는 다음과 같이 구성됩니다.

   * 제목
   * 메시지
   * 아이콘
   * 변형
   * 우선 순위
   * 작업
   * 데이터

7. 선택적으로, 개체에 대해 제공된 경우, 지원 clientlibs `Actions` `ResourceStatus` 는 기능을 작업 링크에 상태 표시줄로 바인딩해야 합니다.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 동작을 지원하는 모든 JavaScript 또는 CSS는 편집기에서 프런트 엔드 코드를 사용할 수 있도록 각 편집기의 각 클라이언트 라이브러리를 통해 프록시되어야 합니다.

   * 페이지 편집기 범주: `cq.authoring.editor.sites.page`
   * 경험 조각 편집기 범주: `cq.authoring.editor.sites.page`
   * 템플릿 편집기 범주: `cq.authoring.editor.sites.template`

## 코드 보기 {#view-the-code}

[GitHub에서 코드 보기](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 추가 리소스 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
