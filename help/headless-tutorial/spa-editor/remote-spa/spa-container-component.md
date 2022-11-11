---
title: 원격 SPA에 편집 가능한 React 컨테이너 구성 요소 추가
description: AEM 작성자가 구성 요소를 드래그하여 놓을 수 있는 원격 SPA에 편집 가능한 컨테이너 구성 요소를 추가하는 방법을 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 2%

---

# 편집 가능한 컨테이너 구성 요소

[고정 구성 요소](./spa-fixed-component.md) SPA 컨텐츠를 작성하는 데 약간의 유연성을 제공할 수 있지만, 이 접근 방식은 까다롭고 개발자가 편집 가능한 컨텐츠의 정확한 구성을 정의해야 합니다. 작성자에 의한 예외적인 경험 만들기를 지원하기 위해 SPA 편집기는 SPA에서 컨테이너 구성 요소의 사용을 지원합니다. 컨테이너 구성 요소를 사용하면 작성자가 기존의 AEM Sites 작성에서 하듯이 허용된 구성 요소를 컨테이너에 드래그하여 놓고 작성할 수 있습니다.

![편집 가능한 컨테이너 구성 요소](./assets/spa-container-component/intro.png)

이 장에서는 SPA에서 직접 편집 가능한 React 구성 요소를 사용하여 리치 컨텐츠 경험을 작성하고 레이아웃할 수 있는 편집 가능한 컨테이너를 홈 보기에 추가합니다.

## WKND 앱 업데이트

홈 보기에 컨테이너 구성 요소를 추가하려면

+ AEM React 편집 가능한 구성 요소의 `ResponsiveGrid` 구성 요소
+ ResponsiveGrid 구성 요소에서 사용할 사용자 지정 편집 가능한 React 구성 요소(텍스트 및 이미지)를 가져와 등록합니다

### ResponsiveGrid 구성 요소 사용

홈 보기에 편집 가능한 영역을 추가하려면

1. 열기 및 편집 `react-app/src/components/Home.js`
1. 가져오기 `ResponsiveGrid` 구성 요소 `@adobe/aem-react-editable-components` 그리고 거기에 `Home` 구성 요소.
1. 에서 다음 속성을 설정합니다. `<ResponsiveGrid...>` 구성 요소
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   이를 통해 `ResponsiveGrid` AEM 리소스에서 컨텐츠를 검색하는 구성 요소입니다.

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   다음 `itemPath` 지도 `responsivegrid` 에 정의된 노드 `Remote SPA Page` AEM 템플릿 및 은(는) `Remote SPA Page` AEM 템플릿.

   업데이트 `Home.js` 를 추가하려면 `<ResponsiveGrid...>` 구성 요소.

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

다음 `Home.js` 파일 형식은 다음과 같습니다.

![Home.js](./assets/spa-container-component/home-js.png)

## 편집 가능한 구성 요소 만들기

SPA Editor에서 제공하는 유연한 작성 경험 컨테이너의 전체 효과를 얻을 수 있습니다. 이미 편집 가능한 제목 구성 요소를 만들었지만, 작성자가 새로 추가된 응답형 그리드 구성 요소에서 편집 가능한 텍스트 및 이미지 구성 요소를 사용할 수 있도록 몇 가지 더 만들어 보겠습니다.

편집 가능한 새로운 텍스트 및 이미지 반응 구성 요소는 다음과 같이 표시되는 편집 가능한 구성 요소 정의 패턴을 사용하여 만듭니다 [편집 가능한 고정 구성 요소](./spa-fixed-component.md).

### 편집 가능한 텍스트 구성 요소

1. IDE에서 SPA 프로젝트를 엽니다.
1. 에서 React 구성 요소 만들기 `src/components/editable/core/Text.js`
1. 다음 코드를 `Text.js`

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

1. 편집 가능한 React 구성 요소를 만들 위치 `src/components/editable/EditableText.js`
1. 다음 코드를 `EditableText.js`

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

편집 가능한 텍스트 구성 요소 구현은 다음과 같습니다.

![편집 가능한 텍스트 구성 요소](./assets/spa-container-component/text-js.png)

### 이미지 구성 요소

