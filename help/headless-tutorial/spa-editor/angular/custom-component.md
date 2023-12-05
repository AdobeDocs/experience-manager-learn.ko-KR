---
title: 사용자 지정 구성 요소 만들기 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법에 대해 알아봅니다.
feature: SPA Editor
version: Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 437
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# 사용자 지정 구성 요소 만들기 {#custom-component}

AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법에 대해 알아봅니다.

## 목표

1. AEM에서 제공하는 JSON 모델 API를 조작하는 Sling 모델의 역할을 이해합니다.
2. AEM 구성 요소 대화 상자를 만드는 방법을 이해합니다.
3. 다음을 만드는 방법 알아보기 **사용자 정의** AEM 편집기 프레임워크와 호환되는 SPA 구성 요소입니다.

## 빌드할 내용

이전 장에서는 SPA 구성 요소를 개발하고 매핑하는 데 중점을 두었습니다 *기존* AEM 핵심 구성 요소. 이 장에서는 를 만들고 확장하는 방법에 중점을 둡니다 *신규* AEM 구성 요소를 사용하고 AEM에서 제공하는 JSON 모델을 조작합니다.

단순 `Custom Component` 순-새 AEM 구성 요소를 만드는 데 필요한 단계를 설명합니다.

![메시지가 모두 대문자로 표시됨](assets/custom-component/message-displayed.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   사용 중인 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 패키지를 위한 완료된 패키지 설치 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest). 에서 제공한 이미지 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 는 WKND SPA에서 재사용됩니다. 패키지를 설치하려면 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/custom-component-solution`.

## AEM 구성 요소 정의

AEM 구성 요소는 노드 및 속성으로 정의됩니다. 프로젝트에서 이러한 노드 및 속성은 XML 파일로 표시됩니다. `ui.apps` 모듈. 그런 다음,에서 AEM 구성 요소를 만듭니다. `ui.apps` 모듈.

>[!NOTE]
>
> 에 대한 빠른 새로 고침 [AEM 구성 요소의 기본 사항이 유용할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 를 엽니다. `ui.apps` 원하는 IDE의 폴더입니다.
2. 다음으로 이동 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 폴더 만들기 `custom-component`.
3. 이름이 인 파일 만들기 `.content.xml` 아래에 `custom-component` 폴더를 삭제합니다. 채우기 `custom-component/.content.xml` 다음을 사용하여:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![사용자 지정 구성 요소 정의 만들기](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - 이 노드가 AEM 구성 요소임을 식별합니다.

   `jcr:title` 는 콘텐츠 작성자에게 표시되는 값이며 `componentGroup` 제작 UI에서 구성 요소의 그룹화를 결정합니다.

4. 아래 `custom-component` 폴더, (이)라는 다른 폴더를 만듭니다. `_cq_dialog`.
5. 아래 `_cq_dialog` 폴더 (이)라는 이름의 파일 만들기 `.content.xml` 다음을 입력합니다.

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

   위의 XML 파일은 `Custom Component`. 파일의 중요한 부분은 내부입니다 `<message>` 노드. 이 대화 상자에는 간단한 항목이 포함되어 있습니다. `textfield` 명명된 `Message` 및 textifield의 값을 `message`.

   값을 표시하기 위해 옆에 슬링 모델 이 만들어집니다. `message` JSON 모델을 통한 속성.

   >[!NOTE]
   >
   > 훨씬 더 많은 항목을 볼 수 있습니다. [핵심 구성 요소 정의를 보는 대화 상자의 예](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 다음과 같은 추가 양식 필드를 볼 수도 있습니다 `select`, `textarea`, `pathfield`, 아래에서 사용 가능 `/libs/granite/ui/components/coral/foundation/form` 위치: [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   기존 AEM 구성 요소를 사용하여 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 일반적으로 스크립트가 필요합니다. SPA은 구성 요소를 렌더링하므로 HTL 스크립트가 필요하지 않습니다.

## Sling 모델 만들기

Sling 모델은 JCR에서 Java™ 변수로 데이터를 쉽게 매핑하는 주석 기반 Java™ &quot;POJO&quot;(일반 이전 Java™ 개체)입니다. [Sling 모델](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 일반적으로 AEM 구성 요소에 대한 복잡한 서버측 비즈니스 논리를 캡슐화하는 역할을 합니다.

SPA 편집기의 컨텍스트에서 슬링 모델은 를 사용하는 기능을 통해 JSON 모델을 통해 구성 요소의 콘텐츠를 노출합니다. [Sling 모델 내보내기](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=ko-KR).

1. 선택한 IDE에서 `core` 모듈. `CustomComponent.java` 및 `CustomComponentImpl.java` 은(는) 이미 챕터 시작 코드의 일부로 만들어지고 스텁아웃되었습니다.

   >[!NOTE]
   >
   > Visual Studio Code IDE를 사용하는 경우 설치하는 것이 도움이 될 수 있습니다 [Java™용 확장](https://code.visualstudio.com/docs/java/extensions).

2. Java™ 인터페이스 열기 `CustomComponent.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java 인터페이스](assets/custom-component/custom-component-interface.png)

   Sling 모델로 구현된 Java™ 인터페이스입니다.

3. 업데이트 `CustomComponent.java` 확장되도록 `ComponentExporter` 인터페이스:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   구현 `ComponentExporter` 인터페이스는 JSON 모델 API에서 슬링 모델을 자동으로 선택해야 합니다.

   다음 `CustomComponent` 인터페이스에 단일 getter 메서드가 포함되어 있습니다. `getMessage()`. JSON 모델을 통해 작성자 대화 상자의 값을 표시하는 메서드입니다. 매개 변수가 비어 있는 getter 메서드만 `()` 는 JSON 모델로 내보내집니다.

