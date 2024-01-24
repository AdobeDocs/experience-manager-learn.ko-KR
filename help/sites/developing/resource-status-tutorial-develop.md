---
title: AEM Sites에서 리소스 상태 개발
description: Adobe Experience Manager의 리소스 상태 API는 AEM 다양한 편집기 웹 UI에서 상태 메시지를 노출하기 위한 플러그형 프레임워크입니다.
doc-type: Tutorial
version: 6.4, 6.5
duration: 108
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 2%

---


# 리소스 상태 개발 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager의 리소스 상태 API는 AEM 다양한 편집기 웹 UI에서 상태 메시지를 노출하기 위한 플러그형 프레임워크입니다.

## 개요 {#overview}

편집기 리소스 상태 프레임워크는 표준적이고 균일한 방식으로 편집기 상태를 표시하고 상호 작용하기 위한 서버측 및 클라이언트측 API를 제공합니다.

편집기 상태 표시줄은 AEM의 페이지, 경험 조각 및 템플릿 편집기에서 기본적으로 사용할 수 있습니다.

사용자 지정 리소스 상태 공급자에 대한 사용 사례는 다음과 같습니다.

* 페이지가 예약된 활성화 후 2시간 이내에 작성자에게 알림
* 지난 15분 내에 페이지가 활성화되었음을 작성자에게 알림
* 지난 5분 내에 페이지가 편집되었다고 작성자에게 알리는 중

![AEM 편집기 리소스 상태 개요](assets/sample-editor-resource-status-screenshot.png)

## 리소스 상태 공급자 프레임워크 {#resource-status-provider-framework}

사용자 정의 리소스 상태를 개발할 때 개발 작업은 다음과 같이 구성됩니다.

1. 상태가 필요한지 여부를 결정하는 ResourceStatusProvider 구현과 상태에 대한 기본 정보(제목, 메시지, 우선 순위, 변형, 아이콘 및 사용 가능한 작업)입니다.
2. 선택 사항으로 사용 가능한 작업의 기능을 구현하는 GraniteUI JavaScript입니다.

   ![리소스 상태 아키텍처](assets/sample-editor-resource-status-application-architecture.png)

3. 페이지, 경험 조각 및 템플릿 편집기의 일부로 제공되는 상태 리소스에는 &quot; 리소스를 통해 유형이 제공됩니다.[!DNL statusType]&quot;속성.

   * 페이지 편집기: `editor`
   * 경험 조각 편집기: `editor`
   * 템플릿 편집기: `template-editor`

4. 상태 리소스 `statusType` 등록된 항목과 일치함 `CompositeStatusType` OSGi 구성됨 `name` 속성.

   모든 일치 항목의 경우 `CompositeStatusType's` 유형은 수집되며 를 수집하는 데 사용됩니다. `ResourceStatusProvider` 을 통해 이 유형의 구현 `ResourceStatusProvider.getType()`.

5. 일치 `ResourceStatusProvider` 이(가) 을(를) 통과함 `resource` 를 편집기에서 `resource` 표시할 상태가 있습니다. 상태가 필요한 경우 이 구현은 0개 이상을 빌드합니다. `ResourceStatuses` 반환할 경우, 각각이 표시할 상태를 나타냅니다.

   일반적으로 `ResourceStatusProvider` 0 또는 1 반환 `ResourceStatus` 마다 `resource`.

6. ResourceStatus는 고객 또는 유용한 사용자가 구현할 수 있는 인터페이스입니다. `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 를 사용하여 상태를 구성할 수 있습니다. 상태는 다음으로 구성됩니다.

   * 제목
   * 메시지
   * 아이콘
   * 변형
   * 우선 순위
   * 액션
   * 데이터

7. 필요한 경우 `Actions` 은(는) 다음에 대해 제공됩니다. `ResourceStatus` 개체, 기능을 지원하는 clientlib은 상태 표시줄의 작업 링크에 바인딩해야 합니다.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 작업을 지원하는 모든 JavaScript 또는 CSS는 편집기에서 프론트엔드 코드를 사용할 수 있도록 각 편집기의 각 클라이언트 라이브러리를 통해 프록시되어야 합니다.

   * 페이지 편집기 카테고리: `cq.authoring.editor.sites.page`
   * 경험 조각 편집기 카테고리: `cq.authoring.editor.sites.page`
   * 템플릿 편집기 범주: `cq.authoring.editor.sites.template`

## 코드 보기 {#view-the-code}

[GitHub의 코드 보기](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 추가 리소스 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDoc](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDoc](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
