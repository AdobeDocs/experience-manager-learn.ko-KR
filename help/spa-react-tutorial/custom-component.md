---
title: 사용자 지정 구성 요소 만들기 | AEM SPA 편집기 시작 및 반응
description: AEM SPA Editor에서 사용할 사용자 정의 구성 요소를 만드는 방법을 알아봅니다. 작성 대화 상자와 Sling Models를 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '1486'
ht-degree: 1%

---


# 사용자 지정 구성 요소 만들기 {#custom-component}

AEM SPA Editor에서 사용할 사용자 정의 구성 요소를 만드는 방법을 알아봅니다. 작성 대화 상자와 Sling Models를 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작할 때 Sling Models의 역할을 파악합니다.
2. 새 AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. SPA 편집기 프레임워크와 호환될 **맞춤형** AEM 구성 요소를 만드는 방법을 학습합니다.

## 구축 내용

이전 장의 초점은 SPA 구성 요소를 개발하고 *기존* AEM 코어 구성 요소에 매핑하는 것이었습니다. 이 장에서는 *새로운* AEM 구성 요소를 만들고 확장하며 AEM에서 제공하는 JSON 모델을 조작하는 방법에 대해 설명합니다.

간단한 `Custom Component` 설명으로 새 AEM 구성 요소를 만드는 데 필요한 단계를 설명합니다.

