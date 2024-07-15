---
title: 사용자 지정 날씨 구성 요소 만들기 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA 편집기에서 사용할 사용자 지정 날씨 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법에 대해 알아봅니다. Open Weather API 및 React Open Weather 구성 요소가 사용됩니다.
feature: SPA Editor
version: Cloud Service
jira: KT-5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 82466e0e-b573-440d-b806-920f3585b638
duration: 323
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1105'
ht-degree: 0%

---

# 사용자 지정 WeatherComponent 만들기 {#custom-component}

AEM SPA 편집기에서 사용할 사용자 지정 날씨 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법에 대해 알아봅니다. [날씨 열기 API](https://openweathermap.org) 및 [React 날씨 열기 구성 요소](https://www.npmjs.com/package/react-open-weather)가 사용됩니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작하는 Sling 모델의 역할을 이해합니다.
2. 새 AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. SPA 편집기 프레임워크와 호환되는 **custom** AEM 구성 요소를 만드는 방법에 대해 알아봅니다.

## 빌드할 내용

간단한 날씨 구성 요소가 구축됩니다. 콘텐츠 작성자는 이 구성 요소를 SPA에 추가할 수 있습니다. 작성자가 AEM 대화 상자를 사용하여 표시할 날씨 위치를 설정할 수 있습니다.  이 구성 요소의 구현은 AEM SPA Editor 프레임워크와 호환되는 새로운 AEM 구성 요소를 만드는 데 필요한 단계를 보여 줍니다.

![기상 구성 요소 열기](assets/custom-component/enter-dialog.png)

## 사전 요구 사항

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. 이 장은 [탐색 및 라우팅](navigation-routing.md) 장의 연속이지만 로컬 AEM 인스턴스에 배포된 SPA 사용 AEM 프로젝트만 있으면 됩니다.

### 날씨 API 키 열기

자습서와 함께 따라가려면 [날씨 열기](https://openweathermap.org/)의 API 키가 필요합니다. [제한된 API 호출의 경우 무료로 등록할 수 있습니다](https://home.openweathermap.org/users/sign_up).

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드 및 속성은 `ui.apps` 모듈에서 XML 파일로 표시됩니다. 그런 다음 `ui.apps` 모듈에서 AEM 구성 요소를 만듭니다.

>[!NOTE]
>
> AEM 구성 요소의 [기본 사항에 대한 빠른 새로 고침이 도움이 될 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 선택한 IDE에서 `ui.apps` 폴더를 엽니다.
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components`(으)로 이동하여 `open-weather`(이)라는 새 폴더를 만드십시오.
3. `open-weather` 폴더 아래에 이름이 `.content.xml`인 새 파일을 만듭니다. `open-weather/.content.xml`을(를) 다음과 같이 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title`은(는) 콘텐츠 작성자에게 표시되는 값이며 `componentGroup`은(는) 작성 UI에서 구성 요소의 그룹화를 결정합니다.

4. `custom-component` 폴더 아래에 `_cq_dialog` 폴더를 만듭니다.
5. `_cq_dialog` 폴더 아래에서 `.content.xml`(이)라는 새 파일을 만들고 다음과 같이 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![사용자 지정 구성 요소 정의](assets/custom-component/dialog-custom-component-defintion.png)

   위의 XML 파일은 `Weather Component`에 대한 매우 간단한 대화 상자를 생성합니다. 파일의 중요한 부분은 내부 `<label>`, `<lat>` 및 `<lon>` 노드입니다. 이 대화 상자에는 두 개의 `numberfield`과(와) 사용자가 표시할 날씨를 구성할 수 있는 `textfield`이(가) 포함되어 있습니다.

   JSON 모델을 통해 `label`,`lat` 및 `long` 속성의 값을 표시하기 위해 슬링 모델이 옆에 만들어집니다.

   >[!NOTE]
   >
   > 핵심 구성 요소 정의를 보면 훨씬 많은 [대화 상자의 예제를 볼 수 있습니다](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)의 `/libs/granite/ui/components/coral/foundation/form` 아래에서 사용할 수 있는 `select`, `textarea`, `pathfield`과(와) 같은 추가 양식 필드를 볼 수도 있습니다.

   기존 AEM 구성 요소의 경우 일반적으로 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=ko-KR) 스크립트가 필요합니다. SPA은 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## Sling 모델 만들기

Sling 모델은 JCR에서 Java 변수로의 데이터 매핑을 용이하게 하는 주석 기반 Java &quot;POJO&quot;(일반 이전 Java 개체)입니다. [Sling 모델](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models)은 일반적으로 AEM 구성 요소에 대한 복잡한 서버측 비즈니스 논리를 캡슐화하는 기능을 합니다.

SPA 편집기의 컨텍스트에서 Sling 모델은 [Sling 모델 내보내기 도구](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=ko-KR)를 사용하는 기능을 통해 JSON 모델을 통해 구성 요소의 콘텐츠를 노출합니다.

1. 선택한 IDE에서 `aem-guides-wknd-spa.react/core`에 `core` 모듈을 엽니다.
1. `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`의 `OpenWeatherModel.java`에 이름이 지정된 파일을 만듭니다.
1. `OpenWeatherModel.java`을(를) 다음으로 채우기:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   구성 요소의 Java 인터페이스입니다. Sling 모델이 SPA Editor 프레임워크와 호환되도록 하려면 `ComponentExporter` 클래스를 확장해야 합니다.

1. `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models` 아래에 이름이 `impl`인 폴더를 만듭니다.
1. `impl` 아래에 이름이 `OpenWeatherModelImpl.java`인 파일을 만들고 다음을 채웁니다.

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   정적 변수 `RESOURCE_TYPE`은(는) 구성 요소의 `ui.apps`에 있는 경로를 가리켜야 합니다. `getExportedType()`은(는) `MapTo`을(를) 통해 SPA 구성 요소에 JSON 속성을 매핑하는 데 사용됩니다. `@ValueMapValue`은(는) 대화 상자에 의해 저장된 jcr 속성을 읽는 주석입니다.

## SPA 업데이트

그런 다음 React 코드를 업데이트하여 [React Open Weather 구성 요소](https://www.npmjs.com/package/react-open-weather)를 포함하고 이전 단계에서 만든 AEM 구성 요소에 매핑합니다.

1. React Open Weather 구성 요소를 **npm** 종속성으로 설치합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. `ui.frontend/src/components/OpenWeather`에 이름이 `OpenWeather`인 새 폴더를 만듭니다.
1. 이름이 `OpenWeather.js`인 파일을 추가하고 다음으로 채웁니다.

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. `OpenWeather` 구성 요소를 포함하도록 `ui.frontend/src/components/import-components.js`에서 `import-components.js` 업데이트:

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Maven 기술을 사용하여 프로젝트 디렉토리의 루트에서 로컬 AEM 환경에 모든 업데이트를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 템플릿 정책 업데이트

그런 다음 AEM으로 이동하여 업데이트를 확인하고 `OpenWeather` 구성 요소를 SPA에 추가할 수 있습니다.

1. [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)(으)로 이동하여 새 Sling 모델의 등록을 확인하십시오.

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   `OpenWeatherModelImpl`이(가) `wknd-spa-react/components/open-weather` 구성 요소와 연결되어 있고 슬링 모델 익스포터를 통해 등록되었음을 나타내는 위의 두 줄이 표시됩니다.

1. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)에 있는 SPA 페이지 템플릿으로 이동합니다.
1. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Open Weather`을(를) 허용된 구성 요소로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 내용을 저장하고 `Open Weather`을(를) 허용된 구성 요소로 관찰합니다.

   ![허용된 구성 요소로서 사용자 지정 구성 요소](assets/custom-component/custom-component-allowed-layout-container.png)

## 개방형 날씨 구성 요소 작성

그런 다음 AEM SPA 편집기를 사용하여 `Open Weather` 구성 요소를 작성합니다.

1. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)(으)로 이동합니다.
1. `Edit` 모드에서 `Open Weather`을(를) `Layout Container`에 추가합니다.

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

1. 구성 요소의 대화 상자를 열고 **레이블**, **위도** 및 **경도**&#x200B;를 입력합니다. 예: **샌디에이고**, **32.7157** 및 **-117.1611**. 서반구 및 남반구 숫자는 Open Weather API를 사용하여 음수로 표시됩니다

   ![기상 구성 요소 열기](assets/custom-component/enter-dialog.png)

   이 대화 상자는 이 장 앞부분에서 XML 파일을 기반으로 만든 대화 상자입니다.

1. 변경 사항을 저장합니다. **샌디에이고**&#x200B;의 날씨가 표시됩니다.

   ![날씨 구성 요소 업데이트됨](assets/custom-component/weather-updated.png)

1. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)(으)로 이동하여 JSON 모델을 봅니다. `wknd-spa-react/components/open-weather` 검색:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   JSON 값은 Sling 모델에 의해 출력됩니다. 이러한 JSON 값은 React 구성 요소에 prop으로 전달됩니다.

## 축하합니다! {#congratulations}

축하합니다. SPA 편집기에서 사용할 사용자 지정 AEM 구성 요소를 만드는 방법에 대해 알아보았습니다. 또한 대화 상자, JCR 속성 및 Sling 모델이 상호 작용하여 JSON 모델을 출력하는 방법에 대해 알아보았습니다.

### 다음 단계 {#next-steps}

[핵심 구성 요소 확장](extend-component.md) - AEM SPA 편집기에서 사용할 기존 AEM 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 콘텐츠를 추가하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 확장하는 강력한 기술입니다.
