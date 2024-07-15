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
duration: 164
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 1%

---

# 편집 가능한 고정 구성 요소

편집 가능한 React 구성 요소는 &quot;고정&quot;되거나 SPA 보기에 하드 코딩할 수 있습니다. 이 경우 개발자는 SPA Editor와 호환되는 구성 요소를 SPA AEM 보기에 배치할 수 있으며 사용자는 SPA Editor에서 구성 요소의 콘텐츠를 작성할 수 있습니다.

![고정 구성 요소](./assets/spa-fixed-component/intro.png)

이 장에서는 `Home.js`에 하드 코딩된 텍스트인 홈 보기의 제목 &quot;현재 모험&quot;을 고정되었지만 편집 가능한 제목 구성 요소로 바꿉니다. 고정 구성 요소를 사용하면 제목 배치를 보장할 뿐만 아니라 제목 텍스트를 작성하고 개발 주기 외부에서 변경할 수도 있습니다.

## WKND 앱 업데이트

__고정__ 구성 요소를 홈 보기에 추가하려면:

+ 사용자 지정 편집 가능한 제목 구성 요소를 만들고 프로젝트의 제목 리소스 유형에 등록합니다.
+ SPA 홈 보기에 편집 가능한 제목 구성 요소를 배치합니다.

### 편집 가능한 React Title 구성 요소 만들기

SPA 홈 보기에서 하드 코딩된 텍스트 `<h2>Current Adventures</h2>`을(를) 사용자 지정 편집 가능한 제목 구성 요소로 바꿉니다. 제목 구성 요소를 사용하기 전에 다음을 수행해야 합니다.

1. 사용자 지정 제목 React 구성 요소 만들기
1. 편집할 수 있도록 `@adobe/aem-react-editable-components`의 메서드를 사용하여 사용자 지정 제목 구성 요소를 장식합니다.
1. 편집 가능한 제목 구성 요소를 `MapTo`에 등록하면 나중에 [컨테이너 구성 요소에서 사용할 수 있습니다](./spa-container-component.md).

이를 위해 진행되는 작업:

1. IDE의 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app`에서 원격 SPA 프로젝트 열기
1. `react-app/src/components/editable/core/Title.js`에서 React 구성 요소 만들기
1. `Title.js`에 다음 코드를 추가하십시오.

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

1. `react-app/src/components/editable/EditableTitle.js`에서 React 구성 요소 만들기
1. `EditableTitle.js`에 다음 코드를 추가하십시오.

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

   이 `EditableTitle` React 구성 요소는 `Title` React 구성 요소를 래핑하고 AEM SPA Editor에서 편집할 수 있도록 래핑 및 데코레이트합니다.

### React EditableTitle 구성 요소 사용

EditableTitle React 구성 요소가에 등록되어 React 앱 내에서 사용할 수 있으므로 홈 보기에서 하드 코딩된 제목 텍스트를 바꾸십시오.

1. `react-app/src/components/Home.js` 편집
1. 하단의 `Home()`에서 `EditableTitle`을(를) 가져와 하드 코딩된 제목을 새 `AEMTitle` 구성 요소로 바꿉니다.

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

`Home.js` 파일은 다음과 같아야 합니다.

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEM에서 제목 구성 요소 작성

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱__(으)로 이동
1. __홈__&#x200B;을 탭하고 맨 위의 작업 표시줄에서 __편집__&#x200B;을 선택합니다.
1. 페이지 편집기의 오른쪽 상단에 있는 편집 모드 선택기에서 __편집__&#x200B;을 선택합니다.
1. 파란색 편집 윤곽선이 표시될 때까지 WKND 로고 아래 및 모험 목록 위에 기본 제목 텍스트를 마우스로 가리킵니다
1. 탭하여 구성 요소의 작업 표시줄을 표시한 다음 __렌치__&#x200B;를 탭하여 편집합니다

   ![제목 구성 요소 작업 표시줄](./assets/spa-fixed-component/title-action-bar.png)

1. 제목 구성 요소 작성:
   + 제목: __WKND 모험__
   + 형식/크기: __H2__

     ![제목 구성 요소 대화 상자](./assets/spa-fixed-component/title-dialog.png)

1. 저장하려면 __완료__&#x200B;를 탭하세요.
1. AEM SPA Editor에서 변경 사항 미리보기
1. [http://localhost:3000](http://localhost:3000)에서 로컬로 실행되는 WKND 앱을 새로 고치고 작성된 제목 변경 사항이 즉시 반영되었는지 확인합니다.

   ![SPA의 제목 구성 요소](./assets/spa-fixed-component/title-final.png)

## 축하합니다!

편집 가능한 고정 구성 요소를 WKND 앱에 추가했습니다. 이제 다음 방법을 이해할 수 있습니다.

+ 수정되었지만 편집 가능한 구성 요소를 SPA에 만들었습니다.
+ AEM에서 고정 구성 요소 작성
+ 원격 SPA에서 작성된 콘텐츠 보기

## 다음 단계

다음 단계는 작성자가 SPA에 구성 요소를 추가하고 편집할 수 있도록 허용하는 SPA에 [AEM ResponsiveGrid 컨테이너 구성 요소를 추가](./spa-container-component.md)하는 것입니다.