![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

## 전제 조건

필요한 도구 및 [로컬 개발 환경 설정을 위한 지침을 검토하십시오](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM [6.x를](overview.md#compatibility) 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트에 필요한 완성된 패키지를 설치합니다](https://github.com/adobe/aem-guides-wknd/releases/latest). WKND 참조 사이트 [에서](https://github.com/adobe/aem-guides-wknd/releases/latest) 제공한 이미지는 WKND SPA에서 다시 사용됩니다. 이 패키지는 [AEM Package Manager를 사용하여 설치할 수 있습니다](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) 완료된 코드를 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/custom-component-solution`있습니다.

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드와 속성은 `ui.apps` 모듈에서 XML 파일로 표현됩니다. 그런 다음 `ui.apps` 모듈에서 AEM 구성 요소를 만듭니다.

>[!NOTE]
>
> AEM 구성 요소의 [기본 사항에 대한 신속한 재교육은 도움이 될 수 있습니다](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html).

1. 선택한 IDE에서 `ui.apps` 폴더를 엽니다.
2. 새 폴더 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` 로 이동하여 만듭니다 `custom-component`.
3. 폴더 아래에 이름이 지정된 새 파일 `.content.xml` 을 `custom-component` 만듭니다. 다음 `custom-component/.content.xml` 으로 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title` 는 컨텐츠 작성자에게 표시될 값이며 작성 UI에서 구성 요소의 그룹을 `componentGroup` 결정합니다.

4. 폴더 아래에서 `custom-component` 다른 폴더를 만듭니다 `_cq_dialog`.
5. 폴더 아래에서 `_cq_dialog` 새 파일 이름 `.content.xml` 을 만들어 다음과 같이 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   위의 XML 파일은 에 대해 매우 간단한 대화 상자를 생성합니다 `Custom Component`. 파일의 중요한 부분은 내부 `<message>` 노드입니다. 이 대화 상자에는 간단한 `textfield` 이름 `Message` 이 포함되어 있으며 텍스트 필드의 값을 이름의 속성으로 지속합니다 `message`.

   JSON 모델을 통해 속성 값을 노출하는 옆에 슬링 모델이 `message` 만들어집니다.

   >[!NOTE]
   >
   > 코어 구성 요소 정의를 보면 대화 상자의 [예도 더 많이 볼 수 있습니다](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 또한 CRXDE-Lite에서 아래 `select`에서 사용할 수 있는 추가 양식 필드 `textarea`를 볼 수 `pathfield`있습니다 `/libs/granite/ui/components/coral/foundation/form` [](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   기존의 AEM 구성 요소에서는 [HTL](https://docs.adobe.com/content/help/ko-KR/experience-manager-htl/using/overview.html) 스크립트가 일반적으로 필요합니다. SPA에서 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## 슬링 모델 만들기

Sling Models는 JCR에서 Java 변수에 대한 데이터 매핑을 용이하게 하는 주석 기반 Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)입니다. [Sling Models는](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) 일반적으로 AEM Components의 복잡한 서버측 비즈니스 로직을 캡슐화하는 기능을 제공합니다.

SPA 편집기의 컨텍스트에서 Sling Models는 Sling Model Exporter를 사용하는 기능을 통해 JSON 모델을 통해 구성 요소의 컨텐츠를 [노출합니다](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. 선택한 IDE에서 `core` 모듈을 엽니다. `CustomComponent.java` 또한 장 시작 코드의 일부로 `CustomComponentImpl.java` 이미 제작되어 게시되었습니다.

   >[!NOTE]
   >
   > Visual Studio 코드 IDE를 사용하는 경우 Java용 [익스텐션을 설치하는 데 도움이 될 수 있습니다](https://code.visualstudio.com/docs/java/extensions).

2. 다음 위치에서 Java 인터페이스 `CustomComponent.java` 를 엽니다 `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`.

   ![CustomComponent.java 인터페이스](assets/custom-component/custom-component-interface.png)

   Sling Model에서 구현되는 Java 인터페이스입니다.

3. 인터페이스 `CustomComponent.java` 를 확장하도록 `ComponentExporter` 업데이트합니다.

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   인터페이스 `ComponentExporter` 구현은 JSON 모델 API에서 자동으로 선택하는 Sling Model의 요구 사항입니다.

   인터페이스에는 단일 getter 메서드가 포함되어 있습니다 `CustomComponent` `getMessage()`. JSON 모델을 통해 작성 대화 상자의 값을 노출하는 방법입니다. 빈 매개 변수를 사용하는 공용 getter 메서드만 JSON 모델에 `()` 내보내집니다.

4. 에서 `CustomComponentImpl.java` 엽니다 `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`.

   인터페이스 구현입니다 `CustomComponent` . 주석은 Java 클래스를 슬링 모델로 식별합니다. `@Model` 주석을 사용하면 Java 클래스가 Sling Model Exporter를 통해 시리얼라이즈 및 내보낼 수 있습니다. `@Exporter`

5. 정적 변수 `RESOURCE_TYPE` 를 업데이트하여 이전 연습 `wknd-spa-react/components/custom-component` 에서 만든 AEM 구성 요소를 가리킵니다.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   구성 요소의 리소스 유형은 Sling 모델을 AEM 구성 요소에 바인딩하는 것이며 궁극적으로 React 구성 요소에 매핑됩니다.

6. 구성 요소 리소스 `getExportedType()` 유형을 반환하려면 `CustomComponentImpl` 클래스에 메서드를 추가합니다.

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   이 방법은 인터페이스를 구현할 때 필요하며, React 구성 요소에 매핑을 허용하는 리소스 유형을 노출합니다. `ComponentExporter`

7. 작성 대화 상자 `getMessage()` 에 의해 지속된 `message` 속성의 값을 반환하려면 메서드를 업데이트합니다. 주석을 `@ValueMap` 사용하면 JCR 값이 Java 변수 `message` 에 매핑됩니다.

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   일부 추가 &quot;비즈니스 논리&quot;가 추가되어 메시지의 문자열 값을 모든 대문자로 반환합니다. 이를 통해 작성자 대화 상자에 저장된 원시 값과 스링 모델에 의해 노출된 값 간의 차이를 확인할 수 있습니다.

   >[!NOTE]
   >
   > 여기서 [완성된 CustomComponentImpl.java를 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java).

## 반응 구성 요소 업데이트

사용자 지정 구성 요소에 대한 반응 코드가 이미 만들어졌습니다. 다음으로, React 구성 요소를 AEM 구성 요소에 매핑하기 위해 몇 가지 업데이트를 만듭니다.

1. 모듈에서 파일을 `ui.frontend` 엽니다 `ui.frontend/src/components/Custom/Custom.js`.
2. 변수의 `{this.props.message}` 일부로서 다음 방법을 `render()` 관찰합니다.

   ```js
   return (
           <div class="CustomComponent">
               <h2 class="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   Sling 모델의 변형된 대문자 값이 이 `message` 속성에 매핑될 것으로 예상됩니다.

3. AEM SPA Editor JS SDK에서 `MapTo` 개체를 가져와 AEM 구성 요소에 매핑합니다.

   ```diff
   + import {MapTo} from '@adobe/aem-react-editable-components';
   
    ...
    export default class Custom extends Component {
        ...
    }
   
   + MapTo('wknd-spa-react/components/custom-component')(Custom, CustomEditConfig);
   ```

4. Maven 기술을 사용하여 프로젝트 디렉토리의 루트에서 로컬 AEM 환경에 모든 업데이트를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 템플릿 정책 업데이트

다음으로 AEM으로 이동하여 업데이트를 확인하고 SPA에 업데이트를 추가할 수 `Custom Component` 있도록 허용합니다.

1. http://localhost:4502/system/console/status-slingmodels으로 이동하여 새 Sling Model의 등록을 [확인합니다](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   위의 두 줄은 구성 요소와 `CustomComponentImpl` 연결되어 있고 Sling 모델 내보내기 도구를 통해 등록되었음을 나타내는 `wknd-spa-react/components/custom-component` 것입니다.

2. http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html에서 SPA 페이지 템플릿으로 [이동합니다](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 구성 요소를 허용된 구성 요소 `Custom Component` 로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 허용된 구성 요소 `Custom Component` 로 확인합니다.

   ![사용자 지정 구성 요소를 허용된 구성 요소로 사용](assets/custom-component/custom-component-allowed-layout-container.png)

## 사용자 지정 구성 요소 작성

다음으로 AEM SPA Editor를 `Custom Component` 사용하여 컨텐츠를 제작합니다.

1. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html으로 [이동합니다](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. 모드 `Edit` 에서 다음에 `Custom Component` 를 `Layout Container`추가합니다.

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

3. 구성 요소의 대화 상자를 열고 소문자가 포함된 메시지를 입력합니다.

   ![사용자 지정 구성 요소 구성](assets/custom-component/enter-dialog-message.png)

   장 앞의 XML 파일을 기반으로 만든 대화 상자입니다.

4. 변경 사항을 저장합니다. 표시된 메시지가 대문자로 표시되는지 확인합니다.

   ![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

5. http://localhost:4502/content/wknd-spa-react/us/en.model.json으로 이동하여 JSON 모델을 [봅니다](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Search for `wknd-spa-react/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   JSON 값은 Sling Model에 추가된 논리를 기반으로 모든 대문자 문자로 설정됩니다.

## 축하합니다! {#congratulations}

축하합니다. SPA 편집기에 사용할 사용자 정의 AEM 구성 요소를 만드는 방법을 알아보았습니다. 또한 대화 상자, JCR 속성 및 Sling Models가 JSON 모델을 출력하기 위해 상호 작용하는 방법을 학습했습니다.

완료된 코드를 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/custom-component-solution`있습니다.

### 다음 단계 {#next-steps}

[핵심 구성 요소](extend-component.md) 확장 - AEM SPA 편집기와 함께 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA Editor 구현 기능을 확장하는 강력한 방법입니다.
