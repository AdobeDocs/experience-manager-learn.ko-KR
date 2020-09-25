---
title: AEM 구성 요소에 SPA 구성 요소 매핑 | AEM SPA 편집기 시작 및 반응
description: AEM SPA Editor JS SDK를 사용하여 React 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다. 이는 일반적인 AEM 저작과 유사합니다.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 1%

---


# AEM 구성 요소에 SPA 구성 요소 매핑 {#map-components}

AEM SPA Editor JS SDK를 사용하여 React 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다. 이는 일반적인 AEM 저작과 유사합니다.

이 장에서는 AEM JSON 모델 API에 대해 자세히 설명하고 AEM 구성 요소에 의해 노출된 JSON 컨텐츠를 React 구성 요소에 prop로 자동으로 주입하는 방법을 살펴봅니다.

## 목표

1. AEM 구성 요소를 SPA 구성 요소에 매핑하는 방법을 알아봅니다.
2. 컨테이너 구성 요소와 **컨텐츠** 구성 요소 간의 차이점을 **이해합니다** .
3. 기존 AEM 구성 요소에 매핑되는 새 React 구성 요소를 만듭니다.

## 구축 내용

이 장에서는 제공된 `Text` SPA 구성 요소가 AEM 구성 요소에 매핑되는 방식을 `Text`검사합니다. SPA에서 사용할 수 있고 AEM에서 작성할 수 있는 새로운 SPA 구성 요소가 만들어집니다. `Image` 또한 레이아웃 컨테이너 **및** 템플릿 편집기 **** 정책의 기본 기능을 사용하여 모양에서 보다 다양한 보기를 만들 수 있습니다.

![장 샘플 최종 저작](./assets/map-components/final-page.png)

## 전제 조건

