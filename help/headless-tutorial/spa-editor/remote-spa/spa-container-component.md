---
title: 원격 SPA에 편집 가능한 컨테이너 구성 요소 추가
description: AEM 작성자가 구성 요소를 드래그하여 원격 SPA에 놓을 수 있는 편집 가능한 컨테이너 구성 요소를 추가하는 방법을 살펴봅니다.
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 2%

---


# 편집 가능한 컨테이너 구성 요소

[고정 ](./spa-fixed-component.md) 구성 요소는 SPA 컨텐츠를 저작하는 데 약간의 유연성을 제공하지만 이 방법은 매우 까다롭고 개발자가 편집 가능한 컨텐츠의 정확한 구성을 정의해야 합니다. 작성자의 탁월한 경험 제작을 지원하기 위해 SPA 편집기는 SPA에서 컨테이너 구성 요소의 사용을 지원합니다. 컨텐츠 작성자는 컨테이너 구성 요소를 사용하여 기존의 AEM Sites 작성에서처럼 허용되는 구성 요소를 컨테이너에 드래그 앤 드롭하고 이를 제작할 수 있습니다.

![편집 가능한 컨테이너 구성 요소](./assets/spa-container-component/intro.png)

이 장에서는 작성자가 AEM의 반응형 핵심 구성 요소를 사용하여 리치 컨텐츠 경험을 구성하고 레이아웃할 수 있도록 편집 가능한 컨테이너를 홈 보기에 추가하겠습니다.

## WKND 앱 업데이트

홈 보기에 컨테이너 구성 요소를 추가하려면:

+ AEM React Editable 구성 요소의 ResponsiveGrid 구성 요소 가져오기
+ 컨테이너 구성 요소에서 사용할 AEM React 핵심 구성 요소(텍스트 및 이미지)를 가져와 등록합니다.

### ResponsiveGrid 컨테이너 구성 요소에서 가져오기

편집 가능 영역을 홈 보기에 배치하려면 다음을 수행해야 합니다.

1. `@adobe/aem-react-editable-components`에서 ResponsiveGrid 구성 요소 가져오기
1. 개발자가 SPA에 배치할 수 있도록 `withMappable`을 사용하여 등록하십시오.
1. 또한 다른 컨테이너 구성 요소에서 다시 사용할 수 있도록 `MapTo`에 등록하고 컨테이너를 효과적으로 중첩합니다.

이를 위해 진행되는 작업:

1. IDE에서 SPA 프로젝트 열기
1. `src/components/aem/AEMResponsiveGrid.js`에서 반응 구성 요소 만들기
1. 다음 코드를 `AEMResponsiveGrid.js`에 추가합니다.

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