1. IDE에서 SPA 프로젝트를 엽니다.
1. 에서 React 구성 요소 만들기 `src/components/editable/core/Image.js`
1. 다음 코드를 `Image.js`

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

1. 편집 가능한 React 구성 요소를 만들 위치 `src/components/editable/EditableImage.js`
1. 다음 코드를 `EditableImage.js`

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


1. SCSS 파일 만들기 `src/components/editable/EditableImage.scss` 에 대한 사용자 지정 스타일을 제공합니다 `EditableImage.scss`. 이러한 스타일은 편집 가능한 React 구성 요소의 CSS 클래스를 대상으로 합니다.
1. 다음 SCSS를에 추가합니다 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 가져오기 `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

편집 가능한 이미지 구성 요소 구현은 다음과 같아야 합니다.

![편집 가능한 이미지 구성 요소](./assets/spa-container-component/image-js.png)


### 편집 가능한 구성 요소 가져오기

새로 만든 `EditableText` 및 `EditableImage` React 구성 요소는 SPA에서 참조되며, AEM에서 반환된 JSON을 기반으로 동적으로 인스턴스화됩니다. 이러한 구성 요소를 SPA에서 사용할 수 있도록 하려면,에서 해당 구성 요소에 대한 가져오기 구문을 만듭니다. `Home.js`

1. IDE에서 SPA 프로젝트를 엽니다.
1. 파일을 엽니다. `src/Home.js`
1. 에 대한 가져오기 구문 추가 `AEMText` 및 `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

결과는 다음과 같습니다.

![Home.js](./assets/spa-container-component/home-js-imports.png)

이러한 가져오기가 _not_ 추가됨, `EditableText` 및 `EditableImage` 코드가 SPA에 의해 호출되지 않으므로 구성 요소는 제공된 리소스 유형에 매핑되지 않습니다.

## AEM에서 컨테이너 구성

AEM 컨테이너 구성 요소는 정책을 사용하여 허용된 구성 요소를 나타냅니다. SPA 구성 요소 옆에 매핑된 AEM 구성 요소만 SPA에서 렌더링할 수 있으므로 SPA 편집기를 사용할 때 중요한 구성입니다. 에 SPA 구현을 제공한 구성 요소만 허용됩니다.

+ `EditableTitle` 매핑된 대상 `wknd-app/components/title`
+ `EditableText` 매핑된 대상 `wknd-app/components/text`
+ `EditableImage` 매핑된 대상 `wknd-app/components/image`

원격 SPA 페이지 템플릿의 Reponsivegrid 컨테이너를 구성하려면 다음을 수행합니다.

1. AEM 작성자에 로그인
1. 다음으로 이동 __도구 > 일반 > 템플릿 > WKND 앱__
1. 편집 __보고서 SPA 페이지__

   ![응답형 그리드 정책](./assets/spa-container-component/templates-remote-spa-page.png)

1. 선택 __구조__ 오른쪽 상단의 모드 전환기에서
1. 탭하여 을(를) 선택합니다 __레이아웃 컨테이너__
1. 탭하기 __정책__ 팝업 막대의 아이콘

   ![응답형 그리드 정책](./assets/spa-container-component/templates-policies-action.png)

1. 오른쪽에, 아래에 __허용된 구성 요소__ 탭, 확장 __WKND 앱 - 컨텐츠__
1. 다음을 선택하기만 하면 됩니다.
   + 이미지
   + 텍스트
   + 제목

   ![원격 SPA 페이지](./assets/spa-container-component/templates-allowed-components.png)

1. __Done__&#x200B;을 누릅니다

## AEM에서 컨테이너 작성

SPA이 업데이트되어 가 포함된 후 `<ResponsiveGrid...>`, 3개의 편집 가능한 React 구성 요소에 대한 래퍼(`EditableTitle`, `EditableText`, 및 `EditableImage`)이고 AEM이 일치하는 템플릿 정책으로 업데이트되므로 컨테이너 구성 요소에서 컨텐츠 작성을 시작할 수 있습니다.

