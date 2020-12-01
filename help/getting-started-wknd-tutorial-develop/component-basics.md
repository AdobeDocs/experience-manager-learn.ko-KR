---
title: AEM Sites 시작하기 - 구성 요소 기초
description: 간단한 'HelloWorld' 예제를 통해 Adobe Experience Manager(AEM) 사이트 구성 요소의 기본 기술을 이해합니다. HTL, Sling Models, Client-side libraries and author dialogs의 주제를 다룹니다.
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 1%

---


# 구성 요소 기본 사항 {#component-basics}

이 장에서는 간단한 `HelloWorld` 예제를 통해 Adobe Experience Manager(AEM) 사이트 구성 요소의 기본 기술을 살펴봅니다. 저작, HTL, Sling Models, Client-side libraries 등의 주제를 포함하여 기존 구성 요소에 대해 약간의 수정이 이루어집니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

## 목표 {#objective}

1. HTML을 동적으로 렌더링하는 HTL 템플릿 및 Sling Models의 역할에 대해 알아봅니다.
1. 대화 상자가 컨텐츠 작성을 용이하게 하는 방법을 이해합니다.
1. 구성 요소를 지원하는 CSS 및 JavaScript를 포함하는 클라이언트측 라이브러리의 기본 사항에 대해 알아봅니다.

## {#what-you-will-build} 빌드할 항목

이 장에서는 매우 간단한 `HelloWorld` 구성 요소에 대한 몇 가지 수정 작업을 수행합니다. `HelloWorld` 구성 요소를 업데이트하는 과정에서 AEM 구성 요소 개발의 주요 영역에 대해 학습합니다.

## 장 시작 프로젝트 {#starter-project}

