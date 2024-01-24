---
title: AEM Sites 시작하기 - 구성 요소 기본 사항
description: 간단한 'HelloWorld' 예제를 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술을 이해합니다. HTL, Sling 모델, 클라이언트측 라이브러리 및 작성자 대화 상자의 주제를 살펴봅니다.
version: 6.5, Cloud Service
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
duration: 624
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 0%

---

# 구성 요소 기본 사항 {#component-basics}

이 장에서는 간단한 을 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술을 살펴보겠습니다 `HelloWorld` 예. 작성, HTL, 슬링 모델, 클라이언트측 라이브러리 등의 주제를 다루는 기존 구성 요소는 약간 수정됩니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](./overview.md#local-dev-environment).

비디오에 사용되는 IDE는 입니다. [Visual Studio 코드](https://code.visualstudio.com/) 및 [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 플러그인입니다.

## 목표 {#objective}

1. HTML을 동적으로 렌더링하기 위한 HTL 템플릿 및 Sling 모델의 역할에 대해 알아봅니다.
1. 콘텐츠 작성을 용이하게 하기 위해 대화 상자를 사용하는 방법을 이해합니다.
1. 구성 요소를 지원하기 위해 CSS 및 JavaScript를 포함하는 클라이언트측 라이브러리의 기본 사항에 대해 알아봅니다.

## 빌드할 항목 {#what-build}

이 장에서는 간단한 을 몇 가지 수정합니다 `HelloWorld` 구성 요소. 을(를) 업데이트하는 동안 `HelloWorld` 구성 요소를 통해 AEM 구성 요소 개발의 주요 영역에 대해 알아봅니다.

## 챕터 스타터 프로젝트 {#starter-project}

이 장은 기본적으로 생성되는 일반 프로젝트를 기반으로 합니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). 아래 비디오 시청 및 검토 [전제 조건](#prerequisites) 시작합니다!

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

새 명령줄 터미널을 열고 다음 작업을 수행합니다.

1. 빈 디렉토리에서 를 복제합니다. [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 필요한 경우 이전 장에서 생성된 프로젝트를 계속 사용할 수 있습니다. [프로젝트 설정](./project-setup.md).

1. 다음으로 이동  `aem-guides-wknd` 폴더를 삭제합니다.

   ```shell
   $ cd aem-guides-wknd
   ```

1. 다음 명령을 사용하여 AEM의 로컬 인스턴스에 프로젝트를 빌드하고 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 모든 Maven 명령에 대한 프로필

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 의 설정 지침에 따라 프로젝트를 원하는 IDE로 가져옵니다. [로컬 개발 환경](overview.md#local-dev-environment).

## 구성 요소 작성 {#component-authoring}

구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. 구성 요소를 재사용하려면 구성 요소를 구성할 수 있어야 합니다. 이 작업은 작성자 대화 상자를 통해 수행됩니다. 다음으로 간단한 구성 요소를 작성하고 대화 상자의 값이 AEM에서 어떻게 유지되는지 검사해 보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

다음은 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. 다음 이름의 페이지 만들기 **구성 요소 기본 사항** 아래에 **WKND 사이트** `>` **US** `>` **en**.
1. 추가 **Hello World 구성 요소** 을 클릭하여 새로 만든 페이지로 이동합니다.
1. 구성 요소의 대화 상자를 열고 텍스트를 입력합니다. 변경 사항을 저장하여 페이지에 표시되는 메시지를 확인합니다.
1. 개발자 모드로 전환하고 CRXDE-Lite에서 컨텐츠 경로를 보고 구성 요소 인스턴스의 속성을 검사합니다.
1. CRXDE-Lite를 사용하여 `cq:dialog` 및 `helloworld.html` 스크립트 출처 `/apps/wknd/components/content/helloworld`.

## HTL(HTML 템플릿 언어) 및 대화 상자 {#htl-dialogs}

HTML 템플릿 언어 또는 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** 는 AEM 구성 요소에서 콘텐츠를 렌더링하는 데 사용하는 가벼운 서버측 템플릿 언어입니다.

**대화 상자** 구성 요소에 사용할 수 있는 구성을 정의합니다.

다음으로, 다음을 업데이트하겠습니다. `HelloWorld` 텍스트 메시지 앞에 추가 인사말을 표시하는 HTL 스크립트입니다.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

다음은 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. IDE로 전환하고 프로젝트를 `ui.apps` 모듈.
1. 를 엽니다. `helloworld.html` HTML 마크업을 파일링하고 업데이트합니다.
1. 다음과 같은 IDE 도구를 사용합니다. [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 로컬 AEM 인스턴스와 파일 변경 내용을 동기화합니다.
1. 브라우저로 돌아가서 구성 요소 렌더링이 변경되었는지 확인합니다.
1. 를 엽니다. `.content.xml` 대화 상자를 정의하는 파일 `HelloWorld` 구성 요소 위치:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 대화 상자를 업데이트하여 이름이 인 추가 텍스트 필드 추가 **제목** 의 이름으로 `./title`:

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

1. 파일 다시 열기 `helloworld.html`: 렌더링을 담당하는 기본 HTL 스크립트를 나타냅니다. `HelloWorld` 아래 경로의 구성 요소:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 업데이트 `helloworld.html` 의 값을 렌더링하려면 **인사말** 의 일부로 텍스트 필드 `H1` 태그:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 개발자 플러그인을 사용하거나 Maven 기술을 사용하여 AEM의 로컬 인스턴스에 변경 사항을 배포합니다.

## Sling 모델 {#sling-models}

Sling 모델은 JCR에서 Java™ 변수로 데이터를 쉽게 매핑하는 주석 기반 Java™ &quot;POJO&quot;(일반 이전 Java™ 개체)입니다. 또한 AEM의 컨텍스트에서 개발할 때 몇 가지 다른 좋은 점을 제공합니다.

다음으로, 다음을 몇 가지 업데이트하겠습니다. `HelloWorldModel` 페이지에 출력하기 전에 JCR에 저장된 값에 일부 비즈니스 논리를 적용하기 위한 Sling 모델 을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. 파일 열기 `HelloWorldModel.java`: 와 함께 사용되는 슬링 모델입니다 `HelloWorld` 구성 요소.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 다음 가져오기 구문을 추가합니다.

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 업데이트 `@Model` 사용할 주석 `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 에 다음 줄을 추가합니다. `HelloWorldModel` 구성 요소의 JCR 속성 값을 매핑할 클래스 `title` 및 `text` Java™ 변수로 다음을 수행합니다.

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

1. 다음 메서드 추가 `getTitle()` (으)로 `HelloWorldModel` 클래스 형식을 반환하며, 이 클래스는 이름이 `title`. 이 메서드는 추가 논리를 추가하여 &quot;Default Value here!&quot;라는 문자열 값을 반환합니다. 속성인 경우 `title` 은(는) null이거나 비어 있습니다.

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 다음 메서드 추가 `getText()` (으)로 `HelloWorldModel` 클래스 형식을 반환하며, 이 클래스는 이름이 `text`. 이 메서드는 String을 모든 대문자로 변환합니다.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 에서 번들 빌드 및 배포 `core` 모듈:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4/6.5의 경우 `mvn clean install -PautoInstallBundle -Pclassic`

1. 파일 업데이트 `helloworld.html` 위치: `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 의 새로 만든 메서드를 사용하려면 `HelloWorld` 모델.

   다음 `HelloWorld` 모델은 HTL 지시문을 통해 이 구성 요소 인스턴스에 대해 인스턴스화됩니다. `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`, 인스턴스를 변수에 저장 `model`.

   다음 `HelloWorld` 이제 다음을 통해 HTL에서 모델 인스턴스를 사용할 수 있습니다. `model` 변수를 사용하는 중 `HelloWord`. 이러한 메서드 호출에서는 다음과 같은 단축된 메서드 구문을 사용할 수 있습니다. `${model.getTitle()}` 단락할 수 있음 `${model.title}`.

   마찬가지로 모든 HTL 스크립트에는 [전역 개체](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) Sling 모델 개체와 동일한 구문을 사용하여 액세스할 수 있습니다.

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

1. Eclipse Developer 플러그인을 사용하거나 Maven 기술을 사용하여 변경 사항을 AEM의 로컬 인스턴스에 배포합니다.

## 클라이언트측 라이브러리 {#client-side-libraries}

클라이언트측 라이브러리, `clientlibs` 요컨대, 은 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리는 AEM의 페이지에 CSS 및 JavaScript를 포함하는 표준 방법입니다.

다음 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 모듈이 연결 해제되었습니다. [webpack](https://webpack.js.org/) 빌드 프로세스에 통합되는 프로젝트입니다. 이를 통해 Sass, LESS 및 TypeScript와 같이 인기 있는 프론트엔드 라이브러리를 사용할 수 있습니다. 다음 `ui.frontend` 모듈은 다음에서 더 깊이 탐색됩니다. [클라이언트측 라이브러리 챕터](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

그런 다음 의 CSS 스타일을 업데이트합니다. `HelloWorld` 구성 요소.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

다음은 위 비디오에서 수행되는 높은 수준의 단계입니다.

1. 터미널 창을 열고 `ui.frontend` 디렉터리

1. 다음 위치에 있음 `ui.frontend` 디렉토리 실행 `npm install npm-run-all --save-dev` 설치 명령 [npm-run-all](https://www.npmjs.com/package/npm-run-all) 노드 모듈입니다. 이 단계는 **Archetype 39 생성 AEM 프로젝트에 필요**, 예정된 Archetype 버전에서는 이 작업이 필요하지 않습니다.

1. 그런 다음 를 실행합니다. `npm run watch` 명령:

   ```shell
   $ npm run watch
   ```

1. IDE로 전환하고 프로젝트를 `ui.frontend` 모듈.
1. 파일 열기 `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. 파일을 업데이트하여 빨간색 제목을 표시합니다.

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 터미널에 다음을 나타내는 활동이 표시됩니다. `ui.frontend` 모듈이 변경 사항을 컴파일하고 AEM의 로컬 인스턴스와 동기화하고 있습니다.

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

1. 브라우저로 돌아가서 제목 색상이 변경되었는지 확인합니다.

   ![구성 요소 기본 사항 업데이트](assets/component-basics/color-update.png)

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager에서 구성 요소 개발에 대한 기본 사항을 배웠습니다!

### 다음 단계 {#next-steps}

다음 장에서 Adobe Experience Manager 페이지 및 템플릿 알아보기 [페이지 및 템플릿](pages-templates.md). 핵심 구성 요소가 프로젝트로 프록시되는 방법을 이해하고 편집 가능한 템플릿의 고급 정책 구성을 학습하여 잘 구성된 문서 페이지 템플릿을 구축할 수 있습니다.

에서 완료된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 또는 Git 분기의 로컬에서 코드를 검토하고 배포합니다 `tutorial/component-basics-solution`.
