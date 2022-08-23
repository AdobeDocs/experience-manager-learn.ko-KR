---
title: AEM Sites에서 리소스 상태 개발
description: 'Adobe Experience Manager의 리소스 상태 API는 AEM 다양한 편집기 웹 UI에서 상태 메시지를 노출하기 위한 플러그형 프레임워크입니다. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
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

1. 상태가 필수인지 확인하는 책임이 있는 ResourceStatusProvider 구현과 상태에 대한 기본 정보: 제목, 메시지, 우선 순위, 변형, 아이콘 및 사용 가능한 작업.
2. 선택적으로, 사용 가능한 모든 작업의 기능을 구현하는 GraniteUI JavaScript입니다.

   ![리소스 상태 아키텍처](assets/sample-editor-resource-status-application-architecture.png)

3. 페이지, 경험 조각 및 템플릿 편집기의 일부로 제공된 상태 리소스에는 &quot; 리소스를 통해 유형이 제공됩니다.[!DNL statusType]&quot; 속성을 사용합니다.

   * 페이지 편집기: `editor`
   * 경험 조각 편집기: `editor`
   * 템플릿 편집기: `template-editor`

4. 상태 리소스의 `statusType` 등록됨 `CompositeStatusType` OSGi 구성 `name` 속성을 사용합니다.

   모든 일치 항목에 대해 `CompositeStatusType's` 유형은 수집되며 `ResourceStatusProvider` 이러한 유형을 갖는 구현 `ResourceStatusProvider.getType()`.

5. 일치 `ResourceStatusProvider` 이 전달됨 `resource` 편집기에서 그리고 가 `resource` 에 표시할 상태가 있습니다. 상태가 필요한 경우 이 구현은 0개 이상의 빌드를 담당합니다 `ResourceStatuses` 반환하려면 각각 표시할 상태를 나타냅니다.

   일반적으로 `ResourceStatusProvider` 0 또는 1 반환 `ResourceStatus` per `resource`.

6. ResourceStatus 는 고객이 구현하거나 유용한 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 상태를 구성하는 데 사용할 수 있습니다. 상태는 다음과 같이 구성됩니다.

   * 제목
   * 메시지
   * 아이콘
   * 변형
   * 우선 순위
   * 액션
   * 데이터

7. 원할 경우 `Actions` 다음 기간 동안 `ResourceStatus` 개체를 지원하는 clientlibs는 상태 표시줄에 작업 링크에 기능을 바인딩해야 합니다.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 작업을 지원하기 위해 지원되는 모든 JavaScript 또는 CSS를 각 편집기의 각 클라이언트 라이브러리를 통해 프록시해야 편집기에서 프런트 엔드 코드를 사용할 수 있습니다.

   * 페이지 편집기 카테고리: `cq.authoring.editor.sites.page`
   * 경험 조각 편집기 카테고리: `cq.authoring.editor.sites.page`
   * 템플릿 편집기 범주: `cq.authoring.editor.sites.template`

## 코드 보기 {#view-the-code}

[GitHub에서 코드를 참조하십시오](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 추가 리소스 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
