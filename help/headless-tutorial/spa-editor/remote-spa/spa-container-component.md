---
title: 원격 SPA에 편집 가능한 React 컨테이너 구성 요소 추가
description: AEM 작성자가 구성 요소를 원격 SPA에 끌어다 놓을 수 있도록 하는 편집 가능한 컨테이너 구성 요소를 추가하는 방법에 대해 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 1%

---

# 편집 가능한 컨테이너 구성 요소

{{spa-editor-deprecation}}

[고정 구성 요소](./spa-fixed-component.md)는 SPA 콘텐츠를 작성하는 데 약간의 유연성을 제공하지만, 이 방법은 엄격하기 때문에 개발자가 편집 가능한 콘텐츠의 정확한 구성을 정의해야 합니다. 작성자가 탁월한 경험을 만들 수 있도록 지원하기 위해 SPA 편집기에서는 SPA의 컨테이너 구성 요소 사용을 지원합니다. 작성자는 컨테이너 구성 요소를 통해 허용된 구성 요소를 드래그하여 컨테이너에 넣고, 기존 AEM Sites 작성에서와 마찬가지로 이 구성 요소를 작성할 수 있습니다.

![편집 가능한 컨테이너 구성 요소](./assets/spa-container-component/intro.png)

이 장에서는 작성자가 SPA에서 직접 Editable React 구성 요소를 사용하여 풍부한 콘텐츠 경험을 구성하고 레이아웃할 수 있도록 하는 편집 가능한 컨테이너를 홈 보기에 추가합니다.

## WKND 앱 업데이트

컨테이너 구성 요소를 홈 보기에 추가하려면 다음을 수행합니다.

* AEM React Editable 구성 요소의 `ResponsiveGrid` 구성 요소 가져오기
* ResponsiveGrid 구성 요소에서 사용할 사용자 지정 Editable React 구성 요소(텍스트 및 이미지)를 가져와서 등록합니다

### ResponsiveGrid 구성 요소 사용

편집 가능한 영역을 홈 보기에 추가하려면 다음을 수행합니다.

