---
title: AEM 구성 요소에 SPA 구성 요소 매핑 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 React 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 기존 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다. 또한 최신 AEM React 코어 구성 요소를 사용하는 방법도 알아봅니다.
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2256'
ht-degree: 1%

---

# AEM 구성 요소에 SPA 구성 요소 매핑 {#map-components}

AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 React 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 기존 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.

이 장에서는 AEM JSON 모델 API와 AEM 구성 요소에 의해 노출된 JSON 컨텐츠를 prop으로 React 구성 요소에 자동으로 삽입할 수 있는 방법에 대해 자세히 살펴봅니다.

## 목표

1. AEM 구성 요소를 SPA 구성 요소에 매핑하는 방법을 알아봅니다.
1. Inspect에서 React 구성 요소가 AEM에서 전달된 동적 속성을 사용하는 방법입니다.
1. 즉시 사용 가능한 방법 알아보기 [React AEM 코어 구성 요소](https://github.com/adobe/aem-react-core-wcm-components-examples).

## 빌드할 내용

이 장에서는 제공되는 방법을 검사합니다 `Text` SPA 구성 요소가 AEM에 매핑됩니다 `Text`구성 요소. 다음과 같은 React 코어 구성 요소 `Image` SPA 구성 요소는 SPA에서 사용되고 AEM에서 작성됩니다. 기본 제공되는 **레이아웃 컨테이너** 및 **템플릿 편집기** 모양새가 조금씩 다르다.

![장 샘플 최종 작성](./assets/map-components/final-page.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment). 이 장은 ...의 연속이다 [SPA 통합](integrate-spa.md) 이 장에서는 SPA을 사용할 수 있는 AEM 프로젝트를 설명합니다.

## 매핑 방법

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. AEM 구성 요소를 실행하고, 서버측을 실행하고, JSON 모델 API의 일부로 컨텐츠를 내보냅니다. JSON 콘텐츠는 브라우저에서 클라이언트측을 실행하는 SPA에 의해 사용됩니다. SPA 구성 요소와 AEM 구성 요소 간의 1:1 매핑이 만들어집니다.

![AEM 구성 요소를 React 구성 요소에 매핑하는 것에 대한 개요](./assets/map-components/high-level-approach.png)

*AEM 구성 요소를 React 구성 요소에 매핑하는 것에 대한 개요*

## 텍스트 구성 요소의 Inspect

다음 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 제공 `Text` AEM에 매핑된 구성 요소 [텍스트 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 이것은 **콘텐츠** 구성 요소를 렌더링합니다. *콘텐츠* AEM에서 가져옵니다.

구성 요소가 어떻게 작동하는지 살펴보겠습니다.

### JSON 모델을 Inspect 합니다.

1. SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 이해하는 것이 중요합니다. 로 이동합니다 [코어 구성 요소 라이브러리](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 텍스트 구성 요소의 페이지를 확인합니다. 코어 구성 요소 라이브러리는 모든 AEM 코어 구성 요소의 예를 제공합니다.
1. 을(를) 선택합니다 **JSON** 탭 - 예제 중 하나에 대해

   ![텍스트 JSON 모델](./assets/map-components/text-json.png)

   다음 세 가지 속성이 표시됩니다. `text`, `richText`, 및 `:type`.

   `:type` 는 `sling:resourceType` AEM 구성 요소의 (또는 경로). 다음 값 `:type` 는 AEM 구성 요소를 SPA 구성 요소에 매핑하는 데 사용되는 것입니다.

   `text` 및 `richText` SPA 구성 요소에 노출되는 추가 속성입니다.

1. 에서 JSON 출력을 봅니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 다음과 유사한 항목을 찾을 수 있습니다.

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### 텍스트 SPA 구성 요소의 Inspect

1. 선택한 IDE에서 SPA용 AEM 프로젝트를 엽니다. 를 확장합니다. `ui.frontend` 모듈 및 파일 열기 `Text.js` 아래에 `ui.frontend/src/components/Text/Text.js`.

1. 우리가 검사할 첫 번째 영역은 `class Text` ~40행:

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

   `Text` 는 표준 React 구성 요소입니다. 구성 요소는 을 사용합니다 `this.props.richText` 렌더링할 컨텐츠가 리치 텍스트인지 아니면 일반 텍스트인지를 판별하는 데 사용됩니다. 사용되는 실제 &quot;컨텐츠&quot;는 `this.props.text`.

   잠재적인 XSS 공격을 방지하기 위해 서식 있는 텍스트는 을 통해 이스케이프 처리합니다 `DOMPurify` 사용 전 [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 컨텐츠를 렌더링합니다. 다음 사항을 기억하십시오 `richText` 및 `text` 연습의 앞부분에서 JSON 모델의 속성을 보여줍니다.

1. 다음에 다음을 보십시오 `TextEditConfig` ~29행:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   위의 코드는 AEM 작성 환경에서 자리 표시자를 렌더링할 시기를 결정합니다. 만약 `isEmpty` 메서드 반환 **true** 그러면 자리 표시자가 렌더링됩니다.

1. 마지막으로 `MapTo` ~line 62에서 호출:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 는 AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`). 경로 `wknd-spa-react/components/text` 는 를 나타냅니다 `sling:resourceType` AEM 구성 요소의 일부입니다. 이 경로는 `:type` 이전에 살펴본 JSON 모델에 의해 노출됩니다. `MapTo` 에서는 JSON 모델 응답을 구문 분석하고 올바른 값을 다음으로 전달합니다 `props` SPA 구성 요소에 매핑됩니다.

   AEM을 찾을 수 있습니다 `Text` 구성 요소 정의 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## React 코어 구성 요소 사용

[AEM WCM 구성 요소 - React Core 구현](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM WCM 구성 요소 - Spa 편집기 - React Core 구현](https://github.com/adobe/aem-react-core-wcm-components-spa). 이는 즉시 사용 가능한 AEM 구성 요소에 매핑되는 재사용 가능한 UI 구성 요소 세트입니다. 대부분의 프로젝트는 이러한 구성 요소를 자체 구현의 시작점으로 다시 사용할 수 있습니다.

1. 프로젝트 코드에서 파일을 엽니다 `import-components.js` at `ui.frontend/src/components`.
이 파일은 AEM 구성 요소에 매핑되는 모든 SPA 구성 요소를 가져옵니다. SPA 편집기 구현의 동적 특성을 고려할 때, AEM 작성 가능한 구성 요소에 연결된 모든 SPA 구성 요소를 명시적으로 참조해야 합니다. 이렇게 하면 AEM 작성자는 애플리케이션에서 원하는 위치에 구성 요소를 사용할 수 있습니다.
1. 다음 가져오기 구문에는 프로젝트에 작성된 SPA 구성 요소가 포함됩니다.

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. 다른 몇 가지 있습니다 `imports` 변환 전: `@adobe/aem-core-components-react-spa` 및 `@adobe/aem-core-components-react-base`. React 코어 구성 요소를 가져와 현재 프로젝트에서 사용할 수 있도록 하는 것입니다. 그런 다음 를 사용하여 프로젝트별 AEM 구성 요소에 매핑됩니다 `MapTo`와 마찬가지로 `Text` 구성 요소 예제 를 먼저 참조하십시오.

### AEM 정책 업데이트

정책은 AEM 템플릿의 기능으로 개발자와 고급 사용자가 사용할 수 있는 구성 요소를 세부적으로 제어할 수 있습니다. React 코어 구성 요소는 SPA 코드에 포함되어 있지만, 애플리케이션에서 사용하려면 정책을 통해 활성화해야 합니다.

1. AEM 시작 화면에서 로 이동합니다. **도구** > **템플릿** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. 을(를) 선택하고 엽니다. **SPA 페이지** 편집할 템플릿입니다.

1. 을(를) 선택합니다 **레이아웃 컨테이너** 그리고 **정책** 아이콘을 사용하여 정책을 편집합니다.

   ![레이아웃 컨테이너 정책](assets/map-components/edit-spa-page-template.png)

1. 아래 **허용된 구성 요소** > **WKND SPA React - 컨텐츠** > check **이미지**, **티저**, 및 **제목**.

   ![업데이트된 구성 요소 사용 가능](assets/map-components/update-components-available.png)

   아래 **기본 구성 요소** > **매핑 추가** 그리고 **이미지 - WKND SPA React - 컨텐츠** 구성 요소:

   ![기본 구성 요소 설정](./assets/map-components/default-components.png)

   을(를) 입력합니다. **mime 유형** 의 `image/*`.

   클릭 **완료** 정책 업데이트를 저장하려면

1. 에서 **레이아웃 컨테이너** 를 클릭합니다. **정책** 아이콘 **텍스트** 구성 요소.

   이름이 인 새 정책 만들기 **WKND SPA 텍스트**. 아래 **Plugins** > **서식** > 모든 상자를 선택하여 추가 서식 옵션을 활성화합니다.

   ![RTE 서식 사용](assets/map-components/enable-formatting-rte.png)

   아래 **Plugins** > **단락 스타일** > 확인란을 선택합니다. **단락 스타일 활성화**:

   ![단락 스타일 사용](./assets/map-components/text-policy-enable-paragraphstyles.png)

   클릭 **완료** 정책 업데이트를 저장하려면

### 컨텐츠 작성

1. 로 이동합니다 **홈페이지** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. 이제 추가 구성 요소를 사용할 수 있습니다 **이미지**, **티저**, 및 **제목** 클릭합니다.

   ![추가 구성 요소](assets/map-components/additional-components.png)

1. 또한 `Text` 구성 요소 및 추가 단락 스타일 추가 **전체 화면** 모드.

   ![전체 화면 리치 텍스트 편집](assets/map-components/full-screen-rte.png)

1. 이미지를 **자산 파인더**:

   ![이미지 드래그 앤 드롭](assets/map-components/drag-drop-image.png)

1. 을 사용한 경험 **제목** 및 **티저** 구성 요소.

1. 를 통해 자체 이미지 추가 [AEM Assets](http://localhost:4502/assets.html/content/dam) 또는 standard용 완료된 코드 베이스를 설치하거나 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest). 다음 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 에는 WKND SPA에서 다시 사용할 수 있는 많은 이미지가 포함되어 있습니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

## 레이아웃 컨테이너 Inspect

지원 대상 **레이아웃 컨테이너** 는 AEM SPA Editor SDK에서 자동으로 제공됩니다. 다음 **레이아웃 컨테이너**&#x200B;로 표시된 대로 는 입니다. **컨테이너** 구성 요소. 컨테이너 구성 요소는 다음을 나타내는 JSON 구조를 허용하는 구성 요소입니다 *기타* 구성 요소를 동적으로 인스턴스화하고 생성할 수 있습니다.

레이아웃 컨테이너를 더 살펴보겠습니다.

1. 브라우저에서 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON 모델 API - 응답형 그리드](./assets/map-components/responsive-grid-modeljson.png)

   다음 **레이아웃 컨테이너** 구성 요소에 `sling:resourceType` 의 `wcm/foundation/components/responsivegrid` 및 는 `:type` 속성, `Text` 및 `Image` 구성 요소.

   구성 요소를 사용하여 다시 크기 조정하는 동일한 기능 [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) SPA 편집기에서 사용할 수 있습니다.

2. 로 돌아가기 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 추가 **이미지** 구성 요소를 사용하여 크기 조정을 다시 시도하십시오 **레이아웃** 옵션:

   ![레이아웃 모드를 사용한 이미지 크기 조정](./assets/map-components/responsive-grid-layout-change.gif)

3. JSON 모델 다시 열기 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 그리고 `columnClassNames` json의 일부로:

   ![열 클래스 이름](./assets/map-components/responsive-grid-classnames.png)

   클래스 이름 `aem-GridColumn--default--4` 구성 요소가 12열 격자를 기반으로 하여 4열 너비여야 함을 나타냅니다. 에 대한 자세한 내용 [응답형 그리드는 여기에서 찾을 수 있습니다.](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. IDE로 돌아가서 `ui.apps` 모듈에는 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. 파일을 엽니다. `less/grid.less`.

   이 파일은 중단점(`default`, `tablet`, 및 `phone`)에서 사용할 수 있습니다. **레이아웃 컨테이너**. 이 파일은 프로젝트 사양에 따라 사용자 지정되기 위한 것입니다. 현재 중단점이 `1200px` 및 `768px`.

5. 반응형 기능과 업데이트된 리치 텍스트 정책을 `Text` 구성 요소를 사용하여 다음과 같은 보기를 작성합니다.

   ![장 샘플 최종 작성](assets/map-components/final-page.png)

## 축하합니다! {#congratulations}

축하합니다. SPA 구성 요소를 AEM 구성 요소에 매핑하는 방법을 배우고 React 코어 구성 요소를 사용했습니다. 또한, **레이아웃 컨테이너**.

### 다음 단계 {#next-steps}

[탐색 및 라우팅](navigation-routing.md) - SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 React Router 및 React Core 구성 요소를 사용하여 구현됩니다.

## (보너스) 소스 제어에 구성을 유지합니다. {#bonus-configs}

대부분의 경우, 특히 AEM 프로젝트 시작 시 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 컨텐츠 및 구성 세트에 대해 작업하고 있으므로 환경 간에 추가적인 일관성을 유지할 수 있습니다. 프로젝트가 일정 수준의 성숙기에 도달하면 템플릿 관리 방법을 특수 사용자 그룹으로 전환할 수 있습니다.

다음 몇 단계는 Visual Studio 코드 IDE를 사용하여 수행됩니다. [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 그러나 사용자가 구성했던 모든 도구 및 IDE를 사용하여 **가져오기** 또는 **가져오기** AEM의 로컬 인스턴스에 있는 컨텐츠입니다.

1. Visual Studio 코드 IDE에서 **VSCode AEM 동기화** marketplace 확장을 통해 설치:

   ![VSCode AEM 동기화](./assets/map-components/vscode-aem-sync.png)

2. 를 확장합니다. **ui.content** 프로젝트 탐색기에서 `/conf/wknd-spa-react/settings/wcm/templates`.

3. **마우스 오른쪽 단추 클릭** a `templates` 폴더를 선택하고 **AEM 서버에서 가져오기**:

   ![VSCode 가져오기 템플릿](./assets/map-components/import-aem-servervscode.png)

4. 단계를 반복하여 컨텐츠를 가져오지만 **정책** 폴더 위치 `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect `filter.xml` 에 있는 파일 `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   다음 `filter.xml` 파일은 패키지와 함께 설치된 노드의 경로를 식별해야 합니다. 다음 사항에 주의하십시오. `mode="merge"` 기존 컨텐츠가 수정되지 않음을 나타내는 각 필터에 새 컨텐츠만 추가됩니다. 컨텐츠 작성자는 이러한 경로를 업데이트할 수 있으므로 코드 배포가 수행하는 것이 중요합니다 **not** 콘텐츠를 덮어씁니다. 자세한 내용은 [FileVault 설명서](https://jackrabbit.apache.org/filevault/filter.html) 를 참조하십시오.

   비교 `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml` 각 모듈에서 관리하는 서로 다른 노드를 이해하기 위해

## (보너스) 사용자 지정 이미지 구성 요소 만들기 {#bonus-image}

React Core 구성 요소에서 이미 SPA 이미지 구성 요소를 제공했습니다. 하지만 추가 작업이 필요한 경우 AEM에 매핑되는 자체 React 구현을 만드십시오 [이미지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 다음 `Image` 구성 요소는 **콘텐츠** 구성 요소.

### JSON의 Inspect

SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 검사하십시오.

1. 로 이동합니다 [코어 구성 요소 라이브러리의 이미지 예](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![이미지 코어 구성 요소 JSON](./assets/map-components/image-json.png)

   속성 `src`, `alt`, 및 `title` 는 SPA을 채우는 데 사용됩니다 `Image` 구성 요소.

   >[!NOTE]
   >
   > 다른 이미지 속성이 노출되어 있습니다(`lazyEnabled`, `widths`)을 클릭하여 개발자가 적응형 및 지연 로드 구성 요소를 만들 수 있습니다. 이 자습서에 포함된 구성 요소는 간단하며 수행합니다 **not** 이러한 고급 속성을 사용합니다.

### 이미지 구성 요소 구현

1. 그런 다음 이름이 인 새 폴더를 만듭니다 `Image` 아래에 `ui.frontend/src/components`.
1. 아래 `Image` 폴더 이름이 인 새 파일 만들기 `Image.js`.

   ![Image.js 파일](./assets/map-components/image-js-file.png)

1. 다음을 추가합니다 `import` 문 `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 그런 다음 `ImageEditConfig` AEM에서 자리 표시자를 표시할 시기를 결정하려면:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   자리 표시자가 `src` 속성이 설정되지 않았습니다.

1. 다음: `Image` 클래스:

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

   위의 코드는 `<img>` prop에 따라 `src`, `alt`, 및 `title` json 모델에 의해 전달됩니다.

1. 추가 `MapTo` React 구성 요소를 AEM 구성 요소에 매핑하기 위한 코드:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   문자열을 확인합니다 `wknd-spa-react/components/image` 는 의 AEM 구성 요소 위치에 해당합니다. `ui.apps` at: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. 이름이 인 새 파일 만들기 `Image.css` 동일한 디렉토리에 다음을 추가합니다.

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. in `Image.js` 파일의 `import` 문:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. 파일을 엽니다. `ui.frontend/src/components/import-components.js` 새 페이지에 대한 참조를 추가합니다 `Image` 구성 요소:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. in `import-components.js` React Core Component 이미지에 주석을 추가합니다.

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   이렇게 하면 사용자 지정 이미지 구성 요소가 대신 사용됩니다.

1. 프로젝트의 루트에서 Maven을 사용하여 SPA 코드를 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEM에서 SPA을 Inspect에 추가합니다. 페이지의 모든 이미지 구성 요소는 계속 작동해야 합니다. Inspect 렌더링된 출력은 React Core 구성 요소 대신 사용자 지정 이미지 구성 요소에 대한 마크업이 표시됩니다.

   *사용자 지정 이미지 구성 요소 마크업*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *React 코어 구성 요소 이미지 마크업*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   자체 구성 요소를 확장하고 구현하는 데 유용한 소개입니다.