코드는 AEM Reach Core Components의 제목 구성 요소](./spa-fixed-component.md)을(를) 가져온 `AEMTitle.js`과(와) 유사합니다.[


`AEMResponsiveGrid.js` 파일은 다음과 같아야 합니다.

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### AEMRespiongrid SPA 구성 요소 사용

AEM ResponsiveGrid 구성 요소가 등록되어 SPA 내에서 사용할 수 있으므로 홈 보기에 배치할 수 있습니다.

1. `react-app/src/App.js` 열기 및 편집
1. `AEMResponsiveGrid` 구성 요소를 가져와 `<AEMTitle ...>` 구성 요소 위에 배치합니다.
1. `<AEMResponsiveGrid...>` 구성 요소에서 다음 특성을 설정합니다.
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   이렇게 하면 이 `AEMResponsiveGrid` 구성 요소가 AEM 리소스에서 해당 컨텐츠를 검색하도록 지시됩니다.

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`은 `Remote SPA Page` AEM Template에 정의된 `responsivegrid` 노드에 매핑되며 `Remote SPA Page` AEM Template에서 만든 새 AEM 페이지에 자동으로 만들어집니다.

   `App.js`을 업데이트하여 `<AEMResponsiveGrid...>` 구성 요소를 추가합니다.

   ```
   ...
   import AEMResponsiveGrid from './components/aem/AEMResponsiveGrid';
   ...
   
   function Home() {
   return (
       <div className="Home">
           <AEMResponsiveGrid
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='root/responsivegrid'/>
   
           <AEMTitle
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='title'/>
           <Adventures />
       </div>
   );
   }
   ```

`Apps.js` 파일은 다음과 같아야 합니다.

![App.js](./assets/spa-container-component/app-js.png)

## 편집 가능한 구성 요소 만들기

SPA Editor에서 제공하는 유연한 작성 경험 컨테이너의 전체 효과를 얻을 수 있습니다. 이미 편집 가능한 제목 구성 요소를 만들었지만 작성자가 새로 추가된 컨테이너 구성 요소에서 텍스트 및 이미지 AEM WCM 핵심 구성 요소를 사용할 수 있도록 몇 가지 더 추가해 보겠습니다.

### 텍스트 구성 요소

1. IDE에서 SPA 프로젝트 열기
1. `src/components/aem/AEMText.js`에서 반응 구성 요소 만들기
1. 다음 코드를 `AEMText.js`에 추가합니다.

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

`AEMText.js` 파일은 다음과 같아야 합니다.

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### 이미지 구성 요소

1. IDE에서 SPA 프로젝트 열기
1. `src/components/aem/AEMImage.js`에서 반응 구성 요소 만들기
1. 다음 코드를 `AEMImage.js`에 추가합니다.

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. `AEMImage.scss`에 대한 사용자 정의 스타일을 제공하는 SCSS 파일 `src/components/aem/AEMImage.scss`을 만듭니다. 이러한 스타일은 AEM React 코어 구성 요소의 BEM 표기법 CSS 클래스를 대상으로 합니다.
1. 다음 SCSS를 `AEMImage.scss`에 추가

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. `AEMImage.js`에서 `AEMImage.scss` 가져오기

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

`AEMImage.js` 및 `AEMImage.scss`은 다음과 같아야 합니다.

![AEMImage.js 및 AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### 편집 가능한 구성 요소 가져오기

새로 만든 `AEMText` 및 `AEMImage` SPA 구성 요소는 SPA에서 참조되며 AEM에서 반환되는 JSON을 기반으로 동적으로 인스턴스화됩니다. 이러한 구성 요소를 SPA에서 사용할 수 있도록 하려면 `App.js`에서 해당 구성 요소에 대한 가져오기 문을 만듭니다.

1. IDE에서 SPA 프로젝트 열기
1. `src/App.js` 파일을 엽니다.
1. `AEMText` 및 `AEMImage`에 대한 가져오기 문 추가

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


결과는 다음과 같아야 합니다.

![Apps.js](./assets/spa-container-component/app-js-imports.png)

이러한 가져오기가 _이(가) 추가되지 않은 경우_&#x200B;은(는) SPA에 의해 `AEMText` 및 `AEMImage` 코드가 호출되지 않으므로 구성 요소가 제공된 리소스 유형에 대해 등록되지 않습니다.

## AEM에서 컨테이너 구성

AEM 컨테이너 구성 요소는 정책을 사용하여 허용된 구성 요소를 나타냅니다. AEM Editor를 사용할 때는 SPA 구성 요소에 매핑되어 있는 SPA WCM 핵심 구성 요소만 SPA에서 렌더링할 수 있으므로 이 작업은 중요한 구성으로 수행됩니다. SPA 구현을 제공한 구성 요소만 허용됩니다.

+ `AEMTitle` 매핑에  `wknd-app/components/title`
+ `AEMText` 매핑에  `wknd-app/components/text`
+ `AEMImage` 매핑에  `wknd-app/components/image`

원격 SPA 페이지 템플릿의 reponsivegrid 컨테이너를 구성하려면:

1. AEM 작성자 로그인
1. __도구 > 일반 > 템플릿 > WKND 앱__&#x200B;으로 이동합니다.
1. __보고서 SPA 페이지 편집__

   ![반응형 격자 정책](./assets/spa-container-component/templates-remote-spa-page.png)

1. 오른쪽 상단의 모드 전환기에서 __구조__&#x200B;를 선택합니다.
1. 을 눌러 __레이아웃 컨테이너__ 선택
1. 팝업 막대에서 __정책__ 아이콘을 누릅니다.

   ![반응형 격자 정책](./assets/spa-container-component/templates-policies-action.png)

1. 오른쪽의 __허용된 구성 요소__ 탭에서 __WKND APP - CONTENT__&#x200B;을 확장합니다.
1. 다음 항목만 선택되었는지 확인합니다.
   + 이미지
   + 텍스트
   + 제목

   ![원격 SPA 페이지](./assets/spa-container-component/templates-allowed-components.png)

1. __Done__&#x200B;을 누릅니다

## AEM에서 컨테이너 작성

`<AEMResponsiveGrid...>`을(를) 포함하도록 SPA이 업데이트되고 3개의 AEM React Core 구성 요소(`AEMTitle`, `AEMText` 및 `AEMImage`)에 대한 래퍼가 업데이트되고 일치하는 템플릿 정책으로 AEM이 업데이트되면 컨테이너 구성 요소에서 컨텐츠 작성을 시작할 수 있습니다.

1. AEM 작성자 로그인
1. __사이트 > WKND 앱__&#x200B;으로 이동합니다.
1. __Home__&#x200B;을 누르고 위쪽 작업 표시줄에서 __편집__&#x200B;을 선택합니다.
   + AEM 프로젝트 원형에서 프로젝트를 생성할 때 자동으로 추가되므로 &quot;Hello World&quot; 텍스트 구성 요소가 표시됩니다.
1. 페이지 편집기의 오른쪽 상단에 있는 모드 선택기에서 __편집__&#x200B;을 선택합니다.
1. 제목 아래에서 __레이아웃 컨테이너__ 편집 가능 영역을 찾습니다.
1. __페이지 편집기의 측면 막대__&#x200B;를 열고 __구성 요소 보기__&#x200B;를 선택합니다.
1. 다음 구성 요소를 __레이아웃 컨테이너__&#x200B;로 드래그합니다.
   + 이미지
   + 제목
1. 구성 요소를 드래그하여 다음 순서로 순서를 변경합니다.
   1. 제목
   1. 이미지
   1. 텍스트
1. __제작__ 제목  ____ 구성 요소
   1. 제목 구성 요소를 누르고 __렌치__ 아이콘을 눌러 __제목 구성 요소 편집__&#x200B;합니다.
   1. 다음 텍스트를 추가합니다.
      + 제목:__여름이 다가온다, 최대한 활용하자!__
      + 유형:__H1__
   1. __Done__&#x200B;을 누릅니다
1. ____ Imagecomponent  ____ 제작
   1. 이미지 구성 요소의 [사이드] 막대(자산 보기로 전환한 후)에서 이미지를 드래그합니다
   1. 이미지 구성 요소를 누르고 __렌치__ 아이콘을 눌러 편집합니다.
   1. __이미지가 장식용임__ 확인란을 선택합니다.
   1. __Done__&#x200B;을 누릅니다
1. __텍스트__ 구성  ____ 요소 제작
   1. 텍스트 구성 요소를 탭하고 __렌치__ 아이콘을 눌러 텍스트 구성 요소를 편집합니다.
   1. 다음 텍스트를 추가합니다.
      + _지금 당장, 여러분은 1주간의 모든 모험에 15퍼센트 그리고 2주 이상의 모든 모험에 20퍼센트 할인을 받을 수 있습니다! 체크아웃 시 할인 혜택을 받으려면 캠페인 코드 SUMMERISCOMING을 추가하면 됩니다!_
   1. __Done__&#x200B;을 누릅니다

1. 이제 구성 요소를 제작했지만 세로로 스택할 수 있습니다.

   ![제작된 구성 요소](./assets/spa-container-component/authored-components.png)

   AEM 레이아웃 모드를 사용하여 구성 요소의 크기와 레이아웃을 조정할 수 있습니다.

1. 오른쪽 상단의 모드 선택기를 사용하여 __레이아웃 모드__&#x200B;로 전환
1. __이미지__ 및 텍스트 구성 요소의 크기를 조정하여 옆에 나란히 놓기
   + __이미지__ 구성 요소는  __8열 너비여야 합니다.__
   + __텍스트__ 구성 요소는  __3열 너비여야 합니다.__

   ![레이아웃 구성 요소](./assets/spa-container-component/layout-components.png)

1. __AEM__ 페이지 편집기에서 변경 내용 미리 보기
1. [http://localhost:3000](http://localhost:3000)에서 로컬로 실행 중인 WKND 앱을 새로 고쳐 만든 변경 사항을 확인합니다!

   ![SPA의 컨테이너 구성 요소](./assets/spa-container-component/localhost-final.png)


## 축하합니다!

작성자가 편집 가능한 구성 요소를 WKND 앱에 추가할 수 있는 컨테이너 구성 요소를 추가했습니다! 이제 다음과 같은 방법을 알 수 있습니다.

+ SPA에서 AEM React Editable 구성 요소의 ResponsiveGrid 구성 요소 사용
+ 컨테이너 구성 요소를 통해 SPA에서 사용할 수 있도록 AEM의 반응형 핵심 구성 요소(텍스트 및 이미지)를 등록합니다.
+ SPA 지원 핵심 구성 요소를 허용하도록 원격 SPA 페이지 템플릿을 구성합니다.
+ 컨테이너 구성 요소에 편집 가능한 구성 요소 추가
+ SPA Editor의 작성자 및 레이아웃 구성 요소

## 다음 단계

다음 단계는 SPA의 [Adventure Details 경로](./spa-dynamic-routes.md)에 편집 가능한 구성 요소를 추가하는 것과 동일한 기술을 사용합니다.
