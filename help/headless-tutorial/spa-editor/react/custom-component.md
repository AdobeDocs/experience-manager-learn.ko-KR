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

AEM SPA 편집기에서 사용할 사용자 지정 날씨 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법에 대해 알아봅니다. 다음 [날씨 API 열기](https://openweathermap.org) 및 [React Open Weather 구성 요소](https://www.npmjs.com/package/react-open-weather) 를 사용합니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작하는 Sling 모델의 역할을 이해합니다.
2. 새 AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. 다음을 만드는 방법 알아보기 **사용자 정의** AEM 편집기 프레임워크와 호환되는 SPA 구성 요소입니다.

## 빌드할 내용

간단한 날씨 구성 요소가 구축됩니다. 콘텐츠 작성자는 이 구성 요소를 SPA에 추가할 수 있습니다. 작성자가 AEM 대화 상자를 사용하여 표시할 날씨 위치를 설정할 수 있습니다.  이 구성 요소의 구현은 AEM SPA Editor 프레임워크와 호환되는 새로운 AEM 구성 요소를 만드는 데 필요한 단계를 보여 줍니다.

![개방형 날씨 구성 요소 구성](assets/custom-component/enter-dialog.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment). 이 장은 의 연속입니다. [탐색 및 라우팅](navigation-routing.md) 챕터에서는 로컬 AEM 인스턴스에 배포된 SPA 사용 AEM 프로젝트만 수행하면 됩니다.

### 날씨 API 키 열기

의 API 키 [탁 트인 날씨](https://openweathermap.org/) 은 자습서와 함께 따라가야 합니다. [무료 등록](https://home.openweathermap.org/users/sign_up) 제한된 양의 API 호출.

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드 및 속성은 의 XML 파일로 표시됩니다. `ui.apps` 모듈. 그런 다음,에서 AEM 구성 요소를 만듭니다. `ui.apps` 모듈.

>[!NOTE]
>
> 에 대한 빠른 새로 고침 [AEM 구성 요소의 기본 사항이 유용할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 선택한 IDE에서 `ui.apps` 폴더를 삭제합니다.
2. 다음으로 이동 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` 및 (이)라는 이름의 새 폴더 만들기 `open-weather`.
3. 이름이 인 새 파일 만들기 `.content.xml` 아래에 `open-weather` 폴더를 삭제합니다. 채우기 `open-weather/.content.xml` 다음을 사용하여:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title` 는 콘텐츠 작성자에게 표시되는 값이며 `componentGroup` 제작 UI에서 구성 요소의 그룹화를 결정합니다.

4. 아래 `custom-component` 폴더, (이)라는 다른 폴더를 만듭니다. `_cq_dialog`.
5. 아래 `_cq_dialog` 폴더 (이)라는 이름의 새 파일을 만듭니다. `.content.xml` 다음을 입력합니다.

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

   위의 XML 파일은 `Weather Component`. 파일의 중요한 부분은 내부입니다 `<label>`, `<lat>` 및 `<lon>` 노드. 이 대화 상자에는 두 개의 `numberfield`s 및 a `textfield` 이를 통해 사용자가 표시할 날씨를 구성할 수 있습니다.

   값을 표시하기 위해 옆에 슬링 모델 이 만들어집니다. `label`,`lat` 및 `long` json 모델을 통한 속성.

   >[!NOTE]
   >
   > 훨씬 더 많은 항목을 볼 수 있습니다. [핵심 구성 요소 정의를 보는 대화 상자의 예](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 다음과 같은 추가 양식 필드를 볼 수도 있습니다 `select`, `textarea`, `pathfield`, 아래에서 사용 가능 `/libs/granite/ui/components/coral/foundation/form` 위치: [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   기존 AEM 구성 요소를 사용하여 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=ko-KR) 일반적으로 스크립트가 필요합니다. SPA은 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## Sling 모델 만들기

Sling 모델은 JCR에서 Java 변수로의 데이터 매핑을 용이하게 하는 주석 기반 Java &quot;POJO&quot;(일반 이전 Java 개체)입니다. [Sling 모델](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) 일반적으로 AEM 구성 요소에 대한 복잡한 서버측 비즈니스 논리를 캡슐화하는 역할을 합니다.

SPA 편집기의 컨텍스트에서 슬링 모델은 를 사용하는 기능을 통해 JSON 모델을 통해 구성 요소의 콘텐츠를 노출합니다. [Sling 모델 내보내기](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=ko-KR).

1. 선택한 IDE에서 `core` 모듈 위치: `aem-guides-wknd-spa.react/core`.
1. 다음 위치에 이름이 지정된 파일 만들기 `OpenWeatherModel.java` 위치: `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 채우기 `OpenWeatherModel.java` 다음을 사용하여:

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

   구성 요소의 Java 인터페이스입니다. Sling 모델이 SPA Editor 프레임워크와 호환되려면 를 확장해야 합니다. `ComponentExporter` 클래스.

1. 다음 이름의 폴더 만들기 `impl` 아래에 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 이름이 인 파일 만들기 `OpenWeatherModelImpl.java` 아래에 `impl` 다음을 채웁니다.

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

   정적 변수 `RESOURCE_TYPE` 의 경로를 가리켜야 합니다. `ui.apps` 구성 요소. 다음 `getExportedType()` 를 통해 SPA 구성 요소에 JSON 속성을 매핑하는 데 사용됩니다. `MapTo`. `@ValueMapValue` 는 대화 상자에 의해 저장된 jcr 속성을 읽는 주석입니다.

## SPA 업데이트

그런 다음 React 코드를 업데이트하여 다음을 포함합니다. [React Open Weather 구성 요소](https://www.npmjs.com/package/react-open-weather) 이전 단계에서 만든 AEM 구성 요소에 매핑하게 합니다.

1. React Open Weather 구성 요소를 로 설치 **npm** 종속성:

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. (이)라는 이름의 새 폴더 만들기 `OpenWeather` 위치: `ui.frontend/src/components/OpenWeather`.
1. 이름이 인 파일 추가 `OpenWeather.js` 다음을 입력합니다.

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

1. 업데이트 `import-components.js` 위치: `ui.frontend/src/components/import-components.js` 다음을 포함 `OpenWeather` 구성 요소:

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

그런 다음 AEM으로 이동하여 업데이트를 확인하고 `OpenWeather` SPA에 추가할 구성 요소입니다.

1. 로 이동하여 새 Sling 모델 등록 확인 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   위의 두 줄은 다음을 나타냅니다. `OpenWeatherModelImpl` 이(가) 와(과) 연결되어 있습니다. `wknd-spa-react/components/open-weather` 구성 요소와 Sling 모델 내보내기를 통해 등록되었는지 확인합니다.

1. 다음 SPA 페이지 템플릿으로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 레이아웃 컨테이너의 정책을 업데이트하여 새 항목 추가 `Open Weather` 허용된 구성 요소:

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 다음을 확인합니다. `Open Weather` 허용된 구성 요소:

   ![사용자 지정 구성 요소를 허용된 구성 요소로 사용](assets/custom-component/custom-component-allowed-layout-container.png)

## 개방형 날씨 구성 요소 작성

다음으로, 를 작성합니다. `Open Weather` AEM SPA 편집기를 사용하는 구성 요소입니다.

1. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. 위치 `Edit` 모드, 추가 `Open Weather` (으)로 `Layout Container`:

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

1. 구성 요소의 대화 상자를 열고 **레이블**, **위도**, 및 **경도**. 예 **샌디에이고**, **32.7157**, 및 **-117.1611**. 서반구 및 남반구 숫자는 Open Weather API를 사용하여 음수로 표시됩니다

   ![개방형 날씨 구성 요소 구성](assets/custom-component/enter-dialog.png)

   이 대화 상자는 이 장 앞부분에서 XML 파일을 기반으로 만든 대화 상자입니다.

1. 변경 사항을 저장합니다. 다음 기간 동안 날씨를 관찰하십시오. **샌디에이고** 은(는) 현재 다음과 같이 표시됩니다.

   ![날씨 구성 요소 업데이트됨](assets/custom-component/weather-updated.png)

1. 로 이동하여 JSON 모델 보기 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 검색 대상 `wknd-spa-react/components/open-weather`:

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
