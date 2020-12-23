---
title: AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 정의
description: 사용자 정의 AEM 구성 요소의 컨텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 정의하는 방법을 알아봅니다. AEM 코어 구성 요소에서 제공하는 API를 사용하여 데이터 레이어를 확장하고 사용자 지정하는 방법을 알아봅니다.
feature: core-component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
translation-type: tm+mt
source-git-commit: 46936876de355de9923f7a755aa6915a13cca354
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 1%

---


# AEM 구성 요소 {#customize-data-layer}를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정

사용자 정의 AEM 구성 요소의 컨텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 정의하는 방법을 알아봅니다. [AEM 핵심 구성 요소에서 제공하는 API를 사용하여](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)데이터 레이어를 확장하고 사용자 지정하는 방법을 알아봅니다.

## 구축 분야

![필자 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

이 자습서에서는 WKND [Byline 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)를 업데이트하여 Adobe 클라이언트 데이터 레이어를 확장하기 위한 다양한 옵션을 살펴봅니다. 이 구성 요소는 사용자 정의 구성 요소이며 이 튜토리얼에서 학습한 교훈을 다른 사용자 정의 구성 요소에 적용할 수 있습니다.

### 목표 {#objective}

1. Sling 모델 및 구성 요소 HTL을 확장하여 구성 요소 데이터를 데이터 레이어에 주입합니다.
1. 핵심 구성 요소 데이터 레이어 유틸리티를 사용하여 노력 감소
1. 핵심 구성 요소 데이터 속성을 사용하여 기존 데이터 레이어 이벤트에 연결

## 전제 조건 {#prerequisites}

이 자습서를 완료하려면 **로컬 개발 환경**&#x200B;이 필요합니다. macOS에서 실행 중인 Cloud Service SDK로 AEM을 사용하여 스크린샷 및 비디오를 캡처합니다. 별도의 언급이 없는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

**Cloud Service으로 AEM을 처음 사용하시나요?** AEM을 Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하는 방법에 대한  [다음 가이드를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**AEM 6.5를 처음 사용하시나요?** 로컬 개발 환경을 설정하는 방법은  [다음 안내서를 참조하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## WKND 참조 사이트 {#set-up-wknd-site} 다운로드 및 배포

이 자습서는 WKND 참조 사이트에서 Byline 구성 요소를 확장합니다. 로컬 환경에 WKND 코드 베이스를 복제하여 설치합니다.

1. [http://localhost:4502](http://localhost:4502)에서 실행 중인 AEM의 로컬 빠른 시작 **작성자** 인스턴스를 시작합니다.
1. 터미널 창을 열고 Git을 사용하여 WKND 코드 베이스를 복제합니다.

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
   > AEM 6.5와 최신 서비스 팩을 사용하는 경우 `classic` 프로파일을 Maven 명령에 추가합니다.
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 새 브라우저 창을 열고 AEM에 로그인합니다. 다음과 같이 **잡지** 페이지를 엽니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![페이지의 byline 구성 요소](assets/adobe-client-data-layer/byline-component-onpage.png)

   경험 조각의 일부로 페이지에 추가된 필자 구성 요소의 예를 볼 수 있습니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)에서 경험 조각을 볼 수 있습니다.
1. 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect은 AEM 사이트에서 데이터 레이어의 현재 상태를 확인하는 응답입니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 레이어 응답](assets/data-layer-state-response.png)

   Byline 구성 요소가 데이터 레이어에 나열되지 않도록 주의하십시오.

## 필라인 슬링 모델 {#sling-model} 업데이트

데이터 레이어의 구성 요소에 대한 데이터를 주입하려면 먼저 구성 요소의 Sling 모델을 업데이트해야 합니다. 다음으로, Byline의 Java 인터페이스 및 Sling 모델 구현을 업데이트하여 새 메서드 `getData()`을(를) 추가합니다. 이 메서드에는 데이터 레이어에 삽입할 속성이 포함됩니다.

1. 선택한 IDE에서 `aem-guides-wknd` 프로젝트를 엽니다. `core` 모듈로 이동합니다.
1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`에서 `Byline.java` 파일을 엽니다.

   ![필자 Java 인터페이스](assets/adobe-client-data-layer/byline-java-interface.png)

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

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`에서 `BylineImpl.java` 파일을 엽니다.

   이것은 `Byline` 인터페이스의 구현이며 Sling 모델로 구현됩니다.

1. 파일 시작 부분에 다음 가져오기 문을 추가합니다.

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API는 JSON으로 노출할 데이터를 일련화하는 데 사용됩니다. AEM 코어 구성 요소의 `ComponentUtils`을 사용하여 데이터 레이어가 활성화되었는지 확인합니다.

1. 구현되지 않은 메서드 `getData()`을 `BylineImple.java`에 추가합니다.

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

   위의 메서드에서 새 `HashMap`은(는) JSON으로 노출할 속성을 캡처하는 데 사용됩니다. `getName()` 및 `getOccupations()` 같은 기존 메서드가 사용된다는 점을 참고하십시오. `@type` 구성 요소의 고유한 리소스 유형을 나타내며, 이를 통해 클라이언트는 구성 요소 유형에 따라 이벤트 및/또는 트리거를 쉽게 식별할 수 있습니다.

   `ObjectMapper`은 속성을 일련화하고 JSON 문자열을 반환하는 데 사용됩니다. 그런 다음 이 JSON 문자열을 데이터 레이어에 삽입할 수 있습니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `core` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Byline HTL {#htl} 업데이트

그런 다음 `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)을 업데이트합니다. HTL(HTML 템플릿 언어)은 구성 요소의 HTML을 렌더링하는 데 사용되는 템플릿입니다.

각 AEM 구성 요소의 특수 데이터 속성 `data-cmp-data-layer`은 해당 데이터 레이어를 표시하는 데 사용됩니다.  AEM 코어 구성 요소에서 제공하는 JavaScript는 이 데이터 속성을 찾고 있으며, 이 값은 Byline Sling 모델의 `getData()` 메서드에서 반환되는 JSON 문자열로 채워지고 Adobe 클라이언트 데이터 레이어에 값을 삽입합니다.

1. IDE에서 `aem-guides-wknd` 프로젝트를 엽니다. `ui.apps` 모듈로 이동합니다.
1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`에서 `byline.html` 파일을 엽니다.

   ![Byline HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. `data-cmp-data-layer` 특성을 포함하도록 `byline.html`을(를) 업데이트합니다.

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer`의 값이 `"${byline.data}"`으로 설정되었습니다. 여기서 `byline`는 이전에 업데이트된 Sling 모델입니다. `.data` 는 이전 연습에서  `getData()` 구현된 HTL의 Java Getter 메서드를 호출하기 위한 표준 표기법입니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `ui.apps` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 Byline 구성 요소가 있는 페이지를 다시 엽니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 개발자 도구를 열고 페이지의 HTML 소스에서 Byline 구성 요소를 검사합니다.

   ![필자 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

   `data-cmp-data-layer`이(가) Sling 모델의 JSON 문자열로 채워졌음을 확인해야 합니다.

1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` 응답 아래로 탐색하여 `byline` 구성 요소의 인스턴스가 데이터 레이어에 추가되었음을 찾습니다.

   ![데이터 레이어의 개별 부분](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   다음과 같은 항목이 표시됩니다.

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   노출되는 속성이 스링 모델의 `HashMap`에 추가된 속성과 동일함을 확인합니다.

## 클릭 이벤트 {#click-event} 추가

Adobe 클라이언트 데이터 레이어는 이벤트 기반이며 작업을 트리거하는 가장 일반적인 이벤트 중 하나는 `cmp:click` 이벤트입니다. AEM 코어 구성 요소를 사용하면 데이터 요소의 도움을 받아 구성 요소를 쉽게 등록할 수 있습니다.`data-cmp-clickable`.

클릭 가능한 요소는 일반적으로 CTA 단추 또는 탐색 링크입니다. 안타깝게도 Byline 구성 요소에는 이러한 구성 요소가 없지만 다른 사용자 지정 구성 요소에 공통인 것일 수 있으므로 나중에 등록할 것입니다.

1. IDE에서 `ui.apps` 모듈을 엽니다.
1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`에서 `byline.html` 파일을 엽니다.

1. `byline.html`을 업데이트하여 Byline의 **name** 요소에 `data-cmp-clickable` 특성을 포함하십시오.

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 새 터미널을 엽니다. Maven 기술을 사용하여 `ui.apps` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 Byline 구성 요소가 추가된 상태로 페이지를 다시 엽니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   이벤트를 테스트하기 위해 개발자 콘솔을 사용하여 일부 JavaScript를 수동으로 추가합니다. 이 방법에 대한 비디오는 [AEM 핵심 구성 요소에 Adobe 클라이언트 데이터 레이어 사용](data-layer-overview.md)을 참조하십시오.

1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 방법을 입력합니다.

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   이 간단한 방법을 사용하면 필라인 구성 요소 이름의 클릭을 처리할 수 있습니다.

1. **콘솔**&#x200B;에 다음 메서드를 입력합니다.

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   위의 메서드는 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:click` 이벤트를 수신하고 `bylineClickHandler`을 호출합니다.

   >[!CAUTION]
   >
   > 이 연습 동안 브라우저를 새로 고치는 것은 **이 아닌**&#x200B;이어야 합니다. 그렇지 않으면 콘솔 JavaScript가 손실됩니다.

1. 브라우저의 **콘솔**&#x200B;이(가) 열린 상태에서 필자 구성 요소에서 작성자의 이름을 클릭합니다.

   ![클릭한 필자 구성 요소](assets/adobe-client-data-layer/byline-component-clicked.png)

   콘솔 메시지 `Byline Clicked!`와 Byline 이름이 표시됩니다.

   `cmp:click` 이벤트는 연결할 수 있는 가장 쉬운 이벤트입니다. 보다 복잡한 구성 요소 및 기타 동작을 추적하려면 사용자 지정 javascript를 추가하여 새 이벤트를 추가하고 등록할 수 있습니다. 슬라이드가 전환될 때마다 `cmp:show` 이벤트를 트리거하는 회전판 구성 요소가 좋은 예입니다. 자세한 내용은 [소스 코드를 참조하십시오](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## DataLayerBuilder 유틸리티 {#data-layer-builder} 사용

Sling 모델이 이 장의 이전 버전에서 [업데이트](#sling-model)인 경우 `HashMap`를 사용하여 JSON 문자열을 만들고 각 속성을 수동으로 설정하기로 했습니다. 이 방법은 작은 일회성 구성 요소에 대해서는 잘 작동하지만 AEM 코어 구성 요소를 확장하는 구성 요소에 대해서는 많은 추가 코드가 발생할 수 있습니다.

대부분의 무거운 리프트를 수행하는 유틸리티 클래스 `DataLayerBuilder`가 있습니다. 이를 통해 구현은 원하는 속성만 확장할 수 있습니다. Sling 모델을 업데이트하여 `DataLayerBuilder`

1. IDE로 돌아가서 `core` 모듈로 이동합니다.
1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`에서 `Byline.java` 파일을 엽니다.
1. `getData()` 메서드를 수정하여 `ComponentData` 유형을 반환합니다.

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

   `ComponentData` 는 AEM 핵심 구성 요소에서 제공하는 객체입니다. 이전 예에서처럼 JSON 문자열로 이어지는 결과가 나오지만 많은 추가 작업을 수행합니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`에서 `BylineImpl.java` 파일을 엽니다.

1. 다음 가져오기 문을 추가합니다.

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. `getData()` 메서드를 다음으로 바꿉니다.

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

   필자 구성 요소는 이미지 코어 구성 요소의 부분을 다시 사용하여 작성자를 나타내는 이미지를 표시합니다. 위의 코드 조각에서 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)은 `Image` 구성 요소의 데이터 레이어를 확장하는 데 사용됩니다. 이렇게 하면 JSON 개체가 사용된 이미지에 대한 모든 데이터로 사전에 채워집니다. 또한 구성 요소의 `@type` 및 고유 식별자 설정과 같은 일부 루틴 기능도 수행합니다. 방법이 정말 작다는 것을 알아두시오!

   유일한 속성은 `getName()` 값으로 대체되는 `withTitle`을 확장했습니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `core` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE로 돌아가 `ui.apps` 아래의 `byline.html` 파일을 엽니다.
1. `byline.data.json`을(를) 사용하여 `data-cmp-data-layer` 속성을 채우도록 HTL을 업데이트합니다.

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   이제 `ComponentData` 유형의 객체를 반환한다는 사실을 기억하십시오. 이 객체에는 getter 메서드 `getJson()`이 포함되며 이 메서드는 `data-cmp-data-layer` 속성을 채우는 데 사용됩니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `ui.apps` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 Byline 구성 요소가 추가된 상태로 페이지를 다시 엽니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` 아래의 응답 아래로 탐색하여 `byline` 구성 요소의 인스턴스를 찾습니다.

   ![라인 데이터 레이어 업데이트됨](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   이제 `byline` 구성 요소 항목 내에 `image` 개체가 있습니다. DAM의 자산에 대한 자세한 정보가 더 있습니다. 또한 `@type` 및 고유 ID(이 경우 `byline-136073cfcb`)가 자동으로 채워지고 구성 요소를 수정할 때 나타내는 `repo:modifyDate`도 확인합니다.

## 추가 예 {#additional-examples}

1. WKND 코드 베이스에서 `ImageList` 구성 요소를 검사하면 데이터 레이어를 확장하는 다른 예를 볼 수 있습니다.
   * `ImageList.java` - 모듈의 Java  `core` 인터페이스.
   * `ImageListImpl.java` - 모듈의 Sling  `core` Model을 참조하십시오.
   * `image-list.html` - 모듈의 HTL  `ui.apps` 템플릿입니다.

   >[!NOTE]
   >
   > [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)를 사용할 때는 `occupation`과 같은 사용자 지정 속성을 포함하기가 조금 더 어렵습니다. 하지만 이미지나 페이지를 포함하는 핵심 구성 요소를 확장하면 이 유틸리티로 많은 시간이 절약됩니다.

   >[!NOTE]
   >
   > 구현 전체에서 재사용되는 개체에 대한 고급 데이터 레이어를 작성하는 경우 데이터 레이어 요소를 자체 데이터 레이어별 Java 객체로 추출하는 것이 좋습니다. 예를 들어 상거래 핵심 구성 요소는 상거래 구현 내의 여러 구성 요소에서 사용할 수 있으므로 `ProductData` 및 `CategoryData`에 대한 인터페이스를 추가했습니다. 자세한 내용은 [aem-cif-core-components 보고서](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)의 코드를 검토하십시오.

## 축하합니다!{#congratulations}

AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어를 확장하고 사용자 정의하는 몇 가지 방법을 살펴보았습니다.

## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [핵심 구성 요소와 데이터 레이어 통합](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)
