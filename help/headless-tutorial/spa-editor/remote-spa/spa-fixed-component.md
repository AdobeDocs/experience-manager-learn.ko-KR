---
title: 원격 SPA에 편집 가능한 고정 구성 요소 추가
description: 편집 가능한 고정 구성 요소를 원격 SPA에 추가하는 방법을 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# 편집 가능한 고정 구성 요소

편집 가능한 React 구성 요소는 &quot;고정&quot; 또는 SPA 보기로 하드 코딩할 수 있습니다. 이 경우 개발자는 SPA 편집기 호환 구성 요소를 SPA 보기에 배치하고 사용자가 AEM SPA 편집기에서 구성 요소의 컨텐츠를 작성할 수 있습니다.

![고정 구성 요소](./assets/spa-fixed-component/intro.png)

이 장에서는 의 하드 코딩된 텍스트인 홈 보기의 제목인 &quot;현재 모험&quot;을 바꿉니다 `Home.js` ( 고정되었지만 편집 가능한 제목 구성 요소 사용) 고정 구성 요소는 제목을 배치하도록 하지만, 제목의 텍스트를 작성할 수도 있고, 개발 주기 외부에서 변경할 수도 있습니다.

## WKND 앱 업데이트

을(를) 추가하려면 __고정__ 구성 요소를 홈 보기에 추가합니다.

+ 사용자 지정 편집 가능한 제목 구성 요소를 만들어 프로젝트의 제목 리소스 유형에 등록합니다
+ 편집 가능한 제목 구성 요소를 SPA 홈 보기에 배치합니다

### 편집 가능한 React 제목 구성 요소 만들기

SPA 홈 보기에서 하드 코딩된 텍스트를 바꿉니다 `<h2>Current Adventures</h2>` 편집 가능한 사용자 지정 제목 구성 요소에 사용할 수 있습니다. 제목 구성 요소를 사용하려면 먼저 다음을 수행해야 합니다.

1. 사용자 지정 Title React 구성 요소 만들기
1. 다음 방법으로 사용자 지정 제목 구성 요소 장식 `@adobe/aem-react-editable-components` 편집 가능하게 만듭니다.
1. 편집 가능한 제목 구성 요소를 `MapTo` 따라서 [나중에 컨테이너 구성 요소](./spa-container-component.md).

이를 위해 진행되는 작업:

1. 원격 SPA 프로젝트 열기 위치 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` IDE에서
1. 에서 React 구성 요소 만들기 `react-app/src/components/editable/core/Title.js`
1. 다음 코드를 `Title.js`.

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

   이 React 구성 요소는 아직 AEM SPA Editor를 사용하여 편집할 수 없습니다. 이 기본 구성 요소는 다음 단계에서 편집할 수 있게 됩니다.

   구현 세부 사항에 대해서는 코드의 주석을 참조하십시오.

1. 에서 React 구성 요소 만들기 `react-app/src/components/editable/EditableTitle.js`
1. 다음 코드를 `EditableTitle.js`.

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

   이 `EditableTitle` React 구성 요소는 다음과 같이 래핑됩니다. `Title` React 구성 요소, 포장 및 장식용으로 AEM SPA Editor에서 편집할 수 있습니다.

### React EditableTitle 구성 요소 사용

EditableTitle React 구성 요소가에 등록되었으며 React 앱 내에서 사용할 수 있으므로, 홈 보기에서 하드 코딩된 제목 텍스트를 대체합니다.

1. 편집 `react-app/src/components/Home.js`
1. 에서 `Home()` 맨 아래에 있는 가져오기 `EditableTitle` 하드 코딩된 제목을 `AEMTitle` 구성 요소:

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

다음 `Home.js` 파일 형식은 다음과 같습니다.

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEM에서 제목 구성 요소 작성

1. AEM 작성자에 로그인
1. 다음으로 이동 __Sites > WKND 앱__
1. 탭 __홈__ 을(를) 선택합니다. __편집__ 맨 위 작업 표시줄에서
1. 선택 __편집__ 페이지 편집기의 오른쪽 상단에 있는 편집 모드 선택기에서
1. 파란색 편집 윤곽선이 표시될 때까지 WKND 로고 아래 및 모험 목록 위에 기본 제목 텍스트를 마우스로 가리킵니다
1. 탭하여 구성 요소의 작업 표시줄을 표시한 다음, __렌치__  편집하려면 다음을 수행하십시오.

   ![제목 구성 요소 작업 표시줄](./assets/spa-fixed-component/title-action-bar.png)

1. 제목 구성 요소를 작성합니다.
   + 제목: __WKND 어드벤처__
   + 유형/크기: __H2__

      ![제목 구성 요소 대화 상자](./assets/spa-fixed-component/title-dialog.png)

1. 탭 __완료__ 저장
1. AEM SPA 편집기에서 변경 사항 미리 보기
1. 로컬에서 실행되는 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000) 작성된 제목 변경 사항을 즉시 반영합니다.

   ![SPA의 제목 구성 요소](./assets/spa-fixed-component/title-final.png)

## 축하합니다!

WKND 앱에 고정 편집 가능한 구성 요소를 추가했습니다. 이제 방법을 알 수 있습니다.

+ SPA에 고정되었지만 편집 가능한 구성 요소를 만들었습니다.
+ AEM에서 고정 구성 요소 작성
+ Remote SPA에서 작성된 컨텐츠를 참조하십시오

## 다음 단계

다음 단계는 다음과 같습니다 [AEM ResponsiveGrid 컨테이너 구성 요소 추가](./spa-container-component.md) 작성자가 SPA에 구성 요소를 추가 및 편집할 수 있도록 하는 SPA에 추가하시기 바랍니다!
