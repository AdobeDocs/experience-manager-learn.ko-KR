---
title: 스타일 시스템을 사용하여 개발
description: Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법에 대해 알아봅니다. 이 튜토리얼에서는 스타일 시스템에서 템플릿 편집기의 브랜드별 CSS 및 고급 정책 구성을 사용하여 핵심 구성 요소를 확장하는 개발 방법을 다룹니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# 스타일 시스템을 사용하여 개발 {#developing-with-the-style-system}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법에 대해 알아봅니다. 이 튜토리얼에서는 스타일 시스템에서 템플릿 편집기의 브랜드별 CSS 및 고급 정책 구성을 사용하여 핵심 구성 요소를 확장하는 개발 방법을 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

또한 [클라이언트측 라이브러리 및 프론트엔드 워크플로](client-side-libraries.md) 튜토리얼을 검토하여 클라이언트측 라이브러리와 AEM 프로젝트에 내장된 다양한 프론트엔드 도구의 기본 사항을 이해하는 것이 좋습니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/style-system-start` 분기 확인

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)에서 완성된 코드를 항상 보거나 `tutorial/style-system-solution` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.

## 목표

1. 스타일 시스템을 사용하여 브랜드별 CSS를 AEM 핵심 구성 요소에 적용하는 방법을 이해합니다.
1. BEM 표기법과 이를 사용하여 스타일의 범위를 신중하게 지정하는 방법에 대해 알아봅니다.
1. 편집 가능한 템플릿을 사용하여 고급 정책 구성을 적용합니다.

## 빌드할 항목 {#what-build}

이 장에서는 [스타일 시스템 기능](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)을 사용하여 문서 페이지에서 사용되는 **제목** 및 **텍스트** 구성 요소의 변형을 만듭니다.

![Title에 사용할 수 있는 스타일](assets/style-system/styles-added-title.png)

*제목 구성 요소에 사용할 수 있는 밑줄 스타일*

## 배경 {#background}

[스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html)을 사용하면 개발자와 템플릿 편집기에서 구성 요소의 여러 가지 시각적 변형을 만들 수 있습니다. 그런 다음 작성자는 페이지를 작성할 때 사용할 스타일을 결정할 수 있습니다. 스타일 시스템은 자습서의 나머지 부분에서 낮은 코드 접근 방식으로 핵심 구성 요소를 사용하여 여러 가지 고유한 스타일을 수행하는 데 사용됩니다.

스타일 시스템의 일반적인 개념은 작성자가 구성 요소가 어떻게 표시되어야 하는지에 대한 다양한 스타일을 선택할 수 있다는 것입니다. &quot;스타일&quot;은 구성 요소의 외부 div에 삽입되는 추가 CSS 클래스에서 지원됩니다. 이러한 스타일 클래스를 기반으로 클라이언트 라이브러리에서 CSS 규칙이 추가되어 구성 요소의 모양이 변경됩니다.

여기에서 [스타일 시스템에 대한 자세한 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=ko)를 찾을 수 있습니다. 또한 스타일 시스템을 이해하는 데 유용한 [기술 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)가 있습니다.

## 밑줄 스타일 - 제목 {#underline-style}

[제목 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html)이(가) **ui.apps** 모듈의 일부로 `/apps/wknd/components/title`의 프로젝트에 프록시되었습니다. 제목 요소(`H1`, `H2`, `H3`...)의 기본 스타일이 **ui.frontend** 모듈에서 이미 구현되었습니다.

[WKND 문서 디자인](assets/pages-templates/wknd-article-design.xd)에는 밑줄이 있는 제목 구성 요소에 대한 고유한 스타일이 포함되어 있습니다. 두 개의 구성 요소를 만들거나 구성 요소 대화 상자를 수정하는 대신 스타일 시스템 을 사용하여 작성자가 밑줄 스타일을 추가할 수 있습니다.

![스타일 밑줄 - 제목 구성 요소](assets/style-system/title-underline-style.png)

### 제목 정책 추가

콘텐츠 작성자가 특정 구성 요소에 적용할 밑줄 스타일을 선택할 수 있도록 제목 구성 요소에 대한 정책을 추가해 보겠습니다. 이 작업은 AEM 내의 템플릿 편집기 를 사용하여 수행됩니다.

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)에서 **문서 페이지** 템플릿으로 이동합니다.

1. **구조** 모드의 기본 **레이아웃 컨테이너**&#x200B;에서 *허용된 구성 요소*&#x200B;에 나열된 **제목** 구성 요소 옆에 있는 **정책** 아이콘을 선택합니다.

   ![제목 정책 구성](assets/style-system/article-template-title-policy-icon.png)

1. 다음 값으로 제목 구성 요소에 대한 정책을 만듭니다.

   *정책 제목&#42;*: **WKND 제목**

   *속성* > *스타일 탭* > *새 스타일 추가*

   **밑줄**: `cmp-title--underline`

   ![제목의 스타일 정책 구성](assets/style-system/title-style-policy.png)

   제목 정책에 대한 변경 내용을 저장하려면 **완료**&#x200B;를 클릭합니다.

   >[!NOTE]
   >
   > 값 `cmp-title--underline`은(는) 구성 요소의 HTML 마크업의 외부 div에서 CSS 클래스를 채웁니다.

### 밑줄 스타일 적용

작성자가 되면 특정 제목 구성 요소에 밑줄 스타일을 적용해 보겠습니다.

1. AEM Sites 편집기의 **La Skateparks** 문서로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. **편집** 모드에서 제목 구성 요소를 선택합니다. **페인트브러쉬** 아이콘을 클릭하고 **밑줄** 스타일을 선택합니다.

   ![밑줄 스타일 적용](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 이 시점에서는 `underline` 스타일이 구현되지 않았으므로 표시되는 변경 내용이 없습니다. 다음 연습에서는 이 스타일이 구현됩니다.

1. **페이지 정보** 아이콘 > **게시됨으로 보기**&#x200B;를 클릭하여 AEM 편집기 외부의 페이지를 검사합니다.
1. 브라우저 개발자 도구를 사용하여 제목 구성 요소 주위의 마크업에 외부 div에 적용된 CSS 클래스 `cmp-title--underline`이(가) 있는지 확인하십시오.

   밑줄 클래스가 적용된 ![Div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 밑줄 스타일 구현 - ui.frontend

그런 다음 AEM 프로젝트의 **ui.frontend** 모듈을 사용하여 밑줄 스타일을 구현합니다. AEM의 로컬 인스턴스에 배포하는 *이전* 스타일을 미리 보기 위해 **ui.frontend** 모듈과 번들로 제공되는 Webpack 개발 서버가 사용됩니다.

1. **ui.frontend** 모듈 내에서 `watch` 프로세스를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   `ui.frontend` 모듈의 변경 내용을 모니터링하고 변경 내용을 AEM 인스턴스에 동기화하는 프로세스를 시작합니다.


1. IDE를 반환하고 `ui.frontend/src/main/webpack/components/_title.scss`에서 `_title.scss` 파일을 엽니다.
1. `cmp-title--underline` 클래스를 대상으로 하는 새 규칙 도입:

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >스타일을 항상 대상 구성 요소에 밀접하게 적용하는 것이 좋습니다. 이렇게 하면 추가 스타일이 페이지의 다른 영역에 영향을 주지 않습니다.
   >
   >모든 핵심 구성 요소는 **[BEM 표기법](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**&#x200B;을 준수합니다. 구성 요소에 대한 기본 스타일을 만들 때 외부 CSS 클래스를 타깃팅하는 것이 좋습니다. 또 다른 모범 사례는 HTML 요소가 아닌 핵심 구성 요소 BEM 표기법으로 지정된 클래스 이름을 타깃팅하는 것입니다.

1. 브라우저 및 AEM 페이지로 돌아갑니다. 밑줄 스타일이 추가된 것을 볼 수 있습니다.

   ![Webpack 개발 서버에 밑줄 스타일이 표시됨](assets/style-system/underline-implemented-webpack.png)

1. 이제 AEM 편집기에서 **밑줄** 스타일을 켜거나 끄고 변경 내용이 시각적으로 반영되었는지 확인할 수 있습니다.

## 견적 블록 스타일 - 텍스트 {#text-component}

그런 다음 유사한 단계를 반복하여 [텍스트 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html)에 고유한 스타일을 적용합니다. 텍스트 구성 요소가 **ui.apps** 모듈의 일부로 `/apps/wknd/components/text`의 프로젝트에 프록시되었습니다. 단락 요소의 기본 스타일이 **ui.frontend**&#x200B;에서 이미 구현되었습니다.

[WKND 문서 디자인](assets/pages-templates/wknd-article-design.xd)에는 견적 블록이 있는 텍스트 구성 요소에 대한 고유한 스타일이 포함되어 있습니다.

![견적 블록 스타일 - 텍스트 구성 요소](assets/style-system/quote-block-style.png)

### 텍스트 정책 추가

그런 다음 텍스트 구성 요소에 대한 정책을 추가합니다.

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)에서 **문서 페이지 템플릿**(으)로 이동합니다.

1. **구조** 모드의 기본 **레이아웃 컨테이너**&#x200B;에서 *허용된 구성 요소*&#x200B;에 나열된 **텍스트** 구성 요소 옆에 있는 **정책** 아이콘을 선택합니다.

   ![텍스트 정책 구성](assets/style-system/article-template-text-policy-icon.png)

1. 텍스트 구성 요소 정책을 다음 값으로 업데이트합니다.

   *정책 제목&#42;*: **콘텐츠 텍스트**

   *플러그인* > *단락 스타일* > *단락 스타일 사용*

   *스타일 탭* > *새 스타일 추가*

   **견적 블록**: `cmp-text--quote`

   ![텍스트 구성 요소 정책](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![텍스트 구성 요소 정책 2](assets/style-system/text-policy-enable-quotestyle.png)

   **완료**&#x200B;를 클릭하여 텍스트 정책에 대한 변경 내용을 저장합니다.

### 견적 블록 스타일 적용

1. AEM Sites 편집기의 **La Skateparks** 문서로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. **편집** 모드에서 텍스트 구성 요소를 선택합니다. 견적 요소를 포함하도록 구성 요소를 편집합니다.

   ![텍스트 구성 요소 구성](assets/style-system/configure-text-component.png)

1. 텍스트 구성 요소를 선택하고 **페인트 브러시** 아이콘을 클릭한 다음 **견적 블록** 스타일을 선택하십시오.

   ![견적 블록 스타일 적용](assets/style-system/quote-block-style-applied.png)

1. 브라우저의 개발자 도구를 사용하여 마크업을 검사합니다. 클래스 이름 `cmp-text--quote`이(가) 구성 요소의 외부 div에 추가된 것을 볼 수 있습니다.

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 견적 블록 스타일 구현 - ui.frontend

다음으로 AEM 프로젝트의 **ui.frontend** 모듈을 사용하여 견적 블록 스타일을 구현해 보겠습니다.

1. 아직 실행 중이 아니면 **ui.frontend** 모듈 내에서 `watch` 프로세스를 시작하십시오.

   ```shell
   $ npm run watch
   ```

1. `ui.frontend/src/main/webpack/components/_text.scss`에서 `text.scss` 파일 업데이트:

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > 이 경우 원시 HTML 요소는 스타일에 의해 타깃팅됩니다. 이는 텍스트 구성 요소가 콘텐츠 작성자를 위한 리치 텍스트 편집기를 제공하기 때문입니다. RTE 콘텐츠에 대해 직접 스타일을 만드는 것은 신중하게 수행해야 하며 스타일의 범위를 엄격하게 지정하는 것이 더 중요합니다.

1. 다시 한 번 브라우저로 돌아오면 Quote 블록 스타일이 추가되었음을 알 수 있습니다.

   ![견적 블록 스타일 표시](assets/style-system/quoteblock-implemented.png)

1. Webpack 개발 서버를 중지합니다.

## 고정 폭 - 컨테이너 (보너스) {#layout-container}

컨테이너 구성 요소는 문서 페이지 템플릿의 기본 구조를 만들고 콘텐츠 작성자가 페이지에 콘텐츠를 추가할 수 있는 놓기 영역을 제공하는 데 사용되었습니다. 컨테이너는 스타일 시스템을 사용하여 콘텐츠 작성자가 레이아웃을 디자인할 수 있는 더 많은 옵션을 제공할 수도 있습니다.

문서 페이지 템플릿의 **기본 컨테이너**&#x200B;에는 작성자 사용 가능한 컨테이너가 두 개 포함되어 있으며 이 컨테이너의 너비는 고정되어 있습니다.

![주 컨테이너](assets/style-system/main-container-article-page-template.png)

*문서 페이지 템플릿의 주 컨테이너*.

**기본 컨테이너**&#x200B;의 정책은 기본 요소를 `main`(으)로 설정합니다.

![주 컨테이너 정책](assets/style-system/main-container-policy.png)

**주 컨테이너**&#x200B;을(를) 수정하는 CSS가 `ui.frontend/src/main/webpack/site/styles/container_main.scss`의 **ui.frontend** 모듈에 설정되어 있습니다.

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

`main` HTML 요소를 타깃팅하는 대신 스타일 시스템을 사용하여 컨테이너 정책의 일부로 **고정 너비** 스타일을 만들 수 있습니다. 스타일 시스템은 사용자에게 **고정 폭**&#x200B;과(와) **유동 폭** 컨테이너 사이를 전환할 수 있는 옵션을 제공합니다.

1. **보너스 챌린지** - 이전 연습에서 배운 교훈 및 스타일 시스템을 사용하여 컨테이너 구성 요소에 대한 **고정 폭** 및 **유동 폭** 스타일을 구현합니다.

## 축하합니다! {#congratulations}

축하합니다. 문서 페이지가 거의 스타일이 지정되었으며 AEM 스타일 시스템을 사용하여 실습한 경험을 쌓았습니다.

### 다음 단계 {#next-steps}

대화 상자에서 작성된 콘텐츠를 표시하는 [사용자 지정 AEM 구성 요소](custom-component.md)를 만들고 Sling 모델 개발을 탐색하여 구성 요소의 HTL을 채우는 비즈니스 논리를 캡슐화하는 전체 단계를 알아봅니다.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git 분기 `tutorial/style-system-solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.
1. `tutorial/style-system-solution` 분기를 확인하십시오.