이 장은 [AEM Project Tranype](https://github.com/adobe/aem-project-archetype)에서 생성된 일반 프로젝트를 기반으로 합니다. 아래 비디오를 시청하고 [사전 요구 사항](#prerequisites)을 검토하여 시작하십시오!

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

새 명령줄 터미널을 열고 다음 작업을 수행합니다.

1. 빈 디렉토리에서 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > 원할 경우, [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) 분기를 직접 다운로드할 수 있습니다.

1. `aem-guides-wknd` 디렉토리로 이동합니다.

   ```shell
   $ cd aem-guides-wknd
   ```

1. `component-basics/start` 분기로 전환:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. 다음 명령을 사용하여 프로젝트를 빌드하고 AEM의 로컬 인스턴스에 배포합니다.

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. [로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 지침에 따라 프로젝트를 기본 IDE로 가져옵니다.

## 구성 요소 작성 {#component-authoring}

구성 요소는 웹 페이지의 작은 모듈 방식의 빌딩 블록으로 생각할 수 있습니다. 구성 요소를 다시 사용하려면 구성 요소를 구성해야 합니다. 이 작업은 작성자 대화 상자를 통해 수행됩니다. 그런 다음 간단한 구성 요소를 작성하고 대화 상자의 값이 AEM에서 어떻게 유지되는지 검사합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

아래는 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. **WKND 사이트** `>` **US** `>` **en** 아래에 **구성 요소 기초**&#x200B;라는 새 페이지를 만듭니다.
1. 새로 만든 페이지에 **Hello World Component**&#x200B;를 추가합니다.
1. 구성 요소의 대화 상자를 열고 텍스트를 입력합니다. 변경 내용을 저장하여 페이지에 표시된 메시지를 확인합니다.
1. 개발자 모드로 전환하고 CRXDE-Lite에서 컨텐츠 경로를 보고 구성 요소 인스턴스의 속성을 검사합니다.
1. CRXDE-Lite를 사용하여 `/apps/wknd/components/content/helloworld`에 있는 `cq:dialog` 및 `helloworld.html` 스크립트를 봅니다.

## HTML 템플릿 언어(HTL) {#htl-templates}

HTML 템플릿 언어 또는 [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)은 AEM 구성 요소가 컨텐츠를 렌더링하는 데 사용하는 경량의 서버측 템플릿 언어입니다.

그런 다음 텍스트 메시지 전에 추가 인사말을 표시하도록 `HelloWorld` HTL 스크립트를 업데이트합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

아래는 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. Eclipse IDE로 전환하고 프로젝트를 `ui.apps` 모듈로 엽니다.
1. 다음 위치에서 `HelloWorld` 구성 요소의 대화 상자를 정의하는 `.content.xml` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. 대화 상자를 업데이트하여 `./greeting`의 이름으로 **Greeting**&#x200B;이라는 추가 텍스트 필드를 추가합니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. 다음 위치에 있는 `HelloWorld` 구성 요소의 렌더링을 담당하는 기본 HTL 스크립트를 나타내는 `helloworld.html` 파일을 엽니다.

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. `helloworld.html`을 업데이트하여 `H1` 태그의 일부로 **Greeting** textfield의 값을 렌더링합니다.

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Eclipse Developer 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## Sling Models {#sling-models}

Sling Models는 JCR에서 Java 변수에 대한 데이터 매핑을 용이하게 하고 AEM 컨텍스트에서 개발 시 많은 다른 특성을 제공하는 주석 기반 Java &quot;POJO&#39;(Plain Old Java Objects)입니다.

다음으로, 페이지에 출력하기 전에 JCR에 저장된 값에 일부 비즈니스 로직을 적용하기 위해 `HelloWorldModel` Sling 모델에 대한 일부 업데이트를 수행합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. `HelloWorld` 구성 요소와 함께 사용되는 슬링 모델인 `HelloWorldModel.java` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 다음 줄을 `HelloWorldModel` 클래스에 추가하여 구성 요소의 JCR 속성 `greeting` 및 `text`의 값을 Java 변수에 매핑합니다.

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. `HelloWorldModel` 클래스에 `getGreeting()` 메서드를 추가합니다. 이 메서드는 `greeting` 속성의 값을 반환합니다. 이 메서드는 `greeting` 속성이 null이거나 비어 있는 경우 &quot;Hello&quot;의 문자열 값을 반환하는 추가 논리를 추가합니다.

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. `HelloWorldModel` 클래스에 `getTextUpperCase()` 메서드를 추가합니다. 이 메서드는 `text` 속성의 값을 반환합니다. 이 메서드는 문자열을 모든 대문자 문자로 변환합니다.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `HelloWorld` 모델의 새로 만든 메서드를 사용하려면 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 파일의 `helloworld.html`을 업데이트합니다.

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Eclipse Developer 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## 클라이언트측 라이브러리 {#client-side-libraries}

Client-Side Libraries, for short는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리는 AEM의 페이지에 CSS 및 JavaScript를 포함하는 표준 방법입니다.

다음으로 클라이언트측 라이브러리의 기본 사항을 파악하기 위해 `HelloWorld` 구성 요소에 대한 일부 CSS 스타일을 포함시킬 것입니다.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

아래는 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` 아래에서 노드 유형이 `cq:ClientLibraryFolder`인 `clientlibs`이라는 새 노드를 만듭니다.
1. `clientlibs` 아래에 다음과 같은 폴더 및 파일 구조를 만듭니다.

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. `helloworld/clientlibs/css/helloworld.css`을(를) 다음 항목으로 채웁니다.

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. `helloworld/clientlibs/css.txt`을(를) 다음 항목으로 채웁니다.

   ```plain
   #base=css
   helloworld.css
   ```

1. `helloworld/clientlibs/js/helloworld.js`을(를) 다음 항목으로 채웁니다.

   ```js
   console.log("hello world!");
   ```

1. `helloworld/clientlibs/js.txt`을(를) 다음 항목으로 채웁니다.

   ```plain
   #base=js
   helloworld.js
   ```

1. 다음 두 속성을 포함하도록 `clientlibs` 노드 속성을 업데이트합니다.

   | 이름 | 유형 | 값 |
   |------|------|-------|
   | 카테고리 | 문자열 | wknd.base |
   | allowProxy | 부울 | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Eclipse Developer 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## 축하합니다!{#congratulations}

축하합니다. Adobe Experience Manager에서 구성 요소 개발의 기본을 배웠습니다!

### 다음 단계 {#next-steps}

다음 장 [페이지 및 템플릿](pages-templates.md)에서 Adobe Experience Manager 페이지 및 템플릿에 대해 알아보십시오. 핵심 구성 요소가 프로젝트에 프록시되는 방법을 파악하고 편집 가능한 템플릿의 고급 정책 구성을 학습하여 잘 구성된 아티클 페이지 템플릿을 작성합니다.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git brach `component-basics/solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.
1. `component-basics/solution` 분기를 확인합니다.
