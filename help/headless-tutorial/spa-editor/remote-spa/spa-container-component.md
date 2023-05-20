---
title: 원격 SPA에 편집 가능한 React 컨테이너 구성 요소 추가
description: AEM 작성자가 구성 요소를 원격 SPA에 끌어다 놓을 수 있도록 하는 편집 가능한 컨테이너 구성 요소를 추가하는 방법에 대해 알아봅니다.
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
ht-degree: 1%

---

# 편집 가능한 컨테이너 구성 요소

[고정 구성 요소](./spa-fixed-component.md) SPA 컨텐츠를 작성하는 데 약간의 유연성을 제공하지만, 이 접근 방식은 엄격하여 개발자가 편집 가능한 컨텐츠의 정확한 구성을 정의해야 합니다. 작성자가 탁월한 경험을 만들 수 있도록 SPA Editor는 SPA에서 컨테이너 구성 요소를 사용할 수 있도록 지원합니다. 작성자는 컨테이너 구성 요소를 통해 허용된 구성 요소를 드래그하여 컨테이너에 넣고, 기존 AEM Sites 작성에서와 마찬가지로 이 구성 요소를 작성할 수 있습니다.

![편집 가능한 컨테이너 구성 요소](./assets/spa-container-component/intro.png)

이 장에서는 작성자가 SPA에서 바로 Editable React 구성 요소를 사용하여 풍부한 콘텐츠 경험을 구성하고 레이아웃할 수 있도록 하는 편집 가능한 컨테이너를 홈 보기에 추가합니다.

## WKND 앱 업데이트

컨테이너 구성 요소를 홈 보기에 추가하려면 다음을 수행합니다.

+ AEM React Editable Component 가져오기 `ResponsiveGrid` 구성 요소
+ ResponsiveGrid 구성 요소에서 사용할 사용자 지정 Editable React 구성 요소(텍스트 및 이미지)를 가져와서 등록합니다

### ResponsiveGrid 구성 요소 사용

편집 가능한 영역을 홈 보기에 추가하려면 다음을 수행합니다.

1. 열기 및 편집 `react-app/src/components/Home.js`
1. 가져오기 `ResponsiveGrid` 구성 요소 출처 `@adobe/aem-react-editable-components` 및에 추가합니다. `Home` 구성 요소.
1. 에서 다음 속성을 설정합니다. `<ResponsiveGrid...>` 구성 요소
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   이에 따라 `ResponsiveGrid` AEM 리소스에서 콘텐츠를 검색할 구성 요소:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   다음 `itemPath` 에 매핑 `responsivegrid` 에 정의된 노드 `Remote SPA Page` AEM 템플릿에서 생성된 새 AEM 페이지에 자동으로 만들어집니다. `Remote SPA Page` AEM 템플릿입니다.

   업데이트 `Home.js` 을(를) 추가하려면 `<ResponsiveGrid...>` 구성 요소.

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

다음 `Home.js` 파일은 다음과 같아야 합니다.

![Home.js](./assets/spa-container-component/home-js.png)

## 편집 가능한 구성 요소 만들기

SPA Editor에서 제공하는 유연한 작성 경험 컨테이너를 최대한 활용하십시오. 이미 편집 가능한 제목 구성 요소를 만들었지만 작성자가 새로 추가된 ResponsiveGrid 구성 요소에서 편집 가능한 텍스트 및 이미지 구성 요소를 사용할 수 있도록 하는 몇 가지 사항을 더 만들어 보겠습니다.

새로운 편집 가능한 텍스트 및 이미지 반응형 구성 요소는에 노출된 편집 가능한 구성 요소 정의 패턴을 사용하여 만들어집니다. [편집 가능한 고정 구성 요소](./spa-fixed-component.md).

### 편집 가능한 텍스트 구성 요소

1. IDE에서 SPA 프로젝트 열기
1. 다음 위치에 React 구성 요소 만들기 `src/components/editable/core/Text.js`
1. 에 다음 코드를 추가합니다 `Text.js`

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

