---
title: AEM Sites 시작하기 - 구성 요소 기본 사항
description: 간단한 'HelloWorld' 예제를 통해 Adobe Experience Manager(AEM) 사이트 구성 요소의 기본 기술을 이해합니다. HTL, Sling 모델, 클라이언트측 라이브러리 및 작성자 대화 상자의 주제를 살펴봅니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1715
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1192'
ht-degree: 100%

---

# 구성 요소 기본 사항 {#component-basics}

이 챕터에서는 간단한 `HelloWorld` 예제를 통해 Adobe Experience Manager(AEM) 사이트 구성 요소의 기본 기술을 살펴보겠습니다. 기존 구성 요소에 작성, HTL, Sling 모델, 클라이언트측 라이브러리 등의 주제를 다루며 사소한 수정 사항을 적용합니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](./overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다.

비디오에서 사용된 IDE는 [Visual Studio Code](https://code.visualstudio.com/) 및 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 플러그인입니다.

## 목표 {#objective}

1. HTML을 동적으로 렌더링할 때 HTL 템플릿과 Sling 모델이 담당하는 역할을 알아봅니다.
1. 대화 상자가 콘텐츠 작성을 용이하게 하는 데 어떻게 사용되는지 알아봅니다.
1. CSS와 JavaScript를 포함하여 구성 요소를 지원하는 클라이언트측 라이브러리의 기본 사항을 알아봅니다.

## 빌드 결과물 {#what-build}

이 챕터에서는 간단한 `HelloWorld` 구성 요소에 여러 가지 수정 사항을 적용합니다. `HelloWorld` 구성 요소를 업데이트하는 동안 AEM 구성 요소 개발의 주요 영역을 알아봅니다.

## 챕터 스타터 프로젝트 {#starter-project}

이 챕터는 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)에 의해 생성된 일반 프로젝트를 기반으로 합니다. 아래의 비디오를 시청하고 시작을 위한 [필수 조건](#prerequisites)을 검토하시기 바랍니다.

>[!NOTE]
>
> 이전 챕터를 성공적으로 완료했다면 프로젝트를 재사용하고 스타터 프로젝트를 체크아웃하는 단계를 건너뛸 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

새 명령줄 터미널을 열고 다음 작업을 수행합니다.

1. 빈 디렉터리에서 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 또는 이전 챕터에서 생성된 프로젝트인 [프로젝트설정](./project-setup.md)을 계속 사용할 수 있습니다.

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. [로컬 개발 환경](overview.md#local-dev-environment)을 설정하기 위한 지침에 따라 프로젝트를 원하는 IDE로 가져옵니다.

## 구성 요소 작성 {#component-authoring}

구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. 구성 요소를 재사용하려면 구성 요소가 구성 가능해야 합니다. 이는 작성자 대화 상자를 통해 수행됩니다. 다음으로 간단한 구성 요소를 작성하고 대화 상자의 값이 AEM에 어떻게 저장되는지 살펴보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

위의 비디오에서 수행된 주요 단계는 아래와 같습니다.

1. **WKND** `>` **US** `>` **en** 아래에서 **구성 요소 기본 사항**&#x200B;이라는 페이지를 생성합니다.
1. 새로 만든 페이지에 **Hello World 구성 요소**&#x200B;를 추가합니다.
1. 구성 요소의 대화 상자를 열고 텍스트를 입력합니다. 변경 사항을 저장하면 페이지에 표시되는 메시지를 확인할 수 있습니다.
1. 개발자 모드로 전환하여 CRXDE-Lite에서 콘텐츠 경로를 보고 구성 요소 인스턴스의 속성을 검사합니다.
1. CRXDE-Lite를 사용하여 `/apps/wknd/components/content/helloworld`의 `cq:dialog` 및 `helloworld.html` 스크립트를 확인합니다.

## HTL(HTML 템플릿 언어) 및 대화 상자 {#htl-dialogs}

HTML 템플릿 언어 또는 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=ko)**&#x200B;은 AEM 구성 요소에서 콘텐츠를 렌더링하는 데 사용되는 가벼운 서버측 템플릿 언어입니다.

**대화 상자**&#x200B;는 구성 요소에 대해 가능한 구성을 정의합니다.

다음으로 `HelloWorld` HTL 스크립트를 업데이트하여 텍스트 메시지 앞에 추가 인사말을 표시해 보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

위의 비디오에서 수행된 주요 단계는 아래와 같습니다.

1. IDE로 전환하고 프로젝트를 `ui.apps` 모듈로 엽니다.
1. `helloworld.html` 파일을 열고 HTML 마크업을 업데이트합니다.
1. [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)와 같은 IDE 도구를 사용하여 파일 변경 사항을 로컬 AEM 인스턴스와 동기화합니다.
1. 브라우저로 돌아가서 구성 요소 렌더링이 변경되었는지 확인합니다.
1. 다음 위치에서 `HelloWorld` 구성 요소에 대한 대화 상자를 정의하는 `.content.xml` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 속성 이름이 `./title`인 **제목**&#x200B;이라는 추가 텍스트 필드를 추가하도록 대화 상자를 업데이트합니다.

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

1. 아래 경로에서 `HelloWorld` 구성 요소 렌더링을 담당하는 주요 HTL 스크립트를 나타내는 `helloworld.html` 파일을 다시 엽니다.

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. **인사말** 텍스트 필드의 값을 `H1` 태그의 일부로 렌더링하도록 `helloworld.html`을 업데이트합니다.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 개발자 플러그인이나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## Sling 모델 {#sling-models}

Sling 모델은 JCR에서 Java™ 변수로 데이터를 매핑하는 것을 용이하게 하는 주석 기반 Java™ &#39;POJO&#39;(Plain Old Java™ 오브젝트)입니다. 이 모델은 AEM 컨텍스트에서 개발할 때 몇 가지 다른 편리한 기능도 제공합니다.

다음으로 JCR에 저장된 값을 페이지에 출력하기 전에 비즈니스 로직을 적용하기 위해 `HelloWorldModel` Sling 모델을 업데이트해 보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. `HelloWorldModel.java` 구성 요소와 함께 사용되는 Sling 모델인 `HelloWorld` 파일을 엽니다.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 다음의 Import 문을 추가합니다.

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. `DefaultInjectionStrategy`를 사용하도록 `@Model` 주석을 업데이트합니다.

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 다음 줄을 `HelloWorldModel` 클래스에 추가하여 구성 요소의 JCR 속성인 `title` 및 `text`의 값을 Java™ 변수에 매핑합니다.

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

1. `getTitle()` 메서드를 `title`이라는 속성의 값을 반환하는 `HelloWorldModel` 클래스에 추가합니다. 이 메서드는 &#39;여기에 기본값!&#39;이라는 문자열 값을 반환하기 위한 추가 논리를 추가합니다. `title` 속성이 null이거나 blank인 경우:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. `getText()` 메서드를 `text`이라는 속성의 값을 반환하는 `HelloWorldModel` 클래스에 추가합니다. 이 메서드는 문자열을 모두 대문자로 변환합니다.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `core` 모듈에서 번들을 빌드하고 배포합니다.

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4/6.5의 경우 `mvn clean install -PautoInstallBundle -Pclassic`을 사용합니다.

1. `HelloWorld` 모델의 새로 만든 메서드를 사용하도록 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`에서 `helloworld.html` 파일을 업데이트합니다.

   `HelloWorld` 모델은 HTL 지침 `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`을 통해 이 구성 요소 인스턴스에 대해 인스턴스화되어 인스턴스를 `model` 변수에 저장합니다.

   이제 `HelloWorld` 모델 인스턴스는 HTL에서 `HelloWord`를 사용하여 `model` 변수를 통해 사용할 수 있습니다. 이러한 메서드 호출은 `${model.getTitle()}`이 `${model.title}`로 축약된 경우와 같이 축약된 메서드 구문을 사용할 수 있습니다.

   마찬가지로, 모든 HTL 스크립트는 Sling 모델 오브젝트와 동일한 구문을 사용하여 액세스할 수 있는 [전역 오브젝트](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=ko)로 주입됩니다.

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
   </div>
   ```

1. Eclipse 개발자 플러그인이나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## 클라이언트측 라이브러리 {#client-side-libraries}

클라이언트측 라이브러리인 `clientlibs`는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리는 AEM 페이지에 CSS와 JavaScript를 포함하는 표준적인 방법입니다.

[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko) 모듈은 빌드 프로세스에 통합되어 있으며 결합이 해제된 [Webpack](https://webpack.js.org/) 프로젝트입니다. 이를 통해 Sass, LESS, TypeScript와 같은 인기 프론트엔드 라이브러리를 사용할 수 있습니다. `ui.frontend` 모듈은 [클라이언트측 라이브러리 챕터](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)에서 더 자세히 살펴보겠습니다.

다음으로 `HelloWorld` 구성 요소에 대한 CSS 스타일을 업데이트합니다.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

위의 비디오에서 수행된 주요 단계는 아래와 같습니다.

1. 터미널 창을 열고 `ui.frontend` 디렉터리로 이동합니다.

1. `ui.frontend` 디렉터리에서 `npm install npm-run-all --save-dev` 명령을 실행하여 [npm-run-all](https://www.npmjs.com/package/npm-run-all) 노드 모듈을 설치합니다. 이 단계는 **Archetype 39에서 생성된 AEM 프로젝트에 필요**&#x200B;하며 향후 Archetype 버전에서는 필수가 아닙니다.

1. 다음으로 `npm run watch` 명령을 실행합니다.

   ```shell
   $ npm run watch
   ```

1. IDE로 전환하고 프로젝트를 `ui.frontend` 모듈로 엽니다.
1. `ui.frontend/src/main/webpack/components/_helloworld.scss` 파일을 엽니다.
1. 파일을 업데이트하여 빨간색 제목을 표시합니다.

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 터미널에서 `ui.frontend` 모듈이 AEM의 로컬 인스턴스로 변경 사항을 컴파일 및 동기화하고 있음을 의미하는 활동을 확인할 수 있습니다.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. 브라우저로 돌아와서 제목 색상이 변경되었는지 확인합니다.

   ![구성 요소 기본 사항 업데이트](assets/component-basics/color-update.png)

## 축하합니다! {#congratulations}

축하합니다! Adobe Experience Manager에서 구성 요소 개발의 기본을 학습했습니다.

### 다음 단계 {#next-steps}

다음 챕터인 [페이지 및 템플릿](pages-templates.md)에서 Adobe Experience Manager 페이지 및 템플릿을 알아봅니다. 핵심 구성 요소가 어떻게 프로젝트에 대한 프록시로 기능하는지 이해하고 편집 가능한 템플릿의 고급 정책 구성을 학습하여 잘 구성된 문서 페이지 템플릿을 빌드합니다.

완성된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 로컬로 `tutorial/component-basics-solution` Git 분기에서 코드를 검토 및 배포할 수 있습니다.
