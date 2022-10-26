---
title: AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정
description: 사용자 지정 AEM 구성 요소의 컨텐츠로 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법을 알아봅니다. AEM 핵심 구성 요소에서 제공하는 API를 사용하여 데이터 레이어를 확장 및 사용자 지정하는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '2016'
ht-degree: 2%

---

# AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정 {#customize-data-layer}

사용자 지정 AEM 구성 요소의 컨텐츠로 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법을 알아봅니다. 에서 제공하는 API를 사용하는 방법을 알아봅니다. [확장할 AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) 데이터 레이어를 사용자 정의합니다.

## 빌드할 내용

![개별 데이터 계층](assets/adobe-client-data-layer/byline-data-layer-html.png)

이 자습서에서는 WKND를 업데이트하여 Adobe 클라이언트 데이터 레이어를 확장하는 다양한 옵션을 살펴봅니다 [필자 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). 이 구성 요소는 사용자 지정 구성 요소이며 이 자습서에서 배운 단원은 다른 사용자 지정 구성 요소에 적용할 수 있습니다.

### 목표 {#objective}

1. Sling 모델 및 구성 요소 HTL을 확장하여 데이터 레이어에 구성 요소 데이터를 삽입합니다
1. 핵심 구성 요소 데이터 계층 유틸리티를 사용하여 노력을 줄일 수 있습니다
1. 코어 구성 요소 데이터 속성을 사용하여 기존 데이터 레이어 이벤트에 연결합니다

## 사전 요구 사항 {#prerequisites}

A **로컬 개발 환경** 은 이 자습서를 완료해야 합니다. 스크린샷 및 비디오는 macOS에서 실행되는 AEM as a Cloud Service SDK를 사용하여 캡처됩니다. 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

**AEM as a Cloud Service을 처음 사용하십니까?** 다음을 확인하십시오 [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 데 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR).

**AEM 6.5를 처음 사용하십니까?** 다음을 확인하십시오 [로컬 개발 환경 설정에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko-KR).

## WKND 참조 사이트 다운로드 및 배포 {#set-up-wknd-site}

이 자습서는 WKND 참조 사이트에서 부산물 구성 요소를 확장합니다. 로컬 환경에 WKND 코드 베이스를 복제하고 설치합니다.

1. 로컬 빠른 시작 **작성자** 에서 실행되는 AEM 인스턴스 [http://localhost:4502](http://localhost:4502).
1. 터미널 창을 열고 Git를 사용하여 WKND 코드 베이스를 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. AEM의 로컬 인스턴스에 WKND 코드 베이스를 배포합니다.

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5와 최신 서비스 팩을 사용하는 경우 `classic` profile to Maven 명령:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 새 브라우저 창을 열고 AEM에 로그인합니다. 열기 **잡지** 다음과 같은 페이지: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![페이지의 개별 구성 요소](assets/adobe-client-data-layer/byline-component-onpage.png)

   경험 조각의 일부로 페이지에 추가된 라인 구성 요소의 예가 표시됩니다. 에서 경험 조각을 볼 수 있습니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. 개발자 도구를 열고 다음 명령을 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 레이어의 현재 상태를 보는 Inspect 응답입니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 레이어 응답](assets/data-layer-state-response.png)

   필자 구성 요소가 데이터 계층에 나열되지 않음을 확인합니다.

## 라인 Sling 모델 업데이트 {#sling-model}

데이터 계층에 구성 요소에 대한 데이터를 주입하려면 먼저 구성 요소의 Sling 모델을 업데이트해야 합니다. 다음으로, Byline의 Java 인터페이스와 Sling 모델 구현을 업데이트하여 새 메서드를 추가합니다 `getData()`. 이 메서드에는 데이터 계층에 삽입할 속성이 포함됩니다.

1. 선택한 IDE에서 `aem-guides-wknd` 프로젝트. 로 이동합니다 `core` 모듈.
1. 파일을 엽니다. `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![개별 Java 인터페이스](assets/adobe-client-data-layer/byline-java-interface.png)

1. 인터페이스에 새 메서드를 추가합니다.

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. 파일을 엽니다. `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   구현은 다음과 같습니다 `Byline` 인터페이스 및 를 Sling 모델로 구현됩니다.

1. 파일의 시작 부분에 다음 가져오기 구문을 추가합니다.

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   다음 `fasterxml.jackson` API는 노출할 데이터를 JSON으로 직렬화하는 데 사용됩니다. 다음 `ComponentUtils` AEM 코어 구성 요소 의 데이터 레이어가 활성화되어 있는지 확인하는 데 사용됩니다.