1. 다음 위치에서 편집 가능한 React 구성 요소 만들기 `src/components/editable/EditableText.js`
1. 에 다음 코드를 추가합니다 `EditableText.js`

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
1. 다음 위치에 React 구성 요소 만들기 `src/components/editable/core/Image.js`
1. 에 다음 코드를 추가합니다 `Image.js`

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

1. 다음 위치에서 편집 가능한 React 구성 요소 만들기 `src/components/editable/EditableImage.js`
1. 에 다음 코드를 추가합니다 `EditableImage.js`

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


1. SCSS 파일 만들기 `src/components/editable/EditableImage.scss` 의 사용자 지정 스타일을 제공합니다. `EditableImage.scss`. 이러한 스타일은 편집 가능한 React 구성 요소의 CSS 클래스를 대상으로 합니다.
1. 에 다음 SCSS 추가 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 가져오기 `EditableImage.scss` 위치: `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

편집 가능한 이미지 구성 요소 구현은 다음과 같아야 합니다.

![편집 가능한 이미지 구성 요소](./assets/spa-container-component/image-js.png)


### 편집 가능한 구성 요소 가져오기

새로 생성된 `EditableText` 및 `EditableImage` React 구성 요소는 SPA에서 참조하며 AEM에서 반환되는 JSON을 기반으로 동적으로 인스턴스화됩니다. SPA에서 이러한 구성 요소를 사용할 수 있도록 하려면에서 해당 구성 요소에 대한 가져오기 구문을 만듭니다. `Home.js`

1. IDE에서 SPA 프로젝트 열기
1. 파일 열기 `src/Home.js`
1. 다음에 대한 가져오기 구문 추가 `AEMText` 및 `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

결과는 다음과 같아야 합니다.

![Home.js](./assets/spa-container-component/home-js-imports.png)

이 가져오기가 _아님_ 추가됨, `EditableText` 및 `EditableImage` 코드가 SPA에 의해 호출되지 않으므로 구성 요소는 제공된 리소스 유형에 매핑되지 않습니다.

## AEM에서 컨테이너 구성

AEM 컨테이너 구성 요소는 정책을 사용하여 허용된 구성 요소를 지시합니다. SPA Editor는 SPA 구성 요소 대응 요소를 매핑한 AEM 구성 요소만 SPA에서 렌더링할 수 있으므로 중요한 구성입니다. SPA 구현을 제공한 구성 요소만 허용되는지 확인하십시오.

+ `EditableTitle` 매핑됨 `wknd-app/components/title`
+ `EditableText` 매핑됨 `wknd-app/components/text`
+ `EditableImage` 매핑됨 `wknd-app/components/image`

원격 SPA 페이지 템플릿의 reponsivegrid 컨테이너를 구성하려면 다음과 같이 하십시오.

1. AEM 작성자에 로그인
1. 다음으로 이동 __도구 > 일반 > 템플릿 > WKND 앱__
1. 편집 __보고서 SPA 페이지__

   ![반응형 그리드 정책](./assets/spa-container-component/templates-remote-spa-page.png)

1. 선택 __구조__ 오른쪽 상단의 모드 전환기에서
1. 탭하여 선택 __레이아웃 컨테이너__
1. 탭 __정책__ 팝업 막대의 아이콘

   ![반응형 그리드 정책](./assets/spa-container-component/templates-policies-action.png)

1. 오른쪽, 아래 __허용된 구성 요소__ 탭, 확장 __WKND 앱 - 콘텐츠__
1. 다음 항목만 선택해야 합니다.
   + 이미지
   + 텍스트
   + 제목

   ![원격 SPA 페이지](./assets/spa-container-component/templates-allowed-components.png)

1. 누르기 __완료__

## AEM에서 컨테이너 작성

SPA이 을 포함하도록 업데이트한 후 `<ResponsiveGrid...>`, 편집 가능한 React 구성 요소 3개 래퍼(`EditableTitle`, `EditableText`, 및 `EditableImage`) 일치하는 템플릿 정책으로 AEM이 업데이트되면 컨테이너 구성 요소에서 컨텐츠 작성을 시작할 수 있습니다.

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱__
1. 누르기 __홈__ 및 선택 __편집__ 맨 위의 작업 표시줄에서
   + AEM Project Archetype에서 프로젝트를 생성할 때 자동으로 추가되었으므로 &quot;Hello World&quot; 텍스트 구성 요소가 표시됩니다
