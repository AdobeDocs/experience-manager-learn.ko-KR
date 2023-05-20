---
title: AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정
description: 사용자 지정 AEM 구성 요소의 콘텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법에 대해 알아봅니다. AEM 핵심 구성 요소에서 제공하는 API를 사용하여 데이터 계층을 확장하고 사용자 지정하는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '2008'
ht-degree: 1%

---

# AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정 {#customize-data-layer}

사용자 지정 AEM 구성 요소의 콘텐츠를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법에 대해 알아봅니다. 에서 제공하는 API를 사용하는 방법을 알아봅니다. [확장할 AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) 데이터 레이어를 사용자 지정합니다.

## 빌드할 항목

![인라인 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

이 자습서에서는 WKND를 업데이트하여 Adobe 클라이언트 데이터 레이어를 확장하는 다양한 옵션에 대해 살펴보겠습니다 [Byline 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html). 다음 _필자_ 구성 요소가 **사용자 지정 구성 요소** 및 이 자습서에서 배운 내용을 다른 사용자 지정 구성 요소에 적용할 수 있습니다.

### 목표 {#objective}

1. 슬링 모델 및 구성 요소 HTL을 확장하여 구성 요소 데이터를 데이터 레이어에 삽입
1. 핵심 구성 요소의 데이터 레이어 유틸리티를 사용하여 작업 부담 감소
1. 핵심 구성 요소의 데이터 속성을 사용하여 기존 데이터 레이어 이벤트에 연결

## 사전 요구 사항 {#prerequisites}

A **로컬 개발 환경** 이 자습서를 완료하는 데 필요합니다. macOS에서 실행되는 AEM as a Cloud Service SDK를 사용하여 스크린샷과 비디오를 캡처합니다. 명령 및 코드는 별도로 명시하지 않는 한 로컬 운영 체제와 독립적입니다.

**AEM을 as a Cloud Service으로 처음 사용하십니까?** 다음을 확인하십시오. [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서입니다](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR).

**AEM 6.5를 처음 사용하십니까?** 다음을 확인하십시오. [로컬 개발 환경 설정에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko-KR).

## WKND 참조 사이트 다운로드 및 배포 {#set-up-wknd-site}

이 튜토리얼에서는 WKND 참조 사이트의 Byline 구성 요소를 확장합니다. 로컬 환경에 WKND 코드 베이스를 복제하고 설치합니다.

1. 로컬 빠른 시작 시작 **작성자** 에서 실행되는 AEM 인스턴스 [http://localhost:4502](http://localhost:4502).
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
   > AEM 6.5 및 최신 서비스 팩의 경우 `classic` Maven 명령에 대한 프로필:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 새 브라우저 창을 열고 AEM에 로그인합니다. 열기 **매거진** 다음과 같은 페이지: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![페이지의 인라인 구성 요소](assets/adobe-client-data-layer/byline-component-onpage.png)

   페이지에 경험 조각의 일부로 추가된 인라인 구성 요소의 예가 표시됩니다. 다음에서 경험 조각을 볼 수 있습니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. 개발자 도구를 열고 다음 명령을 입력합니다 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 계층의 현재 상태를 보려면 응답을 검사합니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 계층 응답](assets/data-layer-state-response.png)

   Byline 구성 요소가 데이터 레이어에 나열되어 있지 않은지 확인하십시오.

## Byline Sling 모델 업데이트 {#sling-model}

데이터 레이어에 구성 요소에 대한 데이터를 주입하려면, 먼저 구성 요소의 Sling 모델을 업데이트해 보겠습니다. 그런 다음 Byline의 Java™ 인터페이스 및 Sling 모델 구현을 업데이트하여 새 메서드를 만듭니다 `getData()`. 이 메서드는 데이터 레이어에 삽입할 속성을 포함합니다.

1. 를 엽니다. `aem-guides-wknd` 원하는 IDE에서 프로젝트를 만듭니다. 다음 위치로 이동 `core` 모듈.
1. 파일 열기 `Byline.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

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

1. 파일 열기 `BylineImpl.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. 이는 의 구현입니다. `Byline` 인터페이스하고 Sling 모델로 구현됩니다.

1. 파일의 시작 부분에 다음 가져오기 구문을 추가합니다.

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   다음 `fasterxml.jackson` API는 JSON으로 노출될 데이터를 직렬화하는 데 사용됩니다. 다음 `ComponentUtils` / AEM 핵심 구성 요소를 사용하여 데이터 레이어가 활성화되었는지 확인합니다.

1. 구현되지 않은 메서드 추가 `getData()` 끝 `BylineImple.java`:

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

   위의 메서드에서 `HashMap` 은 JSON으로 노출될 속성을 캡처하는 데 사용됩니다. 기존 메서드는 다음과 같습니다 `getName()` 및 `getOccupations()` 를 사용합니다. 다음 `@type` 구성 요소의 고유 리소스 유형을 나타내므로 클라이언트는 구성 요소 유형을 기반으로 이벤트 및/또는 트리거를 쉽게 식별할 수 있습니다.

   다음 `ObjectMapper` 는 속성을 직렬화하고 JSON 문자열을 반환하는 데 사용됩니다. 그런 다음 이 JSON 문자열을 데이터 레이어에 삽입할 수 있습니다.

1. 터미널 창을 엽니다. 빌드 및 배포 `core` Maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 인라인 HTL 업데이트 {#htl}

그런 다음 를 업데이트합니다. `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en). HTL(HTML 템플릿 언어)은 구성 요소의 HTML을 렌더링하는 데 사용되는 템플릿입니다.

특수 데이터 속성 `data-cmp-data-layer` 각 AEM 구성 요소에서 를 사용하여 데이터 레이어를 노출합니다. AEM 핵심 구성 요소에서 제공하는 JavaScript가 이 데이터 속성을 찾습니다. 이 데이터 속성의 값은 Byline Sling Model의 `getData()` 메서드, Adobe 클라이언트 데이터 레이어에 삽입

1. 를 엽니다. `aem-guides-wknd` 를 IDE에 투영합니다. 다음 위치로 이동 `ui.apps` 모듈.
1. 파일 열기 `byline.html` 위치: `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![필자 HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 업데이트 `byline.html` 다음을 포함 `data-cmp-data-layer` 특성:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   값: `data-cmp-data-layer` 이(가) (으)로 설정되었습니다. `"${byline.data}"` 위치 `byline` 는 이전에 업데이트된 슬링 모델입니다. `.data` 는 HTL에서 Java™ Getter 메서드를 호출하기 위한 표준 표기법입니다. `getData()` 이전 연습에서 구현되었습니다.

1. 터미널 창을 엽니다. 빌드 및 배포 `ui.apps` Maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아간 다음 Byline 구성 요소가 있는 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 개발자 도구를 열고 페이지의 HTML 소스에서 Byline 구성 요소를 검사합니다.

   ![인라인 데이터 레이어](assets/adobe-client-data-layer/byline-data-layer-html.png)

   다음을 참조하십시오. `data-cmp-data-layer` 슬링 모델의 JSON 문자열로 채워집니다.

1. 브라우저의 개발자 도구를 열고 다음 명령을 입력합니다 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 아래의 응답 아래로 이동 `component` 의 인스턴스를 `byline` 구성 요소가 데이터 레이어에 추가되었습니다.

   ![데이터 레이어의 줄 부분](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   다음과 같은 항목이 표시됩니다.

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   노출된 속성이 다음에 추가된 것과 동일한지 확인합니다. `HashMap` 을 참조하십시오.

## 클릭 이벤트 추가 {#click-event}

Adobe 클라이언트 데이터 레이어는 이벤트를 기반으로 하며 작업을 트리거하는 가장 일반적인 이벤트 중 하나는 입니다. `cmp:click` 이벤트. AEM 핵심 구성 요소를 사용하면 데이터 요소를 통해 구성 요소를 쉽게 등록할 수 있습니다. `data-cmp-clickable`.

클릭 가능한 요소는 일반적으로 CTA 버튼 또는 탐색 링크입니다. 안타깝게도 Byline 구성 요소에는 이러한 항목이 없지만 다른 사용자 정의 구성 요소에서 일반적일 수 있으므로 어쨌든 등록하겠습니다.

1. 를 엽니다. `ui.apps` IDE의 모듈
1. 파일 열기 `byline.html` 위치: `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. 업데이트 `byline.html` 다음을 포함 `data-cmp-clickable` byline의 속성 **이름** 요소:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 새 터미널을 엽니다. 빌드 및 배포 `ui.apps` Maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아간 후 Byline 구성 요소가 추가된 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   이벤트를 테스트하기 위해 개발자 콘솔을 사용하여 일부 JavaScript를 수동으로 추가합니다. 다음을 참조하십시오 [AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용](data-layer-overview.md) 이 작업을 수행하는 방법에 대한 비디오입니다.

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

1. 에 다음 메서드를 입력합니다. **콘솔**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   위의 메서드는 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:click` 이벤트 및 호출 `bylineClickHandler`.

   >[!CAUTION]
   >
   > 이는 중요합니다 **아님** 이 연습을 통해 브라우저를 새로 고치려면, 그렇지 않으면 콘솔 JavaScript가 손실됩니다.

1. 브라우저에서 **콘솔** 을 열고 인라인 구성 요소에서 작성자의 이름을 클릭합니다.

   ![클릭한 인라인 구성 요소](assets/adobe-client-data-layer/byline-component-clicked.png)

   콘솔 메시지가 표시됩니다 `Byline Clicked!` 및 Byline 이름입니다.

   다음 `cmp:click` 가장 쉽게 참여할 수 있는 이벤트는 이벤트입니다. 보다 복잡한 구성 요소의 경우 및 다른 동작을 추적하기 위해 사용자 지정 JavaScript를 추가하여 새 이벤트를 추가하고 등록할 수 있습니다. 가장 좋은 예는 를 트리거하는 슬라이드 구성 요소입니다. `cmp:show` 슬라이드가 전환될 때마다 이벤트. 다음을 참조하십시오. [소스 코드 를 참조하십시오](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## DataLayerBuilder 유틸리티 사용 {#data-layer-builder}

Sling 모델이 [업데이트됨](#sling-model) 이 장 앞부분에서 다음을 사용하여 JSON 문자열을 만들도록 선택했습니다. `HashMap` 각 속성을 수동으로 설정하는 것이 좋습니다. 이 방법은 작은 일회성 구성 요소에 대해서는 잘 작동하지만, AEM 핵심 구성 요소를 확장하는 구성 요소에 대해서는 추가 코드가 많이 생길 수 있습니다.

유틸리티 클래스, `DataLayerBuilder`은 대부분의 무거운 리프팅을 수행하기 위해 존재합니다. 이렇게 하면 구현이 원하는 속성만 확장할 수 있습니다. Sling 모델을 업데이트하여 `DataLayerBuilder`.

1. IDE로 돌아가서 `core` 모듈.
1. 파일 열기 `Byline.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. 수정 `getData()` 메서드에서 형식을 반환합니다. `ComponentData`

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

   `ComponentData` 는 AEM 코어 구성 요소에서 제공하는 객체입니다. 이전 예제와 마찬가지로 JSON 문자열이 생성되지만 많은 추가 작업도 수행합니다.

1. 파일 열기 `BylineImpl.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. 다음 가져오기 구문을 추가합니다.

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 바꾸기 `getData()` 메서드를 다음과 같이 바꿉니다.

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

   인라인 구성 요소는 이미지 핵심 구성 요소의 일부를 재사용하여 작성자를 나타내는 이미지를 표시합니다. 위의 스니펫에서 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) 의 데이터 레이어를 확장하는 데 사용됩니다. `Image` 구성 요소. 그러면 사용된 이미지에 대한 모든 데이터로 JSON 개체가 미리 채워집니다. 또한 설정 과 같은 일부 일상적인 기능도 수행합니다. `@type` 구성 요소에 대한 고유 식별자. 이 방법은 작습니다.

   유일한 속성이 `withTitle` 다음 값으로 대체됨: `getName()`.

1. 터미널 창을 엽니다. 빌드 및 배포 `core` Maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE로 돌아가서 `byline.html` 파일: `ui.apps`
1. 사용할 HTL 업데이트 `byline.data.json` 을(를) 채우려면 `data-cmp-data-layer` 특성:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   유형의 개체를 반환한다는 것을 기억하십시오. `ComponentData`. 이 개체에는 getter 메서드가 포함되어 있습니다. `getJson()` 및 를 채우는 데 사용됩니다. `data-cmp-data-layer` 특성.

1. 터미널 창을 엽니다. 빌드 및 배포 `ui.apps` Maven 기술을 사용한 모듈:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 브라우저로 돌아간 후 Byline 구성 요소가 추가된 페이지를 다시 엽니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. 브라우저의 개발자 도구를 열고 다음 명령을 입력합니다 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 아래의 응답 아래로 이동 `component` 의 인스턴스를 `byline` 구성 요소:

   ![인라인 데이터 레이어 업데이트됨](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   이제 다음 항목이 있는지 확인합니다. `image` 내의 개체 `byline` 구성 요소 항목. 여기에는 DAM의 에셋에 대한 자세한 정보가 포함되어 있습니다. 또한 다음을 확인합니다. `@type` 및 고유 id(이 경우 `byline-136073cfcb`)가 자동으로 채워지고 `repo:modifyDate` 구성 요소가 수정된 시기를 나타냅니다.

## 추가 예 {#additional-examples}

1. 데이터 레이어를 확장하는 다른 예는 다음을 검사하여 확인할 수 있습니다. `ImageList` wknd 코드 베이스의 구성 요소:
   * `ImageList.java` - 의 Java 인터페이스 `core` 모듈.
   * `ImageListImpl.java` - 의 Sling 모델 `core` 모듈.
   * `image-list.html` - 의 HTL 템플릿 `ui.apps` 모듈.

   >[!NOTE]
   >
   > 다음과 같은 사용자 지정 속성을 포함하는 것은 좀 더 어렵습니다. `occupation` 사용 시 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). 하지만 이미지 또는 페이지를 포함하는 핵심 구성 요소를 확장하는 경우 시간이 많이 절약됩니다.

   >[!NOTE]
   >
   > 구현 전체에서 재사용되는 오브젝트에 대한 고급 데이터 레이어를 구축하는 경우 데이터 레이어 요소를 고유한 데이터 레이어별 Java™ 오브젝트로 추출하는 것이 좋습니다. 예를 들어 Commerce 핵심 구성 요소에에 대한 인터페이스가 추가되었습니다. `ProductData` 및 `CategoryData` 상거래 구현 내의 많은 구성 요소에서 사용할 수 있기 때문입니다. 리뷰 [aem-cif-core-components 저장소의 코드](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) 을 참조하십시오.

## 축하합니다! {#congratulations}

AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 를 확장하고 사용자 지정하는 몇 가지 방법을 살펴보았습니다.

## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [핵심 구성 요소와 데이터 레이어 통합](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