1. 구현되지 않은 메서드 추가 `getData()` to `BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   위의 메서드에서 새 항목이 `HashMap` 는 JSON으로 노출할 속성을 캡처하는 데 사용됩니다. 다음과 같은 기존 메서드를 확인합니다. `getName()` 및 `getOccupations()` 이 사용됩니다. `@type` 구성 요소의 고유한 리소스 유형을 나타냅니다. 이렇게 하면 클라이언트가 구성 요소 유형에 따라 이벤트 및/또는 트리거를 쉽게 식별할 수 있습니다.

   다음 `ObjectMapper` 속성을 serialize하고 JSON 문자열을 반환하는 데 사용됩니다. 그런 다음 이 JSON 문자열을 데이터 레이어에 삽입할 수 있습니다.

1. 터미널 창을 엽니다. 빌드 및 배포 `core` maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Byline HTL 업데이트 {#htl}

다음으로, `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL(HTML 템플릿 언어)은 구성 요소의 HTML을 렌더링하는 데 사용되는 템플릿입니다.

특수 데이터 속성 `data-cmp-data-layer` 각 AEM 구성 요소에서 는 해당 데이터 계층을 표시하는 데 사용됩니다.  AEM 코어 구성 요소에서 제공하는 JavaScript는 이 데이터 속성을 찾습니다. 이 데이터 속성은 해당 값이 Sling 모델의 byline Sling에 의해 반환된 JSON 문자열로 채워집니다 `getData()` 메서드를 사용하여 값을 Adobe 클라이언트 데이터 레이어에 삽입합니다.

1. IDE에서 `aem-guides-wknd` 프로젝트. 로 이동합니다 `ui.apps` 모듈.
1. 파일을 엽니다. `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![필자 HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 업데이트 `byline.html` 를 `data-cmp-data-layer` attribute:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   다음 값 `data-cmp-data-layer` 이(가) `"${byline.data}"` 여기서 `byline` 는 Sling 모델이 이전에 업데이트된 것입니다. `.data` 는 HTL에서 Java Getter 메서드를 호출하기 위한 표준 표기법입니다 `getData()` 이전 연습에서 구현되었습니다.

1. 터미널 창을 엽니다. 빌드 및 배포 `ui.apps` maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 개별 구성 요소로 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 개발자 도구를 열고 페이지의 HTML 소스에서 Byline 구성 요소 검사:

   ![개별 데이터 계층](assets/adobe-client-data-layer/byline-data-layer-html.png)

   다음을 볼 수 있습니다. `data-cmp-data-layer` 는 Sling 모델의 JSON 문자열로 채워집니다.

1. 브라우저의 개발자 도구를 열고 다음 명령을 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 아래의 응답 아래로 탐색합니다. `component` 의 인스턴스를 찾으려면 `byline` 구성 요소가 데이터 계층에 추가되었습니다.

   ![데이터 계층의 개별 부분](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   다음과 같은 항목이 표시됩니다.

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   에 추가된 속성은 노출된 속성과 동일합니다 `HashMap` 를 입력합니다.

## 클릭 이벤트 추가 {#click-event}

Adobe 클라이언트 데이터 계층은 이벤트를 기반으로 하며 작업을 트리거하는 가장 일반적인 이벤트 중 하나는 입니다 `cmp:click` 이벤트. AEM 코어 구성 요소를 사용하면 데이터 요소의 도움을 받아 구성 요소를 쉽게 등록할 수 있습니다. `data-cmp-clickable`.

클릭 가능한 요소는 일반적으로 CTA 단추 또는 탐색 링크입니다. 안타깝게도 Byline 구성 요소에는 이러한 구성 요소가 없지만 다른 사용자 지정 구성 요소에 공통인 경우 등록하게 됩니다.

1. 를 엽니다. `ui.apps` IDE의 모듈
1. 파일을 엽니다. `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. 업데이트 `byline.html` 를 `data-cmp-clickable` 속성 **이름** 요소:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 새 터미널을 엽니다. 빌드 및 배포 `ui.apps` maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 라인 구성 요소가 추가된 상태로 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   이벤트를 테스트하기 위해 개발자 콘솔을 사용하여 일부 JavaScript를 수동으로 추가합니다. 자세한 내용은 [AEM 핵심 구성 요소에서 Adobe 클라이언트 데이터 레이어 사용](data-layer-overview.md) 을 참조하십시오.

1. 브라우저의 개발자 도구를 열고 **콘솔**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   이 간단한 방법은 Byline 구성 요소의 이름 클릭을 처리해야 합니다.