필요한 도구 및 [로컬 개발 환경 설정을 위한 지침을 검토하십시오](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM [6.x를](overview.md#compatibility) 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) 완료된 코드를 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/map-components-solution`있습니다.

## 매핑 방법

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. AEM 구성 요소, 서버측 실행, JSON 모델 API의 일부로 컨텐츠 내보내기 JSON 콘텐츠는 브라우저에서 클라이언트 쪽을 실행하는 SPA에 의해 사용됩니다. SPA 구성 요소와 AEM 구성 요소 간의 1:1 매핑이 만들어집니다.

![AEM 구성 요소를 반응형 구성 요소에 매핑하는 개요](./assets/map-components/high-level-approach.png)

*AEM 구성 요소를 반응형 구성 요소에 매핑하는 개요*

## 텍스트 구성 요소 Inspect

AEM [프로젝트 원형](https://github.com/adobe/aem-project-archetype) 유형은 AEM `Text` 텍스트 구성 요소에 매핑되는 구성 요소를 [제공합니다](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/text.html). AEM에서 **컨텐츠** 를 렌더링하기 위한 컨텐츠 ** 구성 요소의 예입니다.

구성 요소의 작동 방식을 살펴보겠습니다.

### JSON 모델 Inspect

1. SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 이해하는 것이 중요합니다. 핵심 구성 요소 [라이브러리로](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) 이동하고 텍스트 구성 요소의 페이지를 확인합니다. 코어 구성 요소 라이브러리는 모든 AEM 코어 구성 요소의 예를 제공합니다.
2. 다음 **중 하나에 대해 JSON** 탭을 선택합니다.

   ![텍스트 JSON 모델](./assets/map-components/text-json.png)

   다음 세 가지 속성이 표시됩니다. `text`, `richText`and `:type`.

   `:type` 는 AEM 구성 요소의 `sling:resourceType` (또는 경로)를 나열하는 예약된 속성입니다. 의 값 `:type` 은 AEM 구성 요소를 SPA 구성 요소에 매핑하는 데 사용됩니다.

   `text` 및 `richText` 는 SPA 구성 요소에 노출되는 추가 속성입니다.

### Inspect the Text component

1. 새 터미널을 열고 프로젝트 내의 `ui.frontend` 폴더로 이동합니다. 다음 `npm install` 을 `npm start` 실행하여 **webpack-dev-server를 시작합니다**.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   이 `ui.frontend` 모듈은 현재 [모의 JSON 모델을 사용하도록 설정되어 있습니다](./integrate-spa.md#mock-json).

2. http://localhost:3000/content/wknd-spa-react/us/en/home.html에 새 브라우저 창이 열려 [있습니다.](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Webpack 개발 서버(모의 컨텐츠 포함)](./assets/map-components/initial-start.png)

3. 원하는 IDE에서 AEM Project for the WKND SPA를 엽니다. 모듈을 `ui.frontend` 확장하고 다음 `Text.js` 아래에서 파일을 엽니다 `ui.frontend/src/components/Text/Text.js`.

   ![Text.js 반응형 구성 요소 소스 코드](./assets/map-components/vscode-ide-text-js.png)

4. The first area we will inspect is the `class Text` at ~line 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` 는 표준 반응 구성 요소입니다. 구성 요소 `this.props.richText` 는 렌더링할 컨텐츠가 리치 텍스트인지 일반 텍스트인지를 판별하는 데 사용됩니다. 사용되는 실제 &quot;컨텐트&quot;는 `this.props.text` 잠재적인 XSS 공격을 방지하기 위해 리치 텍스트는 `DOMPurify` 위험한SetInnerHTML을 [](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 사용하여 컨텐츠를 렌더링하기 전에 escape됩니다. 앞서 연습 `richText` 에서 JSON 모델의 `text` 속성 및 속성을 기억하십시오.

5. 다음으로 ~line 29 `TextEditConfig` 에 대해 살펴봅니다.

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   위의 코드는 AEM 작성 환경에서 자리 표시자를 렌더링할 시기를 결정합니다. 메서드가 `isEmpty` true를 반환하면 **** 자리 표시자가 렌더링됩니다.

6. 마지막으로 ~line 62의 `MapTo` 호출을 살펴보십시오.

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 는 AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)에서 제공합니다. 경로는 AEM 구성 요소 `wknd-spa-react/components/text` `sling:resourceType` 의 경로를 나타냅니다. 이 경로는 앞서 본 JSON 모델에 `:type` 의해 노출된 경로와 일치합니다. `MapTo` JSON 모델 응답을 구문 분석하고 SPA 구성 요소에 올바른 값 `props` 을 전달해야 합니다.

   AEM 구성 요소 정의 `Text` 는 에서 찾을 수 있습니다 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. 에서 파일을 수정하여 `mock.model.json` 실험해 볼 수 있습니다 `ui.frontend/public/mock-content/mock.model.json`. At ~line 62 update the first `Text` value to use an **`H1`** and **`u`** tags:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   http://localhost:3000 [으로](http://localhost:3000) 이동하여 효과를 확인합니다.

   ![업데이트된 텍스트 모델](./assets/map-components/updated-text-model.png)

   렌더링 논리가 제대로 작동하는지 확인하려면 `richText` true **/** false **간에 속성을** 전환해 보십시오.

8. Inspect `Text.scss` 가 `ui.frontend/src/components/Text/Text.scss`있습니다

   이 장의 시작 코드 베이스에 의해 이 파일이 추가되었으며 이전 장에 추가된 [Sass](https://sass-lang.com/) 기능을 사용할 수 있습니다. 참조되는 변수를 참고하십시오 `ui.frontend/src/styles/_variables.scss`.

## 이미지 구성 요소 만들기

그런 다음 AEM `Image` 이미지 구성 요소에 매핑되는 [Responsive 구성 요소를 만듭니다](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/image.html). 구성 요소 `Image` 는 **컨텐츠** 구성 요소의 또 다른 예입니다.

### JSON의 Inspect

SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 검사하십시오.

1. 핵심 구성 요소 라이브러리의 [이미지 예로 이동합니다](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![이미지 코어 구성 요소 JSON](./assets/map-components/image-json.png)

   SPA 구성 요소 `src`를 채우는 데, `alt`및 `title` 의 속성이 `Image` 사용됩니다.

   >[!NOTE]
   >
   > 개발자가 적응형 및 레이지 로드 구성 요소를 만들 수 있도록 하는 노출(`lazyEnabled`, `widths`) 이미지 속성이 있습니다. 이 자습서에 내장된 구성 요소는 간단하며 이러한 고급 속성을 **사용하지 않습니다** .

2. IDE로 돌아가 을 `mock.model.json` 엽니다 `ui.frontend/public/mock-content/mock.model.json`. 이 구성 요소는 프로젝트의 순 새 구성 요소이므로 이미지 JSON을 &quot;모의&quot;해야 합니다.

   ~line 70에서 `image` 모델에 대한 JSON 항목을 추가(두 번째 뒤에 후행 쉼표 `,` 에 대해 잊지 `text_23828680`않음)하고 `:itemsOrder` 배열을 업데이트합니다.

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   이 프로젝트에는 웹팩-개발-서버 `/mock-content/adobestock-140634652.jpeg` 와 함께 사용할 샘플 **이미지가 포함되어 있습니다**.

   여기에서 전체 [mock.model.json을 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json).

### 이미지 구성 요소 구현

1. 그런 다음 아래 `Image` 의 새 폴더를 만듭니다 `ui.frontend/src/components`.
2. 폴더 아래에서 `Image` 새 파일 이름을 만듭니다 `Image.js`.

   ![Image.js 파일](./assets/map-components/image-js-file.png)

3. 다음 `import` 문을 추가합니다 `Image.js`.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 그런 다음 AEM에 자리 표시자 `ImageEditConfig` 를 추가하여 표시할 시기를 결정합니다.

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   속성이 설정되지 않은 경우 자리 `src` 표시자가 표시됩니다.

5. 다음으로 `Image` 클래스를 구현합니다.

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   위의 코드는 prop `<img>` , `src`및 JSON 모델에 의해 `alt``title` 전달된 내용을 기반으로 렌더링됩니다.

6. AEM 구성 요소에 `MapTo` 반응형 구성 요소를 매핑하는 코드를 추가합니다.

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   문자열은 다음 위치에 있는 AEM 구성 요소의 위치 `wknd-spa-react/components/image` `ui.apps` 에 해당합니다. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. 동일한 디렉토리 `Image.scss` 에 새 파일을 만들고 다음을 추가합니다.

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. 문 아래의 맨 위에 있는 파일에 참조를 `Image.js` `import` 추가합니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   여기에서 완료된 [Image.js를 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js).

9. 파일을 열고 새 구성 요소 `ui.frontend/src/components/import-components.js` 에 대한 참조를 `Image` 추가합니다.

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. 아직 시작하지 않은 경우 **webpack-dev-server를 시작합니다**. http://localhost:3000 [으로](http://localhost:3000) 이동하면 이미지 렌더링이 표시됩니다.

   ![이미지에 이미지 추가](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **보너스 당면 과제**:에 새 메서드를 구현하여 이미지 아래 `Image.js` 의 캡션 값 `this.props.title` 을 표시합니다.

## AEM에서 정책 업데이트

구성 `Image` 요소는 **webpack-dev-server에만 표시됩니다**. 다음으로 업데이트된 SPA를 AEM에 배포하고 템플릿 정책을 업데이트합니다.

1. 웹 **팩-개발 서버** 및 프로젝트의 루트에서 Maven 기술을 사용하여 변경 내용을 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM Start 화면에서 **도구** > 템플릿 **> WKND SPA React로** **[이동합니다](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   SPA 페이지를 **선택하고 편집합니다**.

   ![SPA 페이지 템플릿 편집](./assets/map-components/edit-spa-page-template.png)

3. [ **레이아웃 컨테이너** ]를 선택하고 정책 **아이콘을 클릭하여 정책을** 편집합니다.

   ![레이아웃 컨테이너 정책](./assets/map-components/layout-container-policy.png)

4. 허용된 **구성 요소** > **WKND SPA 반응 - 컨텐츠** > **이미지** 구성 요소 선택:

   ![이미지 구성 요소 선택됨](./assets/map-components/check-image-component.png)

   기본 **구성 요소** > 매핑 **** 추가 **아래에서** 이미지 - WKND SPA Reresponse - 컨텐츠구성 요소를 선택합니다.

   ![기본 구성 요소 설정](./assets/map-components/default-components.png)

   MIME **형식을** 입력합니다 `image/*`.

   완료를 **클릭하여** 정책 업데이트를 저장합니다.

5. 레이아웃 **컨테이너** 에서 **텍스트** 구성 요소의 **정책** 아이콘을클릭합니다.

   ![텍스트 구성 요소 정책 아이콘](./assets/map-components/edit-text-policy.png)

   WKND **SPA Text라는 새 정책을 만듭니다**. 추가 서식 **옵션을 활성화하려면 플러그인** > **서식** > 모든 상자를 선택합니다.

   ![RTE 서식 사용](assets/map-components/enable-formatting-rte.png)

   [ **플러그인** ] > [ **단락 스타일** ] > 확인란을 선택하여 **단락 스타일**&#x200B;활성화:

   ![단락 스타일 사용](./assets/map-components/text-policy-enable-paragraphstyles.png)

   완료를 **클릭하여** 정책 업데이트를 저장합니다.

6. 홈 페이지 **http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html** 으로 [이동합니다](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   또한 구성 요소를 편집하고 `Text` 전체 화면 **** 모드에서 추가 단락 스타일을 추가할 수도 있습니다.

   ![전체 화면 리치 텍스트 편집](assets/map-components/full-screen-rte.png)

7. 자산 파인더에서 이미지를 끌어 놓아도 **됩니다**.

   ![드래그 앤 드롭 이미지](./assets/map-components/drag-drop-image.gif)

8. 표준 WKND 참조 사이트 [를 위한 완성된 코드 베이스를](http://localhost:4502/assets.html/content/dam) AEM Assets [를 통해](https://github.com/adobe/aem-guides-wknd/releases/latest)직접 이미지를 추가하거나 WKND [참조 사이트에는](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND SPA에서 다시 사용할 수 있는 많은 이미지가 포함되어 있습니다. 이 패키지는 [AEM Package Manager를 사용하여 설치할 수 있습니다](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect 레이아웃 컨테이너

레이아웃 **컨테이너에** 대한 지원은 AEM SPA Editor SDK에서 자동으로 제공됩니다. 이름으로 표시된 **레이아웃**&#x200B;컨테이너는 **컨테이너** 구성 요소입니다. 컨테이너 구성 요소는 *다른* 구성 요소를 나타내며 동적으로 인스턴스화하는 JSON 구조를 받아들이는 구성 요소입니다.

레이아웃 컨테이너를 더 살펴봅시다.

1. 브라우저에서 http://localhost:4502/content/wknd-spa-react/us/en.model.json으로 [이동합니다.](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON 모델 API - 반응형 격자](./assets/map-components/responsive-grid-modeljson.png)

   레이아웃 **컨테이너** 구성 `sling:resourceType` 요소 `wcm/foundation/components/responsivegrid` 는 구성 요소 `:type` 와 같이 속성 `Text` 을 사용하여 SPA 편집기에서 인식됩니다 `Image` .

   레이아웃 모드를 사용하여 구성 요소 크기를 다시 [조정하는 것과 동일한 기능을 SPA](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 편집기에서 사용할 수 있습니다.

2. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html으로 [돌아갑니다](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 추가 **이미지** 구성 요소를 추가하고 레이아웃 **옵션을 사용하여 크기를 다시** 조정해보십시오.

   ![레이아웃 모드를 사용한 이미지 크기 다시 조정](./assets/map-components/responsive-grid-layout-change.gif)

3. JSON 모델 http://localhost:4502/content/wknd-spa-react/us/en.model.json [를](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 다시 열고 JSON의 `columnClassNames` 일부로 다음을 관찰합니다.

   ![열 클래스 이름](./assets/map-components/responsive-grid-classnames.png)

   클래스 이름 `aem-GridColumn--default--4` 은 구성 요소가 12개의 열 격자를 기준으로 4개의 열 너비여야 함을 나타냅니다. 응답형 [격자에 대한 자세한 내용은 여기를 참조하십시오](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. IDE로 돌아가고 `ui.apps` 모듈에 에 정의된 클라이언트측 라이브러리가 있습니다 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Open the file `less/grid.less`.

   이 파일은 레이아웃 컨테이너`default`에 사용되는 중단점( `tablet`및 `phone`)을 **결정합니다**. 이 파일은 프로젝트 사양에 따라 사용자 정의될 예정입니다. 현재 중단점은 `1200px` 및 으로 설정됩니다 `650px`.

5. 다음과 같은 뷰를 작성하려면 구성 요소의 응답형 기능과 업데이트된 리치 텍스트 정책을 사용할 수 있어야 합니다. `Text`

   ![장 샘플 최종 저작](./assets/map-components/final-page.png)

## 축하합니다! {#congratulations}

축하합니다. SPA 구성 요소를 AEM Components에 매핑하는 방법을 배웠고 새 구성 요소를 구현했습니다 `Image` . 또한 **레이아웃 컨테이너의 응답형 기능을 살펴볼 수 있습니다**.

항상 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) 완료된 코드를 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/map-components-solution`있습니다.

### 다음 단계 {#next-steps}

[탐색 및 라우팅](navigation-routing.md) - SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 뷰를 지원하는 방법을 살펴볼 수 있습니다. 동적 탐색은 반응 라우터를 사용하여 구현되며 기존 헤더 구성 요소에 추가됩니다.

## 보너스 - 소스 제어에 구성 유지 {#bonus}

대부분의 경우, 특히 AEM 프로젝트를 시작할 때는 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 따라서 모든 개발자는 동일한 컨텐츠 및 구성 세트에 대해 작업할 수 있고 환경 간에 추가적인 일관성을 유지할 수 있습니다. 프로젝트가 특정 성숙도에 도달하면 템플릿 관리 방식을 고급 사용자 그룹으로 전환할 수 있습니다.

다음 몇 단계는 Visual Studio 코드 IDE와 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 를 사용하여 발생하지만 AEM의 로컬 인스턴스에서 컨텐츠를 **가져오거나** 가져오도록 **** 구성한 모든 도구 및 IDE를 사용하여 수행할수 있습니다.

1. Visual Studio 코드 IDE에서 Marketplace 확장을 통해 **VSCode AEM Sync** 를 설치했는지 확인합니다.

   ![VSCode AEM 동기화](./assets/map-components/vscode-aem-sync.png)

2. 프로젝트 탐색기에서 **ui.content** 모듈을 확장하고 탐색합니다 `/conf/wknd-spa-react/settings/wcm/templates`.

3. **폴더를 마우스 오른쪽 단추로 클릭하고** AEM 서버에서 `templates` 가져오기를 선택합니다 ****.

   ![VSCode 가져오기 템플릿](./assets/map-components/import-aem-servervscode.png)

4. 컨텐츠를 가져오는 단계를 반복하지만 에 있는 **정책** 폴더를 선택합니다 `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect에 있는 `filter.xml` 파일 `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   이 `filter.xml` 파일은 패키지와 함께 설치할 노드의 경로를 식별합니다. 기존 컨텐츠가 수정되지 않고 새 컨텐츠만 추가된다는 것을 나타내는 각 필터 `mode="merge"` 에 표시됩니다. 컨텐츠 작성자는 이러한 경로를 업데이트하고 있으므로 코드 배포가 컨텐츠를 덮어쓰지 **않는** 것이 중요합니다. 필터 요소 [작업에 대한 자세한 내용은 FileVault 설명서를](https://jackrabbit.apache.org/filevault/filter.html) 참조하십시오.

   각 모듈 `ui.content/src/main/content/META-INF/vault/filter.xml` 에 의해 관리되는 서로 다른 노드를 비교하고 `ui.apps/src/main/content/META-INF/vault/filter.xml` 파악합니다.
