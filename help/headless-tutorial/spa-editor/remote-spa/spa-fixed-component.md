---
title: 원격 SPA에 편집 가능한 고정 구성 요소 추가
description: 편집 가능한 고정 구성 요소를 원격 SPA에 추가하는 방법을 알아봅니다.
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 1%

---


# 편집 가능한 고정 구성 요소

편집 가능한 React 구성 요소는 &quot;고정&quot; 또는 SPA 보기로 하드 코딩할 수 있습니다. 이 경우 개발자는 SPA 편집기 호환 구성 요소를 SPA 보기에 배치하고 사용자가 AEM SPA 편집기에서 구성 요소의 컨텐츠를 작성할 수 있습니다.

![고정 구성 요소](./assets/spa-fixed-component/intro.png)

이 장에서는 `Home.js`에 있는 하드 코딩된 텍스트인 홈 보기의 제목인 &quot;현재 모험&quot;을 고정적이지만 편집 가능한 제목 구성 요소로 바꿉니다. 고정 구성 요소는 제목을 배치하도록 하지만, 제목의 텍스트를 작성할 수도 있고, 개발 주기 외부에서 변경할 수도 있습니다.

## WKND 앱 업데이트

홈 보기에 고정 구성 요소를 추가하려면

+ AEM React 코어 구성 요소 제목 구성 요소를 가져와 프로젝트의 제목 리소스 유형에 등록합니다
+ 편집 가능한 제목 구성 요소를 SPA 홈 보기에 배치합니다

### AEM React 코어 구성 요소의 제목 구성 요소에서 가져오기

SPA 홈 보기에서 하드 코딩된 텍스트 `<h2>Current Adventures</h2>`을 AEM React Core Components의 제목 구성 요소로 바꿉니다. 제목 구성 요소를 사용하려면 먼저 다음을 수행해야 합니다.

1. `@adobe/aem-core-components-react-base`에서 제목 구성 요소를 가져옵니다.
1. 개발자가 SPA에 배치할 수 있도록 `withMappable`을 사용하여 등록합니다
1. 또한 [컨테이너 구성 요소 나중에](./spa-container-component.md)에 사용할 수 있도록 `MapTo`에 등록하십시오.

이를 위해 진행되는 작업:

1. IDE에서 `~/Code/wknd-app/aem-guides-wknd-graphql/react-app`에 원격 SPA 프로젝트를 엽니다.
1. `react-app/src/components/aem/AEMTitle.js`에 React 구성 요소를 만듭니다.
1. 다음 코드를 `AEMTitle.js`에 추가합니다.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

구현 세부 사항에 대해서는 코드의 주석을 참조하십시오.

`AEMTitle.js` 파일은 다음과 같습니다.

![AEMitle.js](./assets/spa-fixed-component/aem-title-js.png)

### React AEMitle 구성 요소 사용

AEM React 코어 구성 요소의 제목 구성 요소가에 등록되었으며 React 앱 내에서 사용할 수 있으므로, 홈 보기에서 하드 코딩된 제목 텍스트를 대체합니다.

1. 편집 `react-app/src/App.js`
1. 맨 아래에 있는 `Home()`에서 하드 코딩된 제목을 새 `AEMTitle` 구성 요소로 바꿉니다.

   ```
   <h2>Current Adventures</h2>
   ```

   with

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   `Apps.js`을(를) 다음 코드로 업데이트합니다.

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Apps.js` 파일은 다음과 같습니다.

![App.js](./assets/spa-fixed-component/app-js.png)

## AEM에서 제목 구성 요소 작성

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱__&#x200B;으로 이동합니다.
1. __Home__&#x200B;을 탭하고 맨 위 작업 표시줄에서 __편집__&#x200B;을 선택합니다
1. 페이지 편집기의 오른쪽 상단에 있는 편집 모드 선택기에서 __편집__&#x200B;을 선택합니다
1. 파란색 편집 윤곽선이 표시될 때까지 WKND 로고 아래 및 모험 목록 위에 기본 제목 텍스트를 마우스로 가리킵니다
1. 구성 요소의 작업 표시줄을 표시하려면 탭하고 __렌치__&#x200B;를 탭하여 편집합니다

   ![제목 구성 요소 작업 표시줄](./assets/spa-fixed-component/title-action-bar.png)

1. 제목 구성 요소를 작성합니다.
   + 제목:__WKND Adventures__
   + 유형/크기:__H2__

      ![제목 구성 요소 대화 상자](./assets/spa-fixed-component/title-dialog.png)

1. __완료__&#x200B;를 눌러 저장합니다
1. AEM SPA 편집기에서 변경 사항 미리 보기
1. [http://localhost:3000](http://localhost:3000)에서 로컬로 실행되는 WKND 앱을 새로 고침하고 작성된 제목 변경 사항이 즉시 반영됩니다.

   ![SPA의 제목 구성 요소](./assets/spa-fixed-component/title-final.png)

## 축하합니다!

WKND 앱에 고정 편집 가능한 구성 요소를 추가했습니다. 이제 방법을 알 수 있습니다.

+ SPA에서 AEM React 코어 구성 요소를 가져와 재사용합니다
+ 고정되었지만 편집 가능한 구성 요소를 SPA에 추가합니다
+ AEM에서 고정 구성 요소 작성
+ Remote SPA에서 작성된 컨텐츠를 참조하십시오

## 다음 단계

다음 단계는 작성자가 SPA에 구성 요소를 추가 및 편집할 수 있도록 SPA에 [AEM ResponsiveGrid 컨테이너 구성 요소](./spa-container-component.md)를 추가하는 것입니다.
