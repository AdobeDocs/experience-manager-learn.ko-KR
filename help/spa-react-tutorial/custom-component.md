---
title: 사용자 지정 구성 요소 만들기 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.
sub-product: 사이트
feature: SPA 편집기
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1489'
ht-degree: 1%

---


# 사용자 지정 구성 요소 {#custom-component} 만들기

AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작할 때 Sling 모델의 역할을 이해합니다.
2. 새 AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. SPA 편집기 프레임워크와 호환되는 **사용자 지정** AEM 구성 요소를 만드는 방법을 알아봅니다.

## 빌드할 내용

이전 장의 초점은 SPA 구성 요소를 개발하고 *기존* AEM 코어 구성 요소에 매핑하는 것이었습니다. 이 장에서는 *새* AEM 구성 요소를 만들고 확장하고 AEM에서 제공하는 JSON 모델을 조작하는 방법에 중점을 둡니다.

간단한 `Custom Component`에서는 새 AEM 구성 요소를 만드는 데 필요한 단계를 보여줍니다.

![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에 대해 완료된 패키지를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에서 제공하는 이미지가 WKND SPA에서 다시 사용됩니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 설치할 수 있습니다.

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution)에서 완료된 코드를 보거나 분기 `React/custom-component-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드 및 속성은 `ui.apps` 모듈에서 XML 파일로 표시됩니다. 그런 다음 `ui.apps` 모듈에서 AEM 구성 요소를 만듭니다.

>[!NOTE]
>
> AEM 구성 요소의 [기본 사항에 대한 빠른 재료는](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)에 도움이 될 수 있습니다.

1. 선택한 IDE에서 `ui.apps` 폴더를 엽니다.
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` 로 이동하여 `custom-component` 라는 새 폴더를 만듭니다.
3. `custom-component` 폴더 아래에 `.content.xml` 라는 새 파일을 만듭니다. `custom-component/.content.xml`을(를) 다음 값으로 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title` 는 컨텐츠 작성자에게 표시될 값이며 작성 UI의 구성 요소  `componentGroup` 그룹을 결정합니다.

4. `custom-component` 폴더 아래에서 `_cq_dialog` 라는 다른 폴더를 만듭니다.
5. `_cq_dialog` 폴더 아래에 `.content.xml` 라는 새 파일을 만들고 다음 파일로 채웁니다.

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

   위의 XML 파일은 `Custom Component`에 대한 매우 간단한 대화 상자를 생성합니다. 파일의 중요한 부분은 내부 `<message>` 노드입니다. 이 대화 상자에는 `Message` 라는 간단한 `textfield`이 포함되며 텍스트 필드의 값을 `message` 속성에 유지합니다.

   JSON 모델을 통해 `message` 속성 값을 노출하는 옆에 Sling 모델이 만들어집니다.

   >[!NOTE]
   >
   > 코어 구성 요소 정의](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)를 보면 대화 상자의 예제를 훨씬 더 많이 볼 수 있습니다. [ [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)에서 `/libs/granite/ui/components/coral/foundation/form` 아래에 있는 `select`, `textarea`, `pathfield` 등의 추가 양식 필드를 볼 수도 있습니다.

   기존 AEM 구성 요소를 사용하는 경우 일반적으로 [HTL](https://docs.adobe.com/content/help/ko-KR/experience-manager-htl/using/overview.html) 스크립트가 필요합니다. SPA이 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## Sling 모델 만들기

Sling 모델은 JCR에서 Java 변수에 데이터를 쉽게 매핑하는 주석 기반의 Java &quot;POJO&quot;(일반 이전 Java 개체)입니다. [AEM ](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) 구성 요소에 대한 복잡한 서버측 비즈니스 로직을 캡슐화하는 Sling Modelstically 함수

SPA Editor 컨텍스트에서 Sling Models는 [Sling Model Exporter](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)를 사용하는 기능을 통해 JSON 모델을 통해 구성 요소의 컨텐츠를 제공합니다.

1. 선택한 IDE에서 `core` 모듈을 엽니다. `CustomComponent.java` 그리고  `CustomComponentImpl.java` 는 이미 장 스타터 코드의 일부로 작성되고 게시되었습니다.

   >[!NOTE]
   >
   > Visual Studio 코드 IDE를 사용하는 경우 Java](https://code.visualstudio.com/docs/java/extensions)용 [확장을 설치하는 것이 도움이 될 수 있습니다.

2. `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`에서 Java 인터페이스 `CustomComponent.java`을 엽니다.

   ![CustomComponent.java 인터페이스](assets/custom-component/custom-component-interface.png)

   Sling 모델에서 구현할 Java 인터페이스입니다.

3. `CustomComponent.java`을 업데이트하여 `ComponentExporter` 인터페이스를 확장합니다.

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   `ComponentExporter` 인터페이스를 구현하는 것은 JSON 모델 API에서 Sling 모델을 자동으로 선택하도록 하는 요구 사항입니다.

   `CustomComponent` 인터페이스에는 단일 getter 메서드 `getMessage()`이 포함되어 있습니다. JSON 모델을 통해 작성 대화 상자의 값을 표시하는 메서드입니다. 빈 매개 변수 `()`이 있는 공개 getter 메서드만 JSON 모델에 내보내집니다.

4. `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`에서 `CustomComponentImpl.java` 을 엽니다.

   `CustomComponent` 인터페이스의 구현입니다. `@Model` 주석은 Java 클래스를 Sling 모델로 식별합니다. `@Exporter` 주석을 사용하면 Sling 모델 익스포터를 통해 Java 클래스를 직렬화하고 내보낼 수 있습니다.

5. 정적 변수 `RESOURCE_TYPE`을 업데이트하여 이전 연습에서 만든 AEM 구성 요소 `wknd-spa-react/components/custom-component`를 가리킵니다.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   구성 요소의 리소스 유형은 Sling 모델을 AEM 구성 요소에 바인딩하고 궁극적으로 React 구성 요소에 매핑됩니다.

6. `CustomComponentImpl` 클래스에 `getExportedType()` 메서드를 추가하여 구성 요소 리소스 유형을 반환합니다.

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   이 메서드는 `ComponentExporter` 인터페이스를 구현할 때 필요하며 React 구성 요소에 매핑을 허용하는 리소스 유형을 표시합니다.

7. 작성자 대화 상자에서 지속되는 `message` 속성 값을 반환하려면 `getMessage()` 메서드를 업데이트합니다. `@ValueMap` 주석을 사용하여 JCR 값 `message`을 Java 변수에 매핑합니다.

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

   일부 추가 &quot;비즈니스 논리&quot;가 추가되어 메시지의 문자열 값을 모든 대문자와 함께 반환합니다. 이렇게 하면 작성 대화 상자에 저장된 원시 값과 Sling 모델에 의해 노출된 값 간의 차이를 볼 수 있습니다.

   >[!NOTE]
   >
   > [완성된 CustomComponentImpl.java를 여기에서 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java).

## React 구성 요소 업데이트

사용자 지정 구성 요소에 대한 React 코드가 이미 만들어졌습니다. 다음으로, 몇 가지 업데이트를 수행하여 React 구성 요소를 AEM 구성 요소에 매핑합니다.

1. `ui.frontend` 모듈에서 `ui.frontend/src/components/Custom/Custom.js` 파일을 엽니다.
2. `render()` 메서드의 일부로 `{this.props.message}` 변수를 관찰합니다.

   ```js
   return (
           <div className="CustomComponent">
               <h2 className="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   Sling Model에서 변환된 대문자 값은 이 `message` 속성에 매핑되어야 합니다.

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

다음으로 AEM으로 이동하여 업데이트를 확인하고 `Custom Component`을 SPA에 추가할 수 있도록 허용합니다.

1. [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)로 이동하여 새 Sling 모델의 등록을 확인합니다.

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   위의 두 행이 표시되고 `CustomComponentImpl` 구성 요소가 `wknd-spa-react/components/custom-component` 구성 요소와 연결되며 Sling 모델 익스포터를 통해 등록되었음을 나타냅니다.

2. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)의 SPA 페이지 템플릿으로 이동합니다.
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Custom Component`을 허용된 구성 요소로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 `Custom Component` 을 허용된 구성 요소로 관찰합니다.

   ![허용된 구성 요소로서의 사용자 지정 구성 요소](assets/custom-component/custom-component-allowed-layout-container.png)

## 사용자 지정 구성 요소 작성

그런 다음 AEM SPA 편집기를 사용하여 `Custom Component` 을 작성합니다.

1. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)로 이동합니다.
2. `Edit` 모드에서 `Custom Component`을 `Layout Container`에 추가합니다.

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

3. 구성 요소의 대화 상자를 열고 소문자가 포함된 메시지를 입력합니다.

   ![사용자 지정 구성 요소 구성](assets/custom-component/enter-dialog-message.png)

   이 대화 상자는 장의 앞부분에 있는 XML 파일을 기반으로 작성된 것입니다.

4. 변경 사항을 저장합니다. 표시된 메시지가 대문자로 표시되는지 확인합니다.

   ![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

5. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)로 이동하여 JSON 모델을 봅니다. `wknd-spa-react/components/custom-component` 검색:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   JSON 값은 Sling 모델에 추가된 논리를 기반으로 모든 대문자로 설정됩니다.

## 축하합니다! {#congratulations}

축하합니다. SPA 편집기에 사용할 사용자 지정 AEM 구성 요소를 만드는 방법을 알아보았습니다. 또한 대화 상자, JCR 속성 및 Sling 모델이 JSON 모델을 출력하는 방법을 학습했습니다.

[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution)에서 완료된 코드를 보거나 분기 `React/custom-component-solution` 로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

### 다음 단계 {#next-steps}

[코어 구성 요소 확장](extend-component.md)  - AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다.
