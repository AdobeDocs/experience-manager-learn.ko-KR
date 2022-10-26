---
title: 사용자 지정 구성 요소 만들기 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 2%

---

# 사용자 지정 구성 요소 만들기 {#custom-component}

AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작할 때 Sling 모델의 역할을 이해합니다.
2. AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. 만들기 알아보기 **사용자 지정** SPA 편집기 프레임워크와 호환되는 AEM 구성 요소입니다.

## 빌드할 내용

이전 장의 초점은 SPA 구성 요소를 개발하여 *기존* AEM 코어 구성 요소. 이 장에서는 생성 및 확장 방법에 중점을 둡니다 *새* AEM 구성 요소를 구성하고 AEM에서 제공하는 JSON 모델을 조작합니다.

간단한 `Custom Component` 는 net-new AEM 구성 요소를 만드는 데 필요한 단계를 보여줍니다.

![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 패키지에 대해 완료된 패키지 설치 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest). 에서 제공하는 이미지 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 는 WKND SPA에서 재사용됩니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/custom-component-solution`.

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드 및 속성은 `ui.apps` 모듈. 다음으로, AEM 구성 요소를 `ui.apps` 모듈.

>[!NOTE]
>
> 에 대한 빠른 재교육 [AEM 구성 요소의 기본 사항이 도움이 될 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 를 엽니다. `ui.apps` 원하는 IDE에 있는 폴더를 선택합니다.
2. 다음으로 이동 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 폴더를 만들고 `custom-component`.
3. 이름이 인 파일 만들기 `.content.xml` 아래 `custom-component` 폴더를 입력합니다. 을(를) 채우기 `custom-component/.content.xml` 사용:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title` 컨텐츠 작성자와 `componentGroup` 작성 UI에서 구성 요소 그룹을 결정합니다.

4. 아래 `custom-component` 폴더, 이름이 지정된 다른 폴더 만들기 `_cq_dialog`.
5. 아래 `_cq_dialog` 폴더 이름이 인 파일 만들기 `.content.xml` 그리고 다음과 같이 채웁니다.

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

   위의 XML 파일은 `Custom Component`. 파일의 중요한 부분은 내부입니다 `<message>` 노드 아래에 있어야 합니다. 이 대화 상자에는 다음과 같은 간단한 `textfield` 명명된 이름 `Message` textfield의 값을 `message`.

   Sling 모델 은 의 값을 표시하는 옆에 만들어집니다 `message` JSON 모델을 통해 속성 을 전송하는 것이 좋습니다.

   >[!NOTE]
   >
   > 훨씬 더 많이 볼 수 있습니다 [핵심 구성 요소 정의를 보는 대화 상자 예](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 다음과 같은 추가 양식 필드를 볼 수도 있습니다 `select`, `textarea`, `pathfield`, 아래에 있음 `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   기존 AEM 구성 요소 사용, [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 스크립트는 일반적으로 필요합니다. SPA에서 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## Sling 모델 만들기

Sling 모델은 JCR에서 Java™ 변수로 데이터를 쉽게 매핑하는 주석 기반의 Java™ &quot;POJO&quot;(일반 이전 Java™ 개체)입니다. [Sling 모델](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 일반적으로 AEM 구성 요소에 대한 복잡한 서버측 비즈니스 로직을 캡슐화하는 데 사용됩니다.

SPA 편집기의 컨텍스트에서 Sling 모델은 를 사용하여 기능을 통해 JSON 모델을 통해 구성 요소의 콘텐츠를 노출합니다 [Sling 모델 내보내기](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=ko-KR).

1. 선택한 IDE에서 `core` 모듈. `CustomComponent.java` 및 `CustomComponentImpl.java` 는 이미 장 스타터 코드의 일부로 작성되고 게시되었습니다.

   >[!NOTE]
   >
   > Visual Studio 코드 IDE를 사용하는 경우 설치하는 것이 도움이 될 수 있습니다 [Java™용 확장](https://code.visualstudio.com/docs/java/extensions).

2. Java™ 인터페이스를 엽니다. `CustomComponent.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java 인터페이스](assets/custom-component/custom-component-interface.png)

   Sling 모델에 의해 구현되는 Java™ 인터페이스입니다.

3. 업데이트 `CustomComponent.java` 이렇게 하면 `ComponentExporter` 인터페이스:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   구현 `ComponentExporter` 인터페이스는 JSON 모델 API에서 Sling 모델을 자동으로 선택하도록 요구 사항입니다.

   다음 `CustomComponent` 인터페이스에는 단일 게터 방법이 포함되어 있습니다 `getMessage()`. JSON 모델을 통해 작성자 대화 상자의 값을 표시하는 메서드입니다. 빈 매개 변수가 있는 getter 메서드만 `()` 를 JSON 모델로 내보냅니다.

