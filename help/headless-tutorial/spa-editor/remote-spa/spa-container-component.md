---
title: 원격 SPA에 편집 가능한 컨테이너 구성 요소 추가
description: AEM 작성자가 구성 요소를 드래그하여 놓을 수 있는 원격 SPA에 편집 가능한 컨테이너 구성 요소를 추가하는 방법을 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 2%

---

# 편집 가능한 컨테이너 구성 요소

[고정 구성 요소](./spa-fixed-component.md) SPA 컨텐츠를 작성하는 데 약간의 유연성을 제공할 수 있지만, 이 접근 방식은 까다롭고 개발자가 편집 가능한 컨텐츠의 정확한 구성을 정의해야 합니다. 작성자에 의한 예외적인 경험 만들기를 지원하기 위해 SPA 편집기는 SPA에서 컨테이너 구성 요소의 사용을 지원합니다. 컨테이너 구성 요소를 사용하면 작성자가 기존의 AEM Sites 작성에서 하듯이 허용된 구성 요소를 컨테이너에 드래그하여 놓고 작성할 수 있습니다.

![편집 가능한 컨테이너 구성 요소](./assets/spa-container-component/intro.png)

이 장에서는 SPA에서 직접 AEM React 코어 구성 요소를 사용하여 리치 컨텐츠 경험을 작성하고 레이아웃할 수 있는 편집 가능한 컨테이너를 홈 보기에 추가합니다.

## WKND 앱 업데이트

홈 보기에 컨테이너 구성 요소를 추가하려면

+ AEM React 편집 가능한 구성 요소의 ResponsiveGrid 구성 요소 가져오기
+ 컨테이너 구성 요소에서 사용할 AEM React 코어 구성 요소(텍스트 및 이미지)를 가져오고 등록합니다

### ResponsiveGrid 컨테이너 구성 요소에서 가져오기

편집 가능한 영역을 홈 보기에 배치하려면 다음을 수행해야 합니다.

1. ResponsiveGrid 구성 요소를 가져올 위치 `@adobe/aem-react-editable-components`
1. 다음을 사용하여 등록 `withMappable` 개발자가 SPA에 배치할 수 있도록
1. 또한 `MapTo` 따라서 다른 컨테이너 구성 요소에서 재사용할 수 있으므로 컨테이너를 효과적으로 중첩합니다.

이를 위해 진행되는 작업:

1. IDE에서 SPA 프로젝트를 엽니다.
1. 에서 React 구성 요소 만들기 `src/components/aem/AEMResponsiveGrid.js`
1. 다음 코드를 `AEMResponsiveGrid.js`

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

코드는 유사합니다 `AEMTitle.js` 그게 [AEM Reach 코어 구성 요소의 제목 구성 요소를 가져왔습니다](./spa-fixed-component.md).


다음 `AEMResponsiveGrid.js` 파일 형식은 다음과 같습니다.

![AEMRresponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### AEMRresponsiveGrid SPA 구성 요소 사용

이제 AEM ResponsiveGrid 구성 요소가에 등록되었으며 SPA 내에서 사용할 수 있으므로 홈 보기에 배치할 수 있습니다.

1. 열기 및 편집 `react-app/src/Home.js`
1. 가져오기 `AEMResponsiveGrid` 구성 요소를 위에 배치하여 `<AEMTitle ...>` 구성 요소.
1. 에서 다음 속성을 설정합니다. `<AEMResponsiveGrid...>` 구성 요소
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   이를 통해 `AEMResponsiveGrid` AEM 리소스에서 컨텐츠를 검색하는 구성 요소입니다.

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   다음 `itemPath` 지도 `responsivegrid` 에 정의된 노드 `Remote SPA Page` AEM 템플릿 및 은(는) `Remote SPA Page` AEM 템플릿.

   업데이트 `Home.js` 를 추가하려면 `<AEMResponsiveGrid...>` 구성 요소.

   ```
   ...
   import AEMResponsiveGrid from './aem/AEMResponsiveGrid';
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

다음 `Home.js` 파일 형식은 다음과 같습니다.

![Home.js](./assets/spa-container-component/home-js.png)

## 편집 가능한 구성 요소 만들기

SPA Editor에서 제공하는 유연한 작성 경험 컨테이너의 전체 효과를 얻을 수 있습니다. 이미 편집 가능한 제목 구성 요소를 만들었지만 작성자가 새로 추가된 컨테이너 구성 요소에서 텍스트 및 이미지 AEM WCM 핵심 구성 요소를 사용할 수 있도록 몇 가지 더 추가하겠습니다.

### 텍스트 구성 요소

1. IDE에서 SPA 프로젝트를 엽니다.
1. 에서 React 구성 요소 만들기 `src/components/aem/AEMText.js`
1. 다음 코드를 `AEMText.js`

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

다음 `AEMText.js` 파일 형식은 다음과 같습니다.

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### 이미지 구성 요소

1. IDE에서 SPA 프로젝트를 엽니다.
1. 에서 React 구성 요소 만들기 `src/components/aem/AEMImage.js`
1. 다음 코드를 `AEMImage.js`

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

1. SCSS 파일 만들기 `src/components/aem/AEMImage.scss` 에 대한 사용자 지정 스타일을 제공합니다 `AEMImage.scss`. 이러한 스타일은 AEM React 코어 구성 요소의 BEM 표기법 CSS 클래스를 타깃팅합니다.
1. 다음 SCSS를에 추가합니다 `AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 가져오기 `AEMImage.scss` in `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

다음 `AEMImage.js` 및 `AEMImage.scss` 다음과 같이 표시됩니다.

![AEMImage.js 및 AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### 편집 가능한 구성 요소 가져오기

새로 만든 `AEMText` 및 `AEMImage` SPA 구성 요소는 SPA에서 참조되며, AEM에서 반환되는 JSON을 기반으로 동적으로 인스턴스화됩니다. 이러한 구성 요소를 SPA에서 사용할 수 있도록 하려면,에서 해당 구성 요소에 대한 가져오기 구문을 만듭니다. `Home.js`

1. IDE에서 SPA 프로젝트를 엽니다.
1. 파일을 엽니다. `src/Home.js`
1. 에 대한 가져오기 구문 추가 `AEMText` 및 `AEMImage`

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


결과는 다음과 같습니다.

![Home.js](./assets/spa-container-component/home-js-imports.png)

이러한 가져오기가 _not_ 추가됨, `AEMText` 및 `AEMImage` 코드가 SPA에 의해 호출되지 않으므로 제공된 리소스 유형에 대해 구성 요소가 등록되지 않습니다.

## AEM에서 컨테이너 구성

AEM 컨테이너 구성 요소는 정책을 사용하여 허용된 구성 요소를 나타냅니다. SPA 구성 요소 옆에 매핑된 AEM WCM 코어 구성 요소만 SPA에서 렌더링할 수 있으므로 SPA 편집기를 사용할 때 중요한 구성입니다. 에 SPA 구현을 제공한 구성 요소만 허용됩니다.

+ `AEMTitle` 매핑된 대상 `wknd-app/components/title`
+ `AEMText` 매핑된 대상 `wknd-app/components/text`
+ `AEMImage` 매핑된 대상 `wknd-app/components/image`

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

SPA이 업데이트되어 가 포함된 후 `<AEMResponsiveGrid...>`, 3개의 AEM React 코어 구성 요소에 대한 래퍼(`AEMTitle`, `AEMText`, 및 `AEMImage`)이고 AEM이 일치하는 템플릿 정책으로 업데이트되므로 컨테이너 구성 요소에서 컨텐츠 작성을 시작할 수 있습니다.

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

+ SPA에서 AEM React 편집 가능한 구성 요소의 ResponsiveGrid 구성 요소를 사용합니다
+ 컨테이너 구성 요소를 통해 SPA에서 사용할 AEM React 코어 구성 요소(텍스트 및 이미지)를 등록합니다
+ SPA이 활성화된 핵심 구성 요소를 허용하도록 원격 SPA 페이지 템플릿을 구성합니다
+ 컨테이너 구성 요소에 편집 가능한 구성 요소 추가
+ SPA 편집기의 작성자 및 레이아웃 구성 요소

## 다음 단계

다음 단계에서는 다음과 같은 기술을 사용하여 [Adventure Details 경로에 편집 가능한 구성 요소 추가](./spa-dynamic-routes.md) SPA에서 확인하십시오.