1. `react-app/src/components/Home.js` 열기 및 편집
1. `@adobe/aem-react-editable-components`에서 `ResponsiveGrid` 구성 요소를 가져와 `Home` 구성 요소에 추가하십시오.
1. `<ResponsiveGrid...>` 구성 요소에서 다음 특성을 설정합니다.
   1. `pagePath = '/content/wknd-app/us/en/home'`
   1. `itemPath = 'root/responsivegrid'`

   이렇게 하면 `ResponsiveGrid` 구성 요소가 AEM 리소스에서 해당 콘텐츠를 검색하도록 지시합니다.

   1. `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`은(는) `Remote SPA Page` AEM 템플릿에 정의된 `responsivegrid` 노드에 매핑되며 `Remote SPA Page` AEM 템플릿에서 만든 새 AEM 페이지에 자동으로 만들어집니다.

   `Home.js`을(를) 업데이트하여 `<ResponsiveGrid...>` 구성 요소를 추가합니다.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Home.js` 파일은 다음과 같아야 합니다.

![Home.js](./assets/spa-container-component/home-js.png)

## 편집 가능한 구성 요소 만들기

SPA 편집기에서 제공하는 유연한 작성 경험 컨테이너를 최대한 활용하십시오. 이미 편집 가능한 제목 구성 요소를 만들었지만 작성자가 새로 추가된 ResponsiveGrid 구성 요소에서 편집 가능한 텍스트 및 이미지 구성 요소를 사용할 수 있도록 하는 몇 가지 사항을 더 만들어 보겠습니다.

새로운 편집 가능한 텍스트 및 이미지 반응 구성 요소는 [편집 가능한 고정 구성 요소](./spa-fixed-component.md)에 노출된 편집 가능한 구성 요소 정의 패턴을 사용하여 만들어집니다.

### 편집 가능한 텍스트 구성 요소

1. IDE에서 SPA 프로젝트 열기
1. `src/components/editable/core/Text.js`에서 React 구성 요소 만들기
1. `Text.js`에 다음 코드 추가

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. `src/components/editable/EditableText.js`에서 편집 가능한 React 구성 요소를 만듭니다.
1. `EditableText.js`에 다음 코드 추가

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

편집 가능한 텍스트 구성 요소 구현은 다음과 같아야 합니다.

![편집 가능한 텍스트 구성 요소](./assets/spa-container-component/text-js.png)

### 이미지 구성 요소

1. IDE에서 SPA 프로젝트 열기
1. `src/components/editable/core/Image.js`에서 React 구성 요소 만들기
1. `Image.js`에 다음 코드 추가

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. `src/components/editable/EditableImage.js`에서 편집 가능한 React 구성 요소를 만듭니다.
1. `EditableImage.js`에 다음 코드 추가

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. `EditableImage.scss`에 대한 사용자 지정 스타일을 제공하는 SCSS 파일 `src/components/editable/EditableImage.scss`을(를) 만듭니다. 이러한 스타일은 편집 가능한 React 구성 요소의 CSS 클래스를 대상으로 합니다.
1. `EditableImage.scss`에 다음 SCSS 추가

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. `EditableImage.js`에서 `EditableImage.scss` 가져오기

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

편집 가능한 이미지 구성 요소 구현은 다음과 같아야 합니다.

![편집 가능한 이미지 구성 요소](./assets/spa-container-component/image-js.png)


### 편집 가능한 구성 요소 가져오기

새로 만든 `EditableText` 및 `EditableImage` React 구성 요소는 SPA에서 참조되며, AEM에서 반환된 JSON을 기반으로 동적으로 인스턴스화됩니다. SPA에서 이러한 구성 요소를 사용할 수 있도록 하려면 `Home.js`에서 해당 구성 요소에 대한 가져오기 구문을 만드십시오.

1. IDE에서 SPA 프로젝트 열기
1. `src/Home.js` 파일을 엽니다.
1. `AEMText` 및 `AEMImage`에 대한 가져오기 문 추가

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

결과는 다음과 같아야 합니다.

![Home.js](./assets/spa-container-component/home-js-imports.png)

이 가져오기가 _추가되지 않음_&#x200B;인 경우 SPA에서 `EditableText` 및 `EditableImage` 코드를 호출하지 않으므로 구성 요소가 제공된 리소스 형식에 매핑되지 않습니다.

## AEM에서 컨테이너 구성

AEM 컨테이너 구성 요소는 정책을 사용하여 허용된 구성 요소를 지시합니다. SPA에서 SPA 구성 요소의 대응 요소를 매핑한 AEM 구성 요소만 렌더링할 수 있으므로 SPA 편집기를 사용할 때 매우 중요한 구성입니다. SPA 구현을 제공한 구성 요소만 허용되는지 확인하십시오.

* `EditableTitle`이(가) `wknd-app/components/title`에 매핑됨
* `EditableText`이(가) `wknd-app/components/text`에 매핑됨
* `EditableImage`이(가) `wknd-app/components/image`에 매핑됨

원격 SPA 페이지 템플릿의 Reponsivegrid 컨테이너를 구성하려면 다음을 수행하십시오.

1. AEM 작성자에 로그인
1. __도구 > 일반 > 템플릿 > WKND 앱으로 이동__
1. __보고서 SPA 페이지__ 편집

   ![응답형 격자 정책](./assets/spa-container-component/templates-remote-spa-page.png)

1. 오른쪽 상단의 모드 전환기에서 __구조__&#x200B;를 선택합니다.
1. 탭하여 __레이아웃 컨테이너__ 선택
1. 팝업 막대에서 __정책__ 아이콘을 탭합니다.

   ![응답형 격자 정책](./assets/spa-container-component/templates-policies-action.png)

1. 오른쪽의 __허용된 구성 요소__ 탭에서 __WKND 앱 - 콘텐츠__&#x200B;를 확장합니다.
1. 다음 항목만 선택해야 합니다.
   1. 이미지
   1. 텍스트
   1. 제목

   ![원격 SPA 페이지](./assets/spa-container-component/templates-allowed-components.png)

1. __완료__ 탭

## AEM에서 컨테이너 작성

SPA에서 `<ResponsiveGrid...>`을(를) 포함하도록 업데이트하고, 세 개의 편집 가능한 React 구성 요소(`EditableTitle`, `EditableText` 및 `EditableImage`)에 대한 래퍼를 업데이트하고, AEM이 일치하는 템플릿 정책으로 업데이트되면 컨테이너 구성 요소에서 콘텐츠 작성을 시작할 수 있습니다.

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱__(으)로 이동
1. __홈__&#x200B;을 탭하고 맨 위의 작업 표시줄에서 __편집__&#x200B;을 선택합니다.
   1. AEM Project Archetype에서 프로젝트를 생성할 때 자동으로 추가되었으므로 &quot;Hello World&quot; 텍스트 구성 요소가 표시됩니다
1. 페이지 편집기의 오른쪽 상단에 있는 모드 선택기에서 __편집__&#x200B;을 선택합니다.
1. 제목 아래에서 __레이아웃 컨테이너__ 편집 가능 영역을 찾습니다.
1. __페이지 편집기의 사이드바__&#x200B;를 열고 __구성 요소 보기__&#x200B;를 선택합니다.
1. 다음 구성 요소를 __레이아웃 컨테이너__(으)로 끌어옵니다.
   1. 이미지
   1. 제목
1. 구성 요소를 드래그하여 다음 순서로 순서를 변경합니다.
   1. 제목
   1. 이미지
   1. 텍스트
1. __제목__ 구성 요소를 __작성자__
   1. 제목 구성 요소를 탭하고 __렌치__ 아이콘을 탭하여 제목 구성 요소를 __편집__&#x200B;합니다.
   1. 다음 텍스트를 추가합니다.
      1. 제목: __여름이 다가오고 있습니다. 최대한 활용해 보세요!__
      1. 유형: __H1__
   1. __완료__ 탭
1. __이미지__ 구성 요소 __작성자__
   1. 이미지 구성 요소의 측면 막대(Assets 보기로 전환한 후)에서 이미지를 드래그합니다
   1. 이미지 구성 요소를 탭하고 __렌치__ 아이콘을 탭하여 편집합니다
   1. __이미지가 장식용임__ 확인란을 선택하십시오.
   1. __완료__ 탭
1. __텍스트__ 구성 요소를 __작성자__
   1. 텍스트 구성 요소를 탭하고 __렌치__ 아이콘을 탭하여 텍스트 구성 요소를 편집합니다
   1. 다음 텍스트를 추가합니다.
      1. _현재 1주일 모험은 15%, 2주 이상 모험은 20% 할인됩니다! 체크아웃할 때 캠페인 코드 SUMMERISCOMING을 추가하여 할인 혜택을 받으십시오!_
   1. __완료__ 탭

1. 이제 구성 요소가 작성되었지만 세로로 스택됩니다.

   ![작성된 구성 요소](./assets/spa-container-component/authored-components.png)

   AEM의 레이아웃 모드 를 사용하여 구성 요소의 크기와 레이아웃을 조정할 수 있습니다.

1. 오른쪽 상단의 모드 선택기를 사용하여 __레이아웃 모드__(으)로 전환
1. 이미지 및 텍스트 구성 요소가 나란히 표시되도록 __크기 조정__
   1. __이미지__ 구성 요소는 __8열__&#x200B;이어야 합니다.
   1. __텍스트__ 구성 요소는 __3열 너비여야 함__

   ![레이아웃 구성 요소](./assets/spa-container-component/layout-components.png)

1. AEM 페이지 편집기에서 변경 내용을 __미리 보기__
1. [http://localhost:3000](http://localhost:3000)에서 로컬로 실행 중인 WKND 앱을 새로 고쳐 작성된 변경 내용을 확인합니다.

   SPA의 ![컨테이너 구성 요소](./assets/spa-container-component/localhost-final.png)


## 축하합니다!

작성자가 편집 가능한 구성 요소를 WKND 앱에 추가할 수 있는 컨테이너 구성 요소를 추가했습니다. 이제 다음 방법을 이해할 수 있습니다.

* SPA에서 AEM React Editable 구성 요소 `ResponsiveGrid` 구성 요소 사용
* 컨테이너 구성 요소를 통해 SPA에서 사용할 편집 가능한 React 구성 요소(텍스트 및 이미지)를 만들고 등록합니다.
* SPA가 활성화된 구성 요소를 허용하도록 원격 SPA 페이지 템플릿 구성
* 컨테이너 구성 요소에 편집 가능한 구성 요소 추가
* SPA 편집기의 작성자 및 레이아웃 구성 요소

## 다음 단계

다음 단계에서는 이와 동일한 기술을 사용하여 SPA에서 [편집 가능한 구성 요소를 Adventure Details 경로에 추가](./spa-dynamic-routes.md)합니다.