4. 열기 `CustomComponentImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   구현은 다음과 같습니다 `CustomComponent` 인터페이스. 다음 `@Model` 주석은 Java™ 클래스를 Sling 모델로 식별합니다. 다음 `@Exporter` 주석을 사용하면 Java™ 클래스를 직렬화하고 Sling Model Exporter를 통해 내보낼 수 있습니다.

5. 정적 변수 업데이트 `RESOURCE_TYPE` AEM 구성 요소를 가리키려면 `wknd-spa-angular/components/custom-component` 이전 연습에서 만들어졌습니다.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   구성 요소의 리소스 유형은 Sling 모델을 AEM 구성 요소에 바인딩하고 궁극적으로 Angular 구성 요소에 매핑하는 것입니다.

6. 추가 `getExportedType()` 메서드를 `CustomComponentImpl` 구성 요소 리소스 유형을 반환하는 클래스:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   이 메서드는 를 구현할 때 필요합니다 `ComponentExporter` 인터페이스 및 는 Angular 구성 요소에 매핑할 수 있는 리소스 유형을 표시합니다.

7. 업데이트 `getMessage()` 메서드 `message` 작성 대화 상자에 의해 지속되는 속성입니다. 를 사용하십시오 `@ValueMap` 주석을 JCR 값에 매핑합니다. `message` 를 Java™ 변수로 추가합니다.

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

   메시지 값을 대문자로 반환하기 위해 일부 추가 &quot;비즈니스 논리&quot;가 추가됩니다. 이렇게 하면 작성 대화 상자에 저장된 원시 값과 Sling 모델에 의해 노출된 값 간의 차이를 볼 수 있습니다.

   >[!NOTE]
   을 볼 수 있습니다 [finished CustomComponentImpl.java here](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## angular 구성 요소 업데이트

사용자 지정 구성 요소에 대한 Angular 코드가 이미 생성되었습니다. 다음으로, 몇 가지 업데이트를 수행하여 Angular 구성 요소를 AEM 구성 요소에 매핑합니다.

1. 에서 `ui.frontend` 모듈이 파일을 엽니다. `ui.frontend/src/app/components/custom/custom.component.ts`
2. 관찰하십시오 `@Input() message: string;` 줄. 변환된 대문자 값은 이 변수에 매핑되어야 합니다.
3. 가져오기 `MapTo` AEM SPA Editor JS SDK의 개체를 사용하여 AEM 구성 요소에 매핑합니다.

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 열기 `cutom.component.html` 그리고 다음 값이 `{{message}}` 이 `<h2>` 태그에 가깝게 포함했습니다.
5. 열기 `custom.component.css` 및 다음 규칙을 추가합니다.

   ```css
   :host-context {
       display: block;
   }
   ```

   구성 요소가 비어 있을 때 AEM 편집기 자리 표시자가 제대로 표시되려면 `:host-context` 또는 `<div>` 를 로 설정해야 합니다. `display: block;`.

6. Maven 기술을 사용하여 프로젝트 디렉토리의 루트에서 로컬 AEM 환경에 업데이트를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 템플릿 정책 업데이트

다음으로 AEM으로 이동하여 업데이트를 확인하고 다음을 허용합니다 `Custom Component` SPA에 추가할 수 없습니다.

1. 로 이동하여 새 Sling 모델의 등록을 확인합니다. [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   위의 두 행이 표시되고 `CustomComponentImpl` 은 `wknd-spa-angular/components/custom-component` Sling Model Exporter를 통해 등록되고 구성 요소를 생성하지 않습니다.

2. 의 SPA 페이지 템플릿으로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Custom Component` 허용된 구성 요소로:

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 `Custom Component` 허용된 구성 요소로:

   ![허용된 구성 요소로서의 사용자 지정 구성 요소](assets/custom-component/custom-component-allowed-layout-container.png)

## 사용자 지정 구성 요소 작성

다음으로, `Custom Component` AEM SPA 편집기 사용.

1. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. in `Edit` 모드, 추가 `Custom Component` 변환 후 `Layout Container`:

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

3. 구성 요소의 대화 상자를 열고 소문자가 포함된 메시지를 입력합니다.

   ![사용자 지정 구성 요소 구성](assets/custom-component/enter-dialog-message.png)

   이 대화 상자는 장의 앞부분에 있는 XML 파일을 기반으로 작성된 것입니다.

4. 변경 사항을 저장합니다. 표시된 메시지가 대문자로 표시되는지 확인합니다.

   ![모두 대문자로 표시된 메시지](assets/custom-component/message-displayed.png)

5. 로 이동하여 JSON 모델 보기 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 검색 대상 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   JSON 값은 Sling 모델에 추가된 논리를 기반으로 모든 대문자로 설정됩니다.

## 축하합니다! {#congratulations}

축하합니다. 사용자 지정 AEM 구성 요소를 만드는 방법과 Sling 모델 및 대화 상자가 JSON 모델로 작동하는 방법을 알아보았습니다.

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/custom-component-solution`.

### 다음 단계 {#next-steps}

[코어 구성 요소 확장](extend-component.md) - AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다.
