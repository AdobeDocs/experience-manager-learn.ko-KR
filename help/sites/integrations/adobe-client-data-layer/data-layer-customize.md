---
title: AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정
description: 사용자 지정 AEM 구성 요소의 콘텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법에 대해 알아봅니다. AEM 핵심 구성 요소에서 제공하는 API를 사용하여 데이터 레이어를 확장하고 맞춤화하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 0%

---

# AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정 {#customize-data-layer}

사용자 지정 AEM 구성 요소의 콘텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법에 대해 알아봅니다. [AEM 핵심 구성 요소에서 제공하는 API를 사용하여 ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)을(를) 확장하고 데이터 레이어를 사용자 지정하는 방법을 알아봅니다.

## 빌드할 항목

![줄 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

이 자습서에서는 WKND [Byline 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html)를 업데이트하여 Adobe 클라이언트 데이터 레이어를 확장하는 다양한 옵션에 대해 알아보겠습니다. _Byline_ 구성 요소는 **사용자 지정 구성 요소**&#x200B;이며 이 자습서에서 배운 내용을 다른 사용자 지정 구성 요소에 적용할 수 있습니다.

### 목표 {#objective}

1. 슬링 모델 및 구성 요소 HTL을 확장하여 구성 요소 데이터를 데이터 레이어에 삽입
1. 핵심 구성 요소의 데이터 레이어 유틸리티를 사용하여 작업 부담 감소
1. 핵심 구성 요소의 데이터 속성을 사용하여 기존 데이터 레이어 이벤트에 연결

## 사전 요구 사항 {#prerequisites}

이 자습서를 완료하려면 **로컬 개발 환경**&#x200B;이 필요합니다. 스크린샷 및 비디오는 macOS에서 실행되는 AEM as a Cloud Service SDK을 사용하여 캡처됩니다. 명령 및 코드는 별도로 명시하지 않는 한 로컬 운영 체제와 독립적입니다.

AEM as a Cloud Service을 처음 사용하십니까?**** AEM as a Cloud Service SDK을 사용하여 로컬 개발 환경을 설정하는 방법에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko).

**AEM 6.5를 처음 사용하십니까?** 로컬 개발 환경 설정에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko).

## WKND 참조 사이트 다운로드 및 배포 {#set-up-wknd-site}

이 튜토리얼에서는 WKND 참조 사이트의 Byline 구성 요소를 확장합니다. 로컬 환경에 WKND 코드 베이스를 복제하고 설치합니다.

1. [http://localhost:4502](http://localhost:4502)에서 실행 중인 AEM의 로컬 빠른 시작 **작성자** 인스턴스를 시작합니다.
1. 터미널 창을 열고 Git을 사용하여 WKND 코드 베이스를 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. WKND 코드 베이스를 AEM의 로컬 인스턴스에 배포합니다.

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 및 최신 서비스 팩의 경우 `classic` 프로필을 Maven 명령에 추가합니다.
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 새 브라우저 창을 열고 AEM에 로그인합니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)과(와) 같은 **잡지** 페이지를 엽니다.

   ![페이지의 Byline 구성 요소](assets/adobe-client-data-layer/byline-component-onpage.png)

   페이지에 경험 조각의 일부로 추가된 인라인 구성 요소의 예가 표시됩니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)에서 경험 조각을 볼 수 있습니다.
1. 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 계층의 현재 상태를 보려면 응답을 검사합니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 계층 응답](assets/data-layer-state-response.png)

   Byline 구성 요소가 데이터 레이어에 나열되어 있지 않은지 확인하십시오.

## Byline Sling 모델 업데이트 {#sling-model}

데이터 레이어에 구성 요소에 대한 데이터를 주입하려면, 먼저 구성 요소의 Sling 모델을 업데이트해 보겠습니다. 그런 다음 Byline의 Java™ 인터페이스 및 Sling 모델 구현을 업데이트하여 새 메서드 `getData()`을(를) 만듭니다. 이 메서드는 데이터 레이어에 삽입할 속성을 포함합니다.

1. 선택한 IDE에서 `aem-guides-wknd` 프로젝트를 엽니다. `core` 모듈로 이동합니다.
1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`에서 `Byline.java` 파일을 엽니다.

   ![Byline Java 인터페이스](assets/adobe-client-data-layer/byline-java-interface.png)

1. 아래 메서드를 인터페이스에 추가합니다.

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

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`에서 `BylineImpl.java` 파일을 엽니다. 이는 `Byline` 인터페이스의 구현이며 Sling 모델로 구현됩니다.

1. 파일의 시작 부분에 다음 가져오기 구문을 추가합니다.

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API는 JSON으로 노출될 데이터를 직렬화하는 데 사용됩니다. AEM 핵심 구성 요소 `ComponentUtils`은(는) 데이터 계층이 활성화되어 있는지 확인하는 데 사용됩니다.

