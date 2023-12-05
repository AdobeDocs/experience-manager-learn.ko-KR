---
title: 원격 SPA에 편집 가능한 고정 구성 요소 추가
description: 원격 SPA에 편집 가능한 고정 구성 요소를 추가하는 방법을 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 246
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 1%

---

# 편집 가능한 고정 구성 요소

편집 가능한 React 구성 요소는 &quot;고정&quot;되거나 SPA 보기에 하드 코딩할 수 있습니다. 이 경우 개발자는 SPA Editor와 호환되는 구성 요소를 SPA AEM 보기에 배치할 수 있으며 사용자는 SPA Editor에서 구성 요소의 콘텐츠를 작성할 수 있습니다.

![고정 구성 요소](./assets/spa-fixed-component/intro.png)

이 장에서는 의 하드 코딩된 텍스트인 홈 보기의 제목 &quot;Current Adventures&quot;를 대체합니다 `Home.js` (고정되었지만 편집 가능한 제목 구성 요소 포함) 고정 구성 요소를 사용하면 제목 배치를 보장할 뿐만 아니라 제목 텍스트를 작성하고 개발 주기 외부에서 변경할 수도 있습니다.

## WKND 앱 업데이트

을(를) 추가하려면 __고정__ 구성 요소를 홈 보기로 변환:

+ 사용자 지정 편집 가능한 제목 구성 요소를 만들고 프로젝트의 제목 리소스 유형에 등록합니다.
+ SPA 홈 보기에 편집 가능한 제목 구성 요소를 배치합니다.

### 편집 가능한 React Title 구성 요소 만들기

SPA 홈 보기에서 하드 코딩된 텍스트를 대체합니다 `<h2>Current Adventures</h2>` 사용자 지정 편집 가능한 제목 구성 요소 사용. 제목 구성 요소를 사용하기 전에 다음을 수행해야 합니다.

1. 사용자 지정 제목 React 구성 요소 만들기
1. 의 방법을 사용하여 사용자 정의 제목 구성 요소 장식 `@adobe/aem-react-editable-components` 편집할 수 있도록 설정.
1. 편집 가능한 제목 구성 요소 등록 `MapTo` 따라서 다음에서 사용할 수 있습니다 [컨테이너 구성 요소 나중에](./spa-container-component.md).

이를 위해 진행되는 작업:

1. 다음 위치에서 원격 SPA 프로젝트 열기 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` IDE에서
1. 다음 위치에 React 구성 요소 만들기 `react-app/src/components/editable/core/Title.js`
1. 에 다음 코드를 추가합니다 `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   이 React 구성 요소는 AEM SPA Editor를 사용하여 아직 편집할 수 없습니다. 이 기본 구성 요소는 다음 단계에서 편집할 수 있습니다.

   구현 세부 사항에 대해서는 코드의 주석을 읽어 보십시오.

1. 다음 위치에 React 구성 요소 만들기 `react-app/src/components/editable/EditableTitle.js`
1. 에 다음 코드를 추가합니다 `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   이 `EditableTitle` React 구성 요소가 `Title` AEM SPA Editor에서 편집할 수 있도록 구성 요소를 래핑하고 장식합니다.

### React EditableTitle 구성 요소 사용

EditableTitle React 구성 요소가에 등록되어 React 앱 내에서 사용할 수 있으므로 홈 보기에서 하드 코딩된 제목 텍스트를 바꾸십시오.

1. 편집 `react-app/src/components/Home.js`
1. 다음에서 `Home()` 맨 아래에서 가져오기 `EditableTitle` 하드 코딩된 제목을 새 제목으로 바꾸기 `AEMTitle` 구성 요소:

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

다음 `Home.js` 파일은 다음과 같아야 합니다.

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEM에서 제목 구성 요소 작성

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱__
1. 누르기 __홈__ 및 선택 __편집__ 맨 위의 작업 표시줄에서
1. 선택 __편집__ 페이지 편집기의 오른쪽 상단에 있는 편집 모드 선택기
1. 파란색 편집 윤곽선이 표시될 때까지 WKND 로고 아래 및 모험 목록 위에 기본 제목 텍스트를 마우스로 가리킵니다
1. 을 눌러 구성 요소의 작업 표시줄을 표시한 다음 을 누릅니다 __렌치__  편집하려면

   ![제목 구성 요소 작업 표시줄](./assets/spa-fixed-component/title-action-bar.png)

1. 제목 구성 요소 작성:
   + 제목: __어드벤처__
   + 유형/크기: __H2__

     ![제목 구성 요소 대화 상자](./assets/spa-fixed-component/title-dialog.png)

1. 누르기 __완료__ 저장
1. AEM SPA Editor에서 변경 사항 미리보기
1. 로컬에서 실행 중인 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000) 그리고 작성된 제목 변경 사항이 즉시 반영되었는지 확인합니다.

   ![SPA의 제목 구성 요소](./assets/spa-fixed-component/title-final.png)

## 축하합니다!

편집 가능한 고정 구성 요소를 WKND 앱에 추가했습니다. 이제 다음 방법을 이해할 수 있습니다.

+ 수정되었지만 편집 가능한 구성 요소를 SPA에 만들었습니다.
+ AEM에서 고정 구성 요소 작성
+ 원격 SPA에서 작성된 콘텐츠 보기

## 다음 단계

다음 단계는 다음과 같습니다 [AEM ResponsiveGrid 컨테이너 구성 요소 추가](./spa-container-component.md) 작성자가 SPA에 구성 요소를 추가하고 편집할 수 있도록 해 주는 SPA에!
