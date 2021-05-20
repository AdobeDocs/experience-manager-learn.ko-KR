---
title: AEM Sites에서 리소스 상태 개발
description: 'Adobe Experience Manager의 리소스 상태 API는 AEM 다양한 편집기 웹 UI에서 상태 메시지를 노출하기 위한 플러그형 프레임워크입니다. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 리소스 상태 개발 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager의 리소스 상태 API는 AEM 다양한 편집기 웹 UI에서 상태 메시지를 노출하기 위한 플러그형 프레임워크입니다.

## 개요 {#overview}

편집자 프레임워크의 리소스 상태는 표준적이고 균일한 방식으로 편집기 상태를 표시하고 상호 작용하는 서버측 및 클라이언트측 API를 제공합니다.

편집기 상태 표시줄은 기본적으로 AEM의 페이지, 경험 조각 및 템플릿 편집기에서 사용할 수 있습니다.

사용자 지정 리소스 상태 공급자에 대한 사용 사례는 다음과 같습니다.

* 페이지가 예약된 활성화 후 2시간 내에 있을 때 작성자에게 알림
* 지난 15분 내에 페이지가 활성화되었음을 작성자에게 알림
* 페이지가 지난 5분 내에 편집되었음을 작성자 사용자에게 알립니다.

![AEM 편집기 리소스 상태 개요](assets/sample-editor-resource-status-screenshot.png)

## 리소스 상태 공급자 프레임워크 {#resource-status-provider-framework}

사용자 지정 리소스 상태를 개발할 때 개발 작업은 다음과 같이 구성됩니다.

1. 상태가 필수인지 확인하는 책임이 있는 ResourceStatusProvider 구현과 상태에 대한 기본 정보:제목, 메시지, 우선 순위, 변형, 아이콘 및 사용 가능한 작업.
2. 선택적으로, 사용 가능한 모든 작업의 기능을 구현하는 GraniteUI JavaScript입니다.

   ![리소스 상태 아키텍처](assets/sample-editor-resource-status-application-architecture.png)

3. 페이지, 경험 조각 및 템플릿 편집기의 일부로 제공된 상태 리소스에는 &quot;[!DNL statusType]&quot; 리소스를 통해 유형이 제공됩니다.

   * 페이지 편집기:`editor`
   * 경험 조각 편집기:`editor`
   * 템플릿 편집기: `template-editor`

4. 상태 리소스의 `statusType`이(가) 등록된 `CompositeStatusType` OSGi에 대해 구성된 `name` 속성과 일치합니다.

   모든 일치 항목에 대해 `CompositeStatusType's` 유형이 수집되고 `ResourceStatusProvider.getType()` 를 통해 이 유형이 있는 `ResourceStatusProvider` 구현을 수집하는 데 사용됩니다.

5. 일치하는 `ResourceStatusProvider`이(가) 편집기의 `resource`에 전달되고 `resource`의 상태가 표시되어야 하는지 확인합니다. 상태가 필요한 경우 이 구현은 반환할 0개 또는 많은 `ResourceStatuses`을 작성하고 각각 표시할 상태를 나타냅니다.

   일반적으로 `ResourceStatusProvider`은 `resource`당 0 또는 1 `ResourceStatus`을 반환합니다.

6. ResourceStatus 는 고객이 구현할 수 있는 인터페이스이거나 유용한 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 을 사용하여 상태를 구성할 수 있습니다. 상태는 다음과 같이 구성됩니다.

   * 제목
   * 메시지
   * 아이콘
   * 변형
   * 우선 순위
   * 작업
   * 데이터

7. 원할 경우 `ResourceStatus` 개체에 `Actions`이 제공된 경우, 지원 clientlibs가 작업 링크에 상태 표시줄에 기능을 바인딩해야 합니다.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 작업을 지원하기 위해 지원되는 모든 JavaScript 또는 CSS를 각 편집기의 각 클라이언트 라이브러리를 통해 프록시해야 편집기에서 프런트 엔드 코드를 사용할 수 있습니다.

   * 페이지 편집기 카테고리:`cq.authoring.editor.sites.page`
   * 경험 조각 편집기 카테고리:`cq.authoring.editor.sites.page`
   * 템플릿 편집기 범주:`cq.authoring.editor.sites.template`

## 코드 {#view-the-code} 보기

[GitHub에서 코드를 참조하십시오](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 추가 리소스 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
