---
title: 코어 구성 요소 확장 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA 편집기에서 사용할 기존 핵심 구성 요소에 대한 JSON 모델을 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다. Sling 모델 및 Sling Resource Merger 기능을 확장하는 데 위임 패턴을 사용하는 방법을 알아봅니다.
sub-product: 사이트
feature: SPA 편집기, 핵심 구성 요소
doc-type: tutorial
version: cloud-service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 0%

---


# 코어 구성 요소 확장 {#extend-component}

AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 사용자 지정하고 확장하는 강력한 방법입니다.

## 목표

1. 추가 속성 및 콘텐츠로 기존 코어 구성 요소를 확장합니다.
2. `sling:resourceSuperType`을 사용하여 구성 요소 상속의 기본 사항을 이해합니다.
3. Sling 모델용 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)을 활용하여 기존 로직 및 기능을 다시 사용하는 방법을 알아봅니다.

## 빌드할 내용

이 장에서는 새 `Banner` 구성 요소에 대한 요구 사항을 충족하기 위해 표준 `Image` 구성 요소에 추가 속성을 추가하는 데 필요한 추가 코드를 보여줍니다. `Banner` 구성 요소에는 표준 `Image` 구성 요소와 동일한 모든 속성이 포함되어 있지만, 사용자가 **배너 텍스트**&#x200B;를 채울 수 있는 추가 속성이 포함되어 있습니다.

![최종 작성 배너 구성 요소](assets/extend-component/final-author-banner-component.png)

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. 이 시점에서 자습서 사용자는 AEM SPA 편집기 기능을 명확하게 이해할 수 있다고 가정합니다.

## Sling 리소스 슈퍼 유형 {#sling-resource-super-type}이(가) 있는 상속