1. 구현되지 않은 메서드 `getData()`을(를) `BylineImple.java`에 추가:

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

   위의 메서드에서는 JSON으로 노출될 속성을 캡처하는 데 새 `HashMap`을(를) 사용합니다. `getName()` 및 `getOccupations()`과(와) 같은 기존 메서드가 사용됩니다. `@type`은(는) 구성 요소의 고유한 리소스 유형을 나타내므로 클라이언트가 구성 요소의 유형에 따라 이벤트 및/또는 트리거를 쉽게 식별할 수 있습니다.

   `ObjectMapper`은(는) 속성을 직렬화하고 JSON 문자열을 반환하는 데 사용됩니다. 그런 다음 이 JSON 문자열을 데이터 레이어에 삽입할 수 있습니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `core` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 인라인 HTL 업데이트 {#htl}

`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en)을(를) 업데이트합니다. HTL(HTML 템플릿 언어)은 구성 요소의 HTML을 렌더링하는 데 사용되는 템플릿입니다.

각 AEM 구성 요소의 특수 데이터 특성 `data-cmp-data-layer`을(를) 사용하여 해당 데이터 계층을 표시합니다. AEM 핵심 구성 요소에서 제공하는 JavaScript이 이 데이터 속성을 찾습니다. 이 데이터 속성의 값은 Byline Sling 모델의 `getData()` 메서드에서 반환된 JSON 문자열로 채워지고 Adobe 클라이언트 데이터 레이어에 삽입됩니다.

1. `aem-guides-wknd` 프로젝트를 IDE로 엽니다. `ui.apps` 모듈로 이동합니다.
1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`에서 `byline.html` 파일을 엽니다.

   ![바이라인 HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. `data-cmp-data-layer` 특성을 포함하도록 `byline.html` 업데이트:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer`의 값이 `"${byline.data}"`(으)로 설정되었습니다. 여기서 `byline`은(는) 이전에 업데이트된 슬링 모델입니다. `.data`은(는) 이전 연습에서 구현된 `getData()`의 HTL에서 Java™ Getter 메서드를 호출하기 위한 표준 표기법입니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `ui.apps` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 Byline 구성 요소를 사용하여 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 개발자 도구를 열고 페이지의 HTML 소스에서 Byline 구성 요소를 검사합니다.

   ![줄 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

   `data-cmp-data-layer`이(가) Sling 모델의 JSON 문자열로 채워져 있습니다.

1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` 아래의 응답 아래로 이동하여 `byline` 구성 요소의 인스턴스가 데이터 레이어에 추가되었는지 확인합니다.

   ![데이터 계층의 일부](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   다음과 같은 항목이 표시됩니다.

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   노출된 속성이 Sling 모델의 `HashMap`에 추가된 속성과 동일한지 확인합니다.

## 클릭 이벤트 추가 {#click-event}

Adobe 클라이언트 데이터 레이어는 이벤트 기반이며 작업을 트리거하는 가장 일반적인 이벤트 중 하나는 `cmp:click` 이벤트입니다. AEM 핵심 구성 요소를 사용하면 데이터 요소 `data-cmp-clickable`을(를) 통해 구성 요소를 쉽게 등록할 수 있습니다.

클릭 가능한 요소는 일반적으로 CTA 단추 또는 탐색 링크입니다. 안타깝게도 Byline 구성 요소에는 이러한 항목이 없지만 다른 사용자 정의 구성 요소에서 일반적일 수 있으므로 어쨌든 등록하겠습니다.

1. IDE에서 `ui.apps` 모듈 열기
1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`에서 `byline.html` 파일을 엽니다.

1. Byline의 **name** 요소에 `data-cmp-clickable` 특성을 포함하도록 `byline.html`을(를) 업데이트합니다.

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

1. 브라우저로 돌아가서 Byline 구성 요소가 추가된 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   이벤트를 테스트하기 위해 개발자 콘솔을 사용하여 일부 JavaScript을 수동으로 추가합니다. 이 작업을 수행하는 방법에 대한 비디오는 [AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용](data-layer-overview.md)을 참조하십시오.

1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 메서드를 입력합니다.

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

1. **콘솔**&#x200B;에서 다음 메서드를 입력하십시오.

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   위의 메서드는 이벤트 수신기를 데이터 레이어로 푸시하여 `cmp:click` 이벤트를 수신하고 `bylineClickHandler`을(를) 호출합니다.

   >[!CAUTION]
   >
   > 이 연습에서 브라우저를 새로 고치는 것이 중요합니다. **not**. 그렇지 않으면 콘솔 JavaScript이 손실됩니다.

1. 브라우저에서 **Console**&#x200B;을(를) 열고 Byline 구성 요소에서 작성자 이름을 클릭합니다.

   ![줄 구성 요소 클릭됨](assets/adobe-client-data-layer/byline-component-clicked.png)

   콘솔 메시지 `Byline Clicked!`과(와) 줄 이름이 표시됩니다.

   `cmp:click` 이벤트는 가장 쉽게 연결할 수 있습니다. 보다 복잡한 구성 요소의 경우 및 다른 동작을 추적하기 위해 사용자 지정 JavaScript을 추가하여 새 이벤트를 추가하고 등록할 수 있습니다. 슬라이드를 전환할 때마다 `cmp:show` 이벤트를 트리거하는 슬라이드 구성 요소 가 좋은 예입니다. 자세한 내용은 [소스 코드를 참조하십시오](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## DataLayerBuilder 유틸리티 사용 {#data-layer-builder}

슬링 모델이 챕터 앞에서 [업데이트됨](#sling-model)일 때 `HashMap`을(를) 사용하고 각 속성을 수동으로 설정하여 JSON 문자열을 만들도록 선택했습니다. 이 방법은 작은 일회성 구성 요소에 잘 작동하지만, AEM 핵심 구성 요소를 확장하는 구성 요소에 대해서는 추가 코드가 많이 생길 수 있습니다.

대부분의 일괄 처리를 수행하기 위한 유틸리티 클래스 `DataLayerBuilder`이(가) 있습니다. 이렇게 하면 구현이 원하는 속성만 확장할 수 있습니다. `DataLayerBuilder`을(를) 사용하도록 Sling 모델을 업데이트하겠습니다.

1. IDE로 돌아가서 `core` 모듈로 이동합니다.
1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`에서 `Byline.java` 파일을 엽니다.
1. `ComponentData` 형식을 반환하도록 `getData()` 메서드를 수정하십시오.

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

   `ComponentData`은(는) AEM 핵심 구성 요소에서 제공하는 개체입니다. 이전 예제와 마찬가지로 JSON 문자열이 생성되지만 많은 추가 작업도 수행합니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`에서 `BylineImpl.java` 파일을 엽니다.

1. 다음 가져오기 구문을 추가합니다.

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. `getData()` 메서드를 다음과 같이 바꿉니다.

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

   인라인 구성 요소는 이미지 핵심 구성 요소의 일부를 재사용하여 작성자를 나타내는 이미지를 표시합니다. 위의 코드 조각에서는 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)을(를) 사용하여 `Image` 구성 요소의 데이터 레이어를 확장합니다. 그러면 사용된 이미지에 대한 모든 데이터로 JSON 개체가 미리 채워집니다. 또한 `@type` 및 구성 요소의 고유 식별자 설정과 같은 일부 루틴 기능도 수행합니다. 이 방법은 작습니다.

   유일한 속성은 `withTitle`을(를) 확장했으며 이 값은 `getName()` 값으로 대체되었습니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `core` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE로 돌아가서 `ui.apps`에서 `byline.html` 파일을 엽니다.
1. `byline.data.json`을(를) 사용하여 `data-cmp-data-layer` 특성을 채우도록 HTL을 업데이트하십시오.

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   이제 `ComponentData` 형식의 개체를 반환합니다. 이 개체에는 getter 메서드 `getJson()`이(가) 포함되어 있으며, 이 개체는 `data-cmp-data-layer` 특성을 채우는 데 사용됩니다.

1. 터미널 창을 엽니다. Maven 기술을 사용하여 `ui.apps` 모듈만 빌드하고 배포합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아가서 Byline 구성 요소가 추가된 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` 아래의 응답 아래로 이동하여 `byline` 구성 요소의 인스턴스를 찾습니다.

   ![줄 데이터 레이어 업데이트됨](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   이제 `byline` 구성 요소 항목 내에 `image` 개체가 있는지 확인하십시오. 여기에는 DAM의 에셋에 대한 자세한 정보가 포함되어 있습니다. 또한 `@type` 및 고유 ID(이 경우 `byline-136073cfcb`)가 자동으로 채워지고 구성 요소가 수정된 시기를 나타내는 `repo:modifyDate`이(가) 채워졌는지 확인하십시오.

## 추가 예 {#additional-examples}

1. WKND 코드 베이스에서 `ImageList` 구성 요소를 검사하여 데이터 계층을 확장하는 다른 예를 볼 수 있습니다.
   * `ImageList.java` - `core` 모듈의 Java 인터페이스입니다.
   * `ImageListImpl.java` - `core` 모듈의 슬링 모델입니다.
   * `image-list.html` - `ui.apps` 모듈의 HTL 템플릿입니다.

   >[!NOTE]
   >
   > [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)를 사용할 때 `occupation`과(와) 같은 사용자 지정 속성을 포함하기가 조금 더 어렵습니다. 하지만 이미지 또는 페이지를 포함하는 핵심 구성 요소를 확장하는 경우 시간이 많이 절약됩니다.

   >[!NOTE]
   >
   > 구현 전체에서 재사용되는 오브젝트에 대한 고급 데이터 레이어를 구축하는 경우 데이터 레이어 요소를 고유한 데이터 레이어별 Java™ 오브젝트로 추출하는 것이 좋습니다. 예를 들어 Commerce 핵심 구성 요소는 `ProductData` 및 `CategoryData`에 대한 인터페이스를 추가했습니다. 이는 Commerce 구현 내의 많은 구성 요소에서 사용할 수 있기 때문입니다. 자세한 내용은 [aem-cif-core-components 저장소](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)의 코드를 검토하십시오.

## 축하합니다! {#congratulations}

AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 를 확장하고 맞춤화하는 몇 가지 방법을 살펴보았습니다.

## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [핵심 구성 요소와 데이터 레이어 통합](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
