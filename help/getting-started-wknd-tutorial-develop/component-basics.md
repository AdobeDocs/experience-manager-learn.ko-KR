---
title: AEM Sites 시작하기 - 구성 요소 기본 사항
description: 간단한 'HelloWorld' 예제를 통해 AEM(Adobe Experience Manager) 사이트 구성 요소의 기본 기술을 이해합니다. HTL, Sling Models, 클라이언트측 라이브러리 및 작성자 대화 상자의 항목에 대해 살펴봅니다.
sub-product: 사이트
feature: Core Components, Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
topic: Content Management, Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 0%

---


# 구성 요소 기본 사항 {#component-basics}

이 장에서는 간단한 `HelloWorld` 예제를 통해 AEM(Adobe Experience Manager) 사이트 구성 요소의 기본 기술을 살펴봅니다. 저작, HTL, Sling Models, Client-side Libraries에 대한 주제를 포함하여 기존 구성 요소에 대해 약간의 수정이 이루어집니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

비디오에 사용되는 IDE는 [Visual Studio 코드](https://code.visualstudio.com/) 및 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 플러그인입니다.

## 목표 {#objective}

1. HTML을 동적으로 렌더링하는 HTL 템플릿 및 Sling 모델의 역할을 알아봅니다.
1. 대화 상자가 컨텐츠 작성을 용이하게 하는 방법을 이해합니다.
1. 구성 요소를 지원하는 CSS 및 JavaScript를 포함하는 클라이언트측 라이브러리의 기본 사항에 대해 알아봅니다.

## {#what-you-will-build} 빌드할 항목

이 장에서는 매우 간단한 `HelloWorld` 구성 요소에 대한 몇 가지 수정 작업을 수행합니다. `HelloWorld` 구성 요소를 업데이트하는 과정에서 AEM 구성 요소 개발의 주요 영역에 대해 학습합니다.

## 장 시작 프로젝트 {#starter-project}

이 장은 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype)에서 생성된 일반 프로젝트를 기반으로 합니다. 아래 비디오를 시청하고 [사전 요구 사항](#prerequisites)을 검토하여 시작하십시오!

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

새 명령줄 터미널을 열고 다음 작업을 수행합니다.

1. 빈 디렉토리에서 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 선택적으로 이전 장 [프로젝트 설정](./project-setup.md)에서 생성된 프로젝트를 계속 사용할 수 있습니다.

1. `aem-guides-wknd` 폴더로 이동합니다.

   ```shell
   $ cd aem-guides-wknd
   ```

1. 다음 명령을 사용하여 프로젝트를 빌드하고 AEM의 로컬 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. [로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 지침에 따라 프로젝트를 기본 IDE로 가져옵니다.

## 구성 요소 제작 {#component-authoring}

구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. 구성 요소를 다시 사용하려면 구성 요소를 구성할 수 있어야 합니다. 이 작업은 작성자 대화 상자를 통해 수행됩니다. 그런 다음 간단한 구성 요소를 작성하고 대화 상자의 값이 AEM에서 유지되는 방식을 검사합니다.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

다음은 위의 비디오에서 수행되는 높은 수준의 단계입니다.

1. **WKND 사이트** `>` **** `>` **en** 아래에 **구성 요소 기초**&#x200B;라는 새 페이지를 만듭니다.
1. 새로 만든 페이지에 **Hello World 구성 요소**&#x200B;를 추가합니다.
1. 구성 요소에 대한 대화 상자를 열고 텍스트를 입력합니다. 변경 내용을 저장하여 페이지에 표시되는 메시지를 확인합니다.
1. 개발자 모드로 전환하고 CRXDE-Lite에서 컨텐츠 경로를 보고 구성 요소 인스턴스의 속성을 검사합니다.
1. CRXDE-Lite를 사용하여 `/apps/wknd/components/content/helloworld`에 있는 `cq:dialog` 및 `helloworld.html` 스크립트를 봅니다.

## HTL(HTML 템플릿 언어) 및 대화 상자 {#htl-dialogs}

HTML 템플릿 언어 또는 **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)**&#x200B;은 AEM 구성 요소가 컨텐츠를 렌더링하는 데 사용하는 경량의 서버측 템플릿 언어입니다.

**대화 상자** 는 구성 요소에 사용할 수 있는 구성을 정의합니다.

다음으로 텍스트 메시지 앞에 추가 인사말을 표시하도록 `HelloWorld` HTL 스크립트를 업데이트합니다.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

다음은 위의 비디오에서 수행되는 높은 수준의 단계입니다.

1. IDE로 전환하고 프로젝트를 `ui.apps` 모듈로 엽니다.
1. `helloworld.html` 파일을 열고 HTML 마크업을 변경합니다.
1. [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)와 같은 IDE 도구를 사용하여 파일 변경 내용을 로컬 AEM 인스턴스와 동기화합니다.
1. 브라우저로 돌아가 구성 요소 렌더링이 변경되었음을 확인합니다.
1. 다음 위치에서 `HelloWorld` 구성 요소에 대한 대화 상자를 정의하는 `.content.xml` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 대화 상자를 업데이트하여 `./title` 이름의 **제목**&#x200B;이라는 이름의 추가 텍스트 필드를 추가합니다.

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. 다음 위치에 있는 `HelloWorld` 구성 요소를 렌더링하는 기본 HTL 스크립트를 나타내는 `helloworld.html` 파일을 다시 엽니다.

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. `helloworld.html`을 업데이트하여 `H1` 태그의 일부로 **Greeting** textfield의 값을 렌더링합니다.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 개발자 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## Sling 모델 {#sling-models}

Sling Models는 JCR에서 Java 변수에 대한 데이터 매핑을 용이하게 하고 AEM 컨텍스트에서 개발 시 많은 다른 세부 사항을 제공하는 주석 기반 Java &quot;POJO&#39;(Plain Old Java Objects)입니다.

다음으로, 페이지에 게시하기 전에 JCR에 저장된 값에 일부 비즈니스 논리를 적용하기 위해 `HelloWorldModel` Sling 모델에 몇 가지 업데이트를 적용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. `HelloWorld` 구성 요소에 사용된 Sling 모델인 `HelloWorldModel.java` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 다음 가져오기 문을 추가합니다.

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. `DefaultInjectionStrategy`을(를) 사용하도록 `@Model` 주석을 업데이트합니다.

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 다음 줄을 `HelloWorldModel` 클래스에 추가하여 구성 요소의 JCR 속성 `title` 및 `text` 값을 Java 변수에 매핑합니다.

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. `HelloWorldModel` 클래스에 다음 메서드 `getTitle()`을 추가합니다. 이 메서드는 `title` 속성의 값을 반환합니다. 이 메서드는 &quot;Default Value here!&quot;의 문자열 값을 반환하는 추가 논리를 추가합니다. 속성 `title`이(가) null이거나 비어 있는 경우:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. `HelloWorldModel` 클래스에 다음 메서드 `getText()`을 추가합니다. 이 메서드는 `text` 속성의 값을 반환합니다. 이 메서드는 문자열을 모든 대문자로 변환합니다.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `core` 모듈에서 번들을 작성하고 배포합니다.

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4/6.5를 사용하는 경우 `mvn clean install -PautoInstallBundle -Pclassic` 사용

1. `HelloWorld` 모델의 새로 만든 메서드를 사용하도록 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`에 있는 `helloworld.html` 파일을 업데이트합니다.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Eclipse Developer 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## 클라이언트측 라이브러리 {#client-side-libraries}

클라이언트측 라이브러리로서 클라이언트측은 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리는 AEM의 페이지에 CSS 및 JavaScript를 포함하는 표준 방법입니다.

다음으로 클라이언트측 라이브러리의 기본 사항을 이해하기 위해 `HelloWorld` 구성 요소에 대한 일부 CSS 스타일을 포함시킬 것입니다.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

다음은 위의 비디오에서 수행되는 높은 수준의 단계입니다.

1. `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` 아래에 `clientlib-helloworld` 라는 새 폴더를 만듭니다.
1. `clientlibs` 아래에 다음과 같은 폴더 및 파일 구조를 만듭니다.

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. `helloworld.css`을(를) 다음 항목으로 채웁니다.

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. `helloworld/clientlibs/js.txt`을(를) 다음 항목으로 채웁니다.

   ```plain
   #base=js
   helloworld.js
   ```

1. 다음 속성을 포함하도록 `clientlib-helloworld/.conten.xml` 파일을 업데이트합니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. `clientlibs/clientlib-base/.content.xml` 파일을 **embed** `wknd.helloworld` 범주로 업데이트합니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. 개발자 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

   >[!NOTE]
   >
   > CSS 및 JavaScript는 성능상의 이유로 브라우저에서 자주 캐시됩니다. 클라이언트 라이브러리에 대한 변경 사항이 즉시 표시되지 않으면 하드 새로 고침을 수행하고 브라우저의 캐시를 지웁니다. 새 캐시를 보장하기 위해 익명으로 만든 윈도우를 사용하는 것이 도움이 될 수 있습니다.

## 축하합니다!{#congratulations}

축하합니다. Adobe Experience Manager에서 구성 요소 개발의 기본 사항을 익혔습니다.

### 다음 단계 {#next-steps}

다음 장 [페이지 및 템플릿](pages-templates.md)에서 Adobe Experience Manager 페이지 및 템플릿에 대해 알아봅니다. 핵심 구성 요소가 프로젝트에 프록시하는 방법을 파악하고 편집 가능한 템플릿의 고급 정책 구성을 학습하여 잘 구성된 아티클 페이지 템플릿을 빌드합니다.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git 분기 `tutorial/component-basics-solution`에서 로컬로 코드를 검토하고 배포합니다.