1. 에 다음 방법을 입력합니다. **콘솔**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   위의 메서드는 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:click` 이벤트 및 호출 `bylineClickHandler`.

   >[!CAUTION]
   >
   > 중요한 일입니다 **not** 이 연습 동안 브라우저를 새로 고치려면 콘솔 JavaScript가 손실됩니다.

1. 브라우저에서 **콘솔** 을 열고 필자 구성 요소에서 작성자 이름을 클릭합니다.

   ![클릭한 개별 구성 요소](assets/adobe-client-data-layer/byline-component-clicked.png)

   콘솔 메시지가 표시됩니다 `Byline Clicked!` 및 Byline 이름

   다음 `cmp:click` 이벤트는 에 연결하는 가장 쉬운 방법입니다. 더 복잡한 구성 요소와 다른 동작을 추적하려면 사용자 지정 Javascript를 추가하여 새 이벤트를 추가하고 등록할 수 있습니다. 가장 좋은 예는 를 트리거하는 회전 메뉴 구성 요소입니다 `cmp:show` 슬라이드가 전환될 때마다 발생합니다. 자세한 내용은 [자세한 내용은 소스 코드 를 참조하십시오.](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## DataLayerBuilder 유틸리티 사용 {#data-layer-builder}

Sling 모델이 [업데이트됨](#sling-model) 이 장의 앞부분에서 `HashMap` 및 각 속성을 수동으로 설정합니다. 이 방법은 작은 일회성 구성 요소에 대해서는 잘 작동하지만 AEM 코어 구성 요소를 확장하는 구성 요소에 대해서는 많은 추가 코드가 발생할 수 있습니다.

유틸리티 클래스 `DataLayerBuilder`는 대부분의 무거운 리프팅을 수행하기 위해 존재합니다. 이를 통해 구현은 원하는 속성만 확장할 수 있습니다. Sling 모델을 업데이트하여 `DataLayerBuilder`.

1. IDE로 돌아가서 `core` 모듈.
1. 파일을 엽니다. `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. 수정 `getData()` 메서드 `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` 는 AEM 코어 구성 요소에서 제공하는 개체입니다. 이 경우 이전 예와 마찬가지로 JSON 문자열로 출력되지만 추가적인 작업도 많이 수행합니다.

1. 파일을 엽니다. `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. 다음 가져오기 구문을 추가합니다.

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 바꾸기 `getData()` 메서드를 사용합니다.

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   필자 구성 요소는 이미지 코어 구성 요소 의 일부를 다시 사용하여 작성자를 나타내는 이미지를 표시합니다. 위의 코드 조각에서 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) 는 페이지의 `Image` 구성 요소. 이렇게 하면 JSON 개체가 사용된 이미지에 대한 모든 데이터로 미리 채워집니다. 그것은 또한 를 설정하는 것과 같은 몇 가지 일상적인 기능을 합니다 `@type` 및 구성 요소의 고유 식별자입니다. 방법이 정말 작아!

   유일한 속성은 `withTitle` 이 값은 `getName()`.

1. 터미널 창을 엽니다. 빌드 및 배포 `core` maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE로 돌아가서 를 엽니다. `byline.html` 파일 위치 `ui.apps`
1. 사용할 HTL 업데이트 `byline.data.json` 를 채우기 위해 `data-cmp-data-layer` attribute:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   이제 유형의 객체를 반환한다는 것을 기억하십시오 `ComponentData`. 이 개체에는 getter 메서드가 포함되어 있습니다 `getJson()` 그리고 이것은 `data-cmp-data-layer` 속성을 사용합니다.

1. 터미널 창을 엽니다. 빌드 및 배포 `ui.apps` maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 라인 구성 요소가 추가된 상태로 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. 브라우저의 개발자 도구를 열고 다음 명령을 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 아래의 응답 아래로 탐색합니다. `component` 의 인스턴스를 찾으려면 `byline` 구성 요소:

   ![개별 데이터 계층 업데이트됨](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   다음과 같은 항목이 표시됩니다.

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   이제 `image` 내의 개체 `byline` 구성 요소 항목입니다. 여기에는 DAM의 자산에 대한 자세한 정보가 있습니다. 또한 `@type` 및 고유 id(이 경우 `byline-136073cfcb`)을 자동으로 채울 수 있을 뿐만 아니라 `repo:modifyDate` 구성 요소를 수정한 시기를 나타냅니다.

## 추가 예 {#additional-examples}

1. 데이터 레이어를 확장하는 다른 예는 를 검사하여 확인할 수 있습니다 `ImageList` WKND 코드 베이스의 구성 요소:
   * `ImageList.java` - 의 Java 인터페이스 `core` 모듈.
   * `ImageListImpl.java` - 의 Sling 모델 `core` 모듈.
   * `image-list.html` - 의 HTL 템플릿 `ui.apps` 모듈.

   >[!NOTE]
   >
   > 과 같은 사용자 지정 속성을 포함시키는 것은 더 어렵습니다 `occupation` 사용 시 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). 하지만 이미지나 페이지를 포함하는 코어 구성 요소를 확장하는 경우 이 유틸리티를 사용하면 많은 시간이 절약됩니다.

   >[!NOTE]
   >
   > 구현 전체에서 객체에 대한 고급 데이터 레이어를 다시 사용하는 경우 데이터 레이어 요소를 해당 Java 객체에 대한 자체 데이터 레이어로 추출하는 것이 좋습니다. 예를 들어 상거래 코어 구성 요소에 `ProductData` 및 `CategoryData` 이러한 매개 변수는 상거래 구현 내의 많은 구성 요소에서 사용할 수 있으므로 검토 [aem-cif-core-components 리포지토리의 코드](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) 자세한 내용

## 축하합니다! {#congratulations}

AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어를 확장 및 사용자 지정하는 몇 가지 방법을 탐색했습니다!

## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [핵심 구성 요소와 데이터 레이어 통합](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