4. 열기 `CustomComponentImpl.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   다음은 의 구현입니다. `CustomComponent` 인터페이스. 다음 `@Model` 주석은 Java™ 클래스를 Sling 모델로 식별합니다. 다음 `@Exporter` 주석을 사용하면 Java™ 클래스를 직렬화하고 슬링 모델 익스포터를 통해 내보낼 수 있습니다.

5. 정적 변수 업데이트 `RESOURCE_TYPE` AEM 구성 요소를 가리키려면 다음을 수행하십시오 `wknd-spa-angular/components/custom-component` 이(가) 이전 연습에서 만들어졌습니다.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   구성 요소의 리소스 유형은 Sling 모델을 AEM 구성 요소에 바인딩하고 궁극적으로 Angular 구성 요소에 매핑하는 것입니다.

6. 추가 `getExportedType()` 메서드를 로 변환 `CustomComponentImpl` 구성 요소 리소스 유형을 반환하는 클래스:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   이 메서드는 를 구현할 때 필요합니다. `ComponentExporter` 인터페이스를 만들고 Angular 구성 요소에 매핑을 허용하는 리소스 유형을 표시합니다.

7. 업데이트 `getMessage()` 값을 반환하는 메서드 `message` 작성자 대화 상자에서 지속되는 속성입니다. 사용 `@ValueMap` 주석은 JCR 값을 매핑합니다. `message` Java™ 변수에 다음을 수행합니다.

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

   메시지 값을 대문자로 반환하기 위해 일부 &quot;비즈니스 논리&quot;가 추가됩니다. 이를 통해 작성자 대화 상자에 의해 저장된 원시 값과 슬링 모델에 의해 노출된 값 간의 차이를 볼 수 있습니다.

   >[!NOTE]
   >
   다음을 볼 수 있습니다. [여기서 CustomComponentImpl.java를 완료했습니다.](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## angular 구성 요소 업데이트

사용자 지정 구성 요소에 대한 Angular 코드가 이미 생성되었습니다. 그런 다음 몇 가지 업데이트를 통해 Angular 구성 요소를 AEM 구성 요소에 매핑합니다.

1. 다음에서 `ui.frontend` 모듈이 파일을 엽니다. `ui.frontend/src/app/components/custom/custom.component.ts`
2. 다음을 확인합니다. `@Input() message: string;` 줄. 변환된 대문자 값은 이 변수에 매핑되어야 합니다.
3. 가져오기 `MapTo` AEM SPA Editor JS SDK의 개체를 사용하고 이를 사용하여 AEM 구성 요소에 매핑합니다.

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 열기 `cutom.component.html` 및 의 값이 `{{message}}` 옆에 표시됨 `<h2>` 태그에 가깝게 배치하십시오.
5. 열기 `custom.component.css` 및 다음 규칙을 추가합니다.

   ```css
   :host-context {
       display: block;
   }
   ```

   구성 요소가 비어 있을 때 AEM 편집기 자리 표시자가 제대로 표시되도록 하려면 `:host-context` 또는 다른 `<div>` 을(를) (으)로 설정해야 함 `display: block;`.

6. Maven 기술을 사용하여 프로젝트 디렉토리의 루트에서 로컬 AEM 환경에 업데이트를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 템플릿 정책 업데이트

그런 다음 AEM으로 이동하여 업데이트를 확인하고 `Custom Component` SPA에 추가됩니다.

1. 로 이동하여 새 Sling 모델 등록 확인 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   위의 두 줄은 다음을 나타냅니다. `CustomComponentImpl` 이(가) 와(과) 연결되어 있습니다. `wknd-spa-angular/components/custom-component` 구성 요소와 Sling 모델 내보내기를 통해 등록되었는지 확인합니다.

2. 다음 SPA 페이지 템플릿으로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 항목 추가 `Custom Component` 허용된 구성 요소:

   ![레이아웃 컨테이너 정책 업데이트](assets/custom-component/custom-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 다음을 확인합니다. `Custom Component` 허용된 구성 요소:

   ![사용자 지정 구성 요소를 허용된 구성 요소로 사용](assets/custom-component/custom-component-allowed-layout-container.png)

## 사용자 지정 구성 요소 작성

다음으로, 를 작성합니다. `Custom Component` AEM SPA 편집기 사용.

1. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. 위치 `Edit` 모드, 추가 `Custom Component` (으)로 `Layout Container`:

   ![새 구성 요소 삽입](assets/custom-component/insert-custom-component.png)

3. 구성 요소의 대화 상자를 열고 일부 소문자가 포함된 메시지를 입력합니다.

   ![사용자 지정 구성 요소 구성](assets/custom-component/enter-dialog-message.png)

   이 대화 상자는 이 장 앞부분에서 XML 파일을 기반으로 만든 대화 상자입니다.

4. 변경 사항을 저장합니다. 표시된 메시지가 모두 대문자로 표시되는지 확인합니다.

   ![메시지가 모두 대문자로 표시됨](assets/custom-component/message-displayed.png)

5. 로 이동하여 JSON 모델 보기 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 검색 대상 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   JSON 값은 Sling 모델에 추가된 논리를 기반으로 모든 대문자로 설정됩니다.

## 축하합니다! {#congratulations}

축하합니다. 사용자 지정 AEM 구성 요소를 만드는 방법과 Sling 모델 및 대화 상자가 JSON 모델과 함께 작동하는 방법을 배웠습니다.

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/custom-component-solution`.

### 다음 단계 {#next-steps}

[핵심 구성 요소 확장](extend-component.md) - AEM SPA 편집기에서 사용할 기존 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 콘텐츠를 추가하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 확장하는 강력한 기술입니다.