1. AEM 작성자에 로그인
1. 다음으로 이동 __Sites > WKND 앱__
1. 탭 __홈__ 을(를) 선택합니다. __편집__ 맨 위 작업 표시줄에서
   + AEM 프로젝트 원형 중 프로젝트를 생성할 때 자동으로 추가되었으므로 &quot;Hello World&quot; 텍스트 구성 요소가 표시됩니다
1. 선택 __편집__ 페이지 편집기의 오른쪽 상단에 있는 모드 선택기에서
1. 을(를) 찾습니다 __레이아웃 컨테이너__ 제목 아래 편집 가능한 영역
1. 를 엽니다. __페이지 편집기의 사이드 바__, 을(를) 선택하고 을(를) 선택합니다. __구성 요소 보기__
1. 다음 구성 요소를 __레이아웃 컨테이너__
   + 이미지
   + 제목
1. 구성 요소를 드래그하여 다음 순서로 재정렬합니다.
   1. 제목
   1. 이미지
   1. 텍스트
1. __작성자__ a __제목__ 구성 요소
   1. 제목 구성 요소를 탭하고 __렌치__ 아이콘 대상 __편집__ 제목 구성 요소
   1. 다음 텍스트를 추가합니다.
      + 제목: __여름이 다가오고 있어, 그것을 최대한 활용하자!__
      + 유형: __H1__
   1. __Done__&#x200B;을 누릅니다
1. __작성자__ a __이미지__ 구성 요소
   1. 이미지 구성 요소의 사이드 바(자산 보기로 전환한 후)에서 이미지를 드래그합니다
   1. 이미지 구성 요소를 탭하고 __렌치__ 편집할 아이콘
   1. 을(를) 확인합니다. __이미지가 장식용임__ 확인란
   1. __Done__&#x200B;을 누릅니다
1. __작성자__ a __텍스트__ 구성 요소
   1. 텍스트 구성 요소를 탭하고 __렌치__ 아이콘
   1. 다음 텍스트를 추가합니다.
      + _지금, 여러분은 1주간의 모든 모험에서 15퍼센트를 얻을 수 있고, 2주 이상 되는 모든 모험에서 20퍼센트를 받을 수 있습니다! 체크아웃 시 할인을 받으려면 캠페인 코드 SUMMERISCOMING을 추가합니다!_
   1. __Done__&#x200B;을 누릅니다

1. 이제 구성 요소가 작성되었지만 수직으로 스택됩니다.

   ![작성된 구성 요소](./assets/spa-container-component/authored-components.png)

구성 요소의 크기 및 레이아웃을 조정할 수 있도록 하려면 AEM 레이아웃 모드 를 사용하십시오.

1. 다음으로 전환 __레이아웃 모드__ 오른쪽 상단에서 모드 선택기 사용
1. __크기 조정__ 이미지 및 텍스트 구성 요소가 나란히 표시되도록 합니다
   + __이미지__ 구성 요소 __8열 너비__
   + __텍스트__ 구성 요소 __3열 너비__

   ![레이아웃 구성 요소](./assets/spa-container-component/layout-components.png)

1. __미리 보기__ AEM 페이지 편집기의 변경 사항
1. 로컬에서 실행되는 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000) 작성된 변경 사항을 보려면

   ![SPA의 컨테이너 구성 요소](./assets/spa-container-component/localhost-final.png)


## 축하합니다!

작성자가 편집 가능한 구성 요소를 WKND 앱에 추가할 수 있는 컨테이너 구성 요소를 추가했습니다. 이제 방법을 알 수 있습니다.

+ AEM React 편집 가능한 구성 요소의 `ResponsiveGrid` SPA의 구성 요소
+ 컨테이너 구성 요소를 통해 SPA에서 사용할 편집 가능한 React 구성 요소(텍스트 및 이미지)를 만들고 등록합니다
+ SPA이 활성화된 구성 요소를 허용하도록 원격 SPA 페이지 템플릿을 구성합니다
+ 컨테이너 구성 요소에 편집 가능한 구성 요소 추가
+ SPA 편집기의 작성자 및 레이아웃 구성 요소

## 다음 단계

다음 단계에서는 다음과 같은 기술을 사용하여 다음을 수행합니다 [Adventure Details 경로에 편집 가능한 구성 요소 추가](./spa-dynamic-routes.md) SPA에서 확인하십시오.