기존 구성 요소를 확장하려면 구성 요소의 정의에 `sling:resourceSuperType` 속성을 설정합니다.  `sling:resourceSuperType`는  [](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 다른 구성 요소를 가리키는 AEM 구성 요소 정의에 설정할 수 있는 속성입니다. 이렇게 하면 구성 요소가 `sling:resourceSuperType`(으)로 식별된 구성 요소의 모든 기능을 상속하도록 명시적으로 설정됩니다.

`wknd-spa-react/components/image`에서 `Image` 구성 요소를 확장하려면 `ui.apps` 모듈에서 코드를 업데이트해야 합니다.

1. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`에서 `banner`에 대한 `ui.apps` 모듈 아래에 새 폴더를 만듭니다.
1. `banner` 아래에 다음과 같이 구성 요소 정의(`.content.xml`)를 만듭니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   `wknd-spa-react/components/banner`을 설정하여 `wknd-spa-react/components/image`의 모든 기능을 상속합니다.

## cq:editConfig {#cq-edit-config}

`_cq_editConfig.xml` 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 나타냅니다. 이미지 구성 요소를 확장할 때 리소스 유형이 구성 요소 자체와 일치해야 합니다.

1. `ui.apps` 모듈에서 `_cq_editConfig.xml` 라는 이름의 `banner` 아래에 다른 파일을 만듭니다.
1. 다음 XML로 `_cq_editConfig.xml` 채우기:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. 파일의 고유한 양상은 resourceType을 `wknd-spa-react/components/banner`로 설정하는 `<parameters>` 노드입니다.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소는 `_cq_editConfig`이 필요하지 않습니다. 이미지 구성 요소 및 하위 항목은 예외입니다.

## 대화 상자 {#extend-dialog} 확장

`Banner` 구성 요소에는 `bannerText`을 캡처하기 위한 대화 상자에 추가 텍스트 필드가 필요합니다. Sling 상속을 사용하고 있으므로 [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html)의 기능을 사용하여 대화 상자의 부분을 대체하거나 확장할 수 있습니다. 이 샘플에서는 카드 구성 요소를 채우기 위해 작성자의 추가 데이터를 캡처하기 위해 대화 상자에 새 탭이 추가되었습니다.

1. `ui.apps` 모듈의 `banner` 폴더 아래에서 `_cq_dialog` 폴더를 만듭니다.
1. `_cq_dialog` 아래에 대화 상자 정의 파일 `.content.xml`을 만듭니다. 다음과 같이 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   위의 XML 정의에서는 **Text**&#x200B;라는 새 탭을 만들고, 기존 **Asset** 탭 전에 *순서를 지정합니다.* 여기에는 단일 필드 **배너 텍스트**&#x200B;가 포함됩니다.

1. 대화 상자는 다음과 같습니다.

   ![배너 최종 대화 상자](assets/extend-component/banner-dialog.png)

   **자산** 또는 **메타데이터**&#x200B;에 대한 탭을 정의할 필요가 없습니다. 이러한 속성은 `sling:resourceSuperType` 속성을 통해 상속됩니다.

   대화 상자를 미리 보려면 SPA 구성 요소 및 `MapTo` 함수를 구현해야 합니다.

## SPA 구성 요소 구현 {#implement-spa-component}

배너 구성 요소를 SPA 편집기와 함께 사용하려면 `wknd-spa-react/components/banner`에 매핑되는 새 SPA 구성 요소를 만들어야 합니다. 이 작업은 `ui.frontend` 모듈에서 수행됩니다.

1. `ui.frontend` 모듈에서 `ui.frontend/src/components/Banner`에 대한 새 폴더를 만듭니다.`Banner`
1. `Banner` 폴더 아래에 `Banner.js` 라는 새 파일을 만듭니다. 다음과 같이 채웁니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
   
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if(BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   이 SPA 구성 요소는 이전에 만든 AEM 구성 요소 `wknd-spa-react/components/banner`에 매핑됩니다.

1. 새 `Banner` SPA 구성 요소를 포함하도록 `ui.frontend/src/components/import-components.js`에서 `import-components.js`을 업데이트합니다.

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 이 시점에서 프로젝트를 AEM에 배포할 수 있으며 대화 상자를 테스트할 수 있습니다. Maven 기술을 사용하여 프로젝트를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. SPA 템플릿의 정책을 업데이트하여 `Banner` 구성 요소를 **허용된 구성 요소**&#x200B;로 추가합니다.

1. SPA 페이지로 이동하고 `Banner` 구성 요소를 SPA 페이지 중 하나에 추가합니다.

   ![배너 구성 요소 추가](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 대화 상자에서는 **배너 텍스트**&#x200B;에 대한 값을 저장할 수 있지만 이 값은 SPA 구성 요소에 반영되지 않습니다. 활성화하려면 구성 요소에 대한 Sling 모델을 확장해야 합니다.

## Java 인터페이스 추가 {#java-interface}

구성 요소 대화 상자의 값을 React 구성 요소에 최종적으로 노출하려면 `Banner` 구성 요소에 대한 JSON을 채우는 Sling 모델을 업데이트해야 합니다. 이 작업은 SPA 프로젝트에 대한 모든 Java 코드가 포함된 `core` 모듈에서 수행됩니다.

먼저 `Image` Java 인터페이스를 확장하는 `Banner`에 대한 새 Java 인터페이스를 만듭니다.

1. `core` 모듈에서 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`에 `BannerModel.java` 라는 새 파일을 만듭니다.
1. `BannerModel.java` 을 다음과 같이 채웁니다.

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   이 작업은 코어 구성 요소 `Image` 인터페이스에서 모든 메서드를 상속하고 하나의 새 메서드 `getBannerText()`를 추가합니다.

## Sling 모델 구현 {#sling-model}

그런 다음 `BannerModel` 인터페이스에 대한 Sling 모델을 구현합니다.

1. `core` 모듈에서 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`에 `BannerModelImpl.java` 라는 새 파일을 만듭니다.

1. `BannerModelImpl.java` 을 다음과 같이 채웁니다.

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
       )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   
   }
   ```

   `@Model` 및 `@Exporter` 주석을 사용하여 Sling 모델을 Sling Model Exporter를 통해 JSON으로 직렬화할 수 있는지 확인합니다.

   `BannerModelImpl.java` 는  [Sling Models에 위임 ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 패턴을 사용하여 이미지 코어 구성 요소에서 모든 논리를 다시 작성하지 않습니다.

1. 다음 줄을 준수합니다.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 `Banner` 구성 요소의 `sling:resourceSuperType` 상속을 기반으로 `image`이라는 이미지 개체를 인스턴스화합니다.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그런 다음 `image` 개체를 사용하여 논리를 직접 작성하지 않고도 `Image` 인터페이스에 정의된 메서드를 구현할 수 있습니다. 이 기법은 `getSrc()`, `getAlt()` 및 `getTitle()`에 사용됩니다.

1. 터미널 창을 열고 `core` 디렉토리의 Maven `autoInstallBundle` 프로필을 사용하여 `core` 모듈에 대한 업데이트만 배포합니다.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 모두 함께 {#put-together}

1. AEM으로 돌아가서 `Banner` 구성 요소가 있는 SPA 페이지를 엽니다.
1. **배너 텍스트**&#x200B;를 포함하도록 `Banner` 구성 요소를 업데이트합니다.

   ![배너 텍스트](assets/extend-component/banner-text-dialog.png)

1. 이미지로 구성 요소 채우기:

   ![배너 대화 상자에 이미지 추가](assets/extend-component/banner-dialog-image.png)

   대화 상자 업데이트를 저장합니다.

1. 이제 **배너 텍스트**&#x200B;의 렌더링된 값이 표시됩니다.

   ![표시되는 배너 텍스트](assets/extend-component/banner-text-displayed.png)

1. 다음 위치에서 JSON 모델 응답을 봅니다.[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 및 `wknd-spa-react/components/card` 검색:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   `BannerModelImpl.java`에서 Sling 모델을 구현하면 JSON 모델이 추가 키/값 쌍으로 업데이트됩니다.

## 축하합니다! {#congratulations}

축하합니다. 를 사용하여 AEM 구성 요소를 확장하는 방법과 Sling 모델 및 대화 상자가 JSON 모델로 작동하는 방법을 알아보았습니다.