1. 선택 __편집__ 페이지 편집기의 오른쪽 상단에 있는 모드 선택기에서
1. 를 찾습니다. __레이아웃 컨테이너__ 제목 아래의 편집 가능한 영역
1. 를 엽니다. __페이지 편집기의 사이드바__&#x200B;을(를) 클릭하고 __구성 요소 보기__
1. 다음 구성 요소를 __레이아웃 컨테이너__
   + 이미지
   + 제목
1. 구성 요소를 드래그하여 다음 순서로 순서를 변경합니다.
   1. 제목
   1. 이미지
   1. 텍스트
1. __작성자__ 다음 __제목__ 구성 요소
   1. 제목 구성 요소를 탭하고 __렌치__ 아이콘 위치 __편집__ 제목 구성 요소
   1. 다음 텍스트를 추가합니다.
      + 제목: __여름이 다가오고 있어, 최대한 활용하자!__
      + 유형: __H1__
   1. 누르기 __완료__
1. __작성자__ 다음 __이미지__ 구성 요소
   1. 이미지 구성 요소의 사이드 바에서(에셋 보기로 전환한 후) 이미지를 드래그합니다
   1. 이미지 구성 요소를 탭하고 __렌치__ 편집할 아이콘
   1. 다음 확인: __이미지가 장식용임__ 확인란
   1. 누르기 __완료__
1. __작성자__ 다음 __텍스트__ 구성 요소
   1. 텍스트 구성 요소를 탭하고 을 탭하여 텍스트 구성 요소를 편집합니다. __렌치__ 아이콘
   1. 다음 텍스트를 추가합니다.
      + _지금 당장, 당신은 모든 1주 모험에서 15%를 얻을 수 있고 2주 이상 모든 모험에서 20%를 할인받을 수 있습니다! 체크아웃할 때 캠페인 코드 SUMMERISCOMING 을 추가하여 할인을 받으십시오!_
   1. 누르기 __완료__

1. 이제 구성 요소가 작성되었지만 세로로 스택됩니다.

   ![작성된 구성 요소](./assets/spa-container-component/authored-components.png)

구성 요소의 크기와 레이아웃을 조정할 수 있도록 AEM 레이아웃 모드 를 사용하십시오.

1. 다음으로 전환 __레이아웃 모드__ 오른쪽 상단의 모드 선택기 사용
1. __크기 조정__ 이미지 및 텍스트 구성 요소를 나란히 배치할 수 있습니다
   + __이미지__ 구성 요소는 다음과 같아야 합니다 __8열 너비__
   + __텍스트__ 구성 요소는 다음과 같아야 합니다 __3열 너비__

   ![레이아웃 구성 요소](./assets/spa-container-component/layout-components.png)

1. __미리 보기__ AEM 페이지 편집기의 변경 사항
1. 로컬에서 실행 중인 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000) 작성된 변경 내용을 보려면!

   ![SPA의 컨테이너 구성 요소](./assets/spa-container-component/localhost-final.png)


## 축하합니다!

작성자가 편집 가능한 구성 요소를 WKND 앱에 추가할 수 있는 컨테이너 구성 요소를 추가했습니다. 이제 다음 방법을 이해할 수 있습니다.

+ AEM React Editable 구성 요소 사용 `ResponsiveGrid` SPA 구성 요소
+ 컨테이너 구성 요소를 통해 SPA에서 사용할 편집 가능한 React 구성 요소(텍스트 및 이미지)를 만들고 등록합니다
+ SPA 지원 구성 요소를 허용하도록 원격 SPA 페이지 템플릿 구성
+ 컨테이너 구성 요소에 편집 가능한 구성 요소 추가
+ SPA Editor에서 구성 요소 작성 및 레이아웃

## 다음 단계

다음 단계에서는 이와 동일한 기술을 사용하여 [어드벤처 세부 정보 경로에 편집 가능한 구성 요소 추가](./spa-dynamic-routes.md) SPA에서
