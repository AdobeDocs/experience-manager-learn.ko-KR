---
title: 탐색 및 라우팅 추가 | AEM SPA 편집기 및 반응 시작하기
description: SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 React Router 및 React Core 구성 요소를 사용하여 구현됩니다.
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# 탐색 및 라우팅 추가 {#navigation-routing}

SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 React Router 및 React Core 구성 요소를 사용하여 구현됩니다.

## 목표

1. SPA 편집기를 사용할 때 사용할 수 있는 SPA 모델 라우팅 옵션을 이해합니다.
1. 사용 방법 알아보기 [React 라우터](https://reacttraining.com/react-router/) SPA의 다른 보기 사이를 탐색합니다.
1. AEM React 코어 구성 요소 를 사용하여 AEM 페이지 계층 구조에 의해 구동되는 동적 탐색을 구현합니다.

## 빌드할 내용

이 장에서는 AEM의 SPA에 탐색을 추가합니다. 탐색 메뉴는 AEM 페이지 계층 구조에 의해 제어되며, 가 제공하는 JSON 모델을 사용합니다. [탐색 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![탐색 추가](assets/navigation-routing/navigation-added.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment). 이 장은 ...의 연속이다 [구성 요소 매핑](map-components.md) 그러나 필요한 모든 작업을 수행하려면 로컬 AEM 인스턴스에 배포된 SPA 지원 AEM 프로젝트가 있습니다.

## 템플릿에 탐색 추가 {#add-navigation-template}

1. 브라우저를 열고 AEM에 로그인합니다. [http://localhost:4502/](http://localhost:4502/). 시작 코드 베이스를 이미 배포해야 합니다.
1. 로 이동합니다 **SPA 페이지 템플릿**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 가장 바깥쪽을 선택합니다 **루트 레이아웃 컨테이너** 및 **정책** 아이콘. 조심해 **not** 을(를) 선택하려면 **레이아웃 컨테이너** 작성을 위해 잠금이 해제되었습니다.

   ![루트 레이아웃 컨테이너 정책 아이콘을 선택합니다](assets/navigation-routing/root-layout-container-policy.png)

1. 이름이 인 새 정책 만들기 **SPA 구조**:

   ![SPA 구조 정책](assets/navigation-routing/spa-policy-update.png)

   아래 **허용된 구성 요소** > **일반** > 를 선택합니다. **레이아웃 컨테이너** 구성 요소.

   아래 **허용된 구성 요소** > **WKND SPA REACT - 구조** > 를 선택합니다. **탐색** 구성 요소:

   ![탐색 구성 요소 선택](assets/navigation-routing/select-navigation-component.png)

   아래 **허용된 구성 요소** > **WKND SPA REACT - 컨텐츠** > 를 선택합니다. **이미지** 및 **텍스트** 구성 요소. 총 4개의 구성 요소가 선택되어 있어야 합니다.

   클릭 **완료** 변경 사항을 저장하려면 을 클릭합니다.

1. 페이지를 새로 고침하고 **탐색** 잠기지 않은 구성 요소 위에 있는 구성 요소 **레이아웃 컨테이너**:

   ![템플릿에 탐색 구성 요소 추가](assets/navigation-routing/add-navigation-component.png)

1. 을(를) 선택합니다 **탐색** 구성 요소를 클릭한 다음 **정책** 아이콘을 클릭하여 정책을 편집합니다.
1. 를 사용하여 새 정책 만들기 **정책 제목** 의 **SPA 탐색**.

   아래에 **속성**:

   * 설정 **탐색 루트** to `/content/wknd-spa-react/us/en`.
   * 설정 **루트 수준 제외** to **1**.
   * 선택을 취소합니다 **모든 하위 페이지 수집**.
   * 설정 **탐색 구조 깊이** to **3**.

   ![탐색 정책 구성](assets/navigation-routing/navigation-policy.png)

   이렇게 하면 아래의 탐색 2단계가 수집됩니다 `/content/wknd-spa-react/us/en`.

1. 변경 사항을 저장한 후에는 채워진 가 표시됩니다 `Navigation` 템플릿의 일부로:

   ![채워진 탐색 구성 요소](assets/navigation-routing/populated-navigation.png)

## 하위 페이지 만들기

다음으로, SPA에서 다른 보기 역할을 하는 AEM에서 추가 페이지를 만듭니다. 또한 AEM에서 제공하는 JSON 모델의 계층 구조도 검사합니다.

1. 로 이동합니다 **Sites** 콘솔: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 을(를) 선택합니다 **WKND SPA React 홈 페이지** 을(를) 클릭합니다. **만들기** > **페이지**:

   ![새 페이지 만들기](assets/navigation-routing/create-new-page.png)

1. 아래 **템플릿** 선택 **SPA 페이지**. 아래 **속성** enter **1페이지** 대상 **제목** 및 **page-1** 를 입력합니다.

   ![초기 페이지 속성 입력](assets/navigation-routing/initial-page-properties.png)

   클릭 **만들기** 대화 상자 팝업에서 **열기** 를 클릭하여 AEM SPA 편집기에서 페이지를 엽니다.

1. 새 추가 **텍스트** 기본 구성 요소 **레이아웃 컨테이너**. 구성 요소를 편집하고 텍스트를 입력합니다. **1페이지** RTE와 **H2** 요소를 생성하지 않습니다.

   ![샘플 컨텐츠 페이지 1](assets/navigation-routing/page-1-sample-content.png)

   이미지와 같은 추가 컨텐츠를 자유롭게 추가할 수 있습니다.

1. AEM Sites 콘솔으로 돌아가서 위의 절차를 반복하고 이름이 인 두 번째 페이지를 만듭니다. **2페이지** 형제로서 **1페이지**.
1. 마지막으로 세 번째 페이지를 만들고 **3페이지** 하지만 **하위** 의 **2페이지**. 사이트 계층 구조가 완료되면 다음과 같이 표시됩니다.

   ![샘플 사이트 계층](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 이제 탐색 구성 요소를 사용하여 SPA의 다른 영역으로 이동할 수 있습니다.

   ![탐색 및 라우팅](assets/navigation-routing/navigation-working.gif)

1. AEM 편집기 외부에서 페이지를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 를 사용하십시오 **탐색** 구성 요소를 사용하여 앱의 다른 보기로 이동합니다.

1. 탐색할 때 브라우저의 개발자 도구를 사용하여 네트워크 요청을 검사합니다. 아래 스크린샷은 Google Chrome 브라우저에서 촬영됩니다.

   ![Observe Network 요청](assets/navigation-routing/inspect-network-requests.png)

   초기 페이지 로드 후 후속 탐색으로 전체 페이지 새로 고침이 발생하지 않으며 이전에 방문한 페이지로 돌아올 때 네트워크 트래픽이 최소화되는지 확인합니다.

## 계층 페이지 JSON 모델 {#hierarchy-page-json-model}

다음으로, SPA의 다중 보기 경험을 유도하는 JSON 모델을 검사합니다.

1. 새 탭에서 AEM에서 제공하는 JSON 모델 API를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 브라우저 확장을 사용하여 다음을 수행하는 것이 도움이 될 수 있습니다. [JSON 형식 지정](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   SPA이 처음 로드될 때 이 JSON 컨텐츠가 요청됩니다. 외부 구조는 다음과 같습니다.

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   아래 `:children` 만들어진 각 페이지에 대한 항목이 표시됩니다. 모든 페이지의 컨텐츠는 이 초기 JSON 요청에 있습니다. 탐색 라우팅에서는 컨텐츠를 이미 클라이언트측에서 사용할 수 있으므로 SPA의 후속 보기가 빠르게 로드됩니다.

   적재하는 것은 현명하지 않다 **모두** 초기 JSON 요청에 있는 SPA의 컨텐츠가 처리되면 초기 페이지 로드 속도가 느려집니다. 다음으로, 페이지의 계층 구조 깊이를 수집하는 방법을 살펴보겠습니다.

1. 로 이동합니다 **SPA 루트** 템플릿 위치: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   을(를) 클릭합니다. **페이지 속성 메뉴** > **페이지 정책**:

   ![SPA 루트에 대한 페이지 정책을 엽니다.](assets/navigation-routing/open-page-policy.png)

1. 다음 **SPA 루트** 템플릿에는 추가 항목이 있습니다 **계층적 구조** 수집된 JSON 컨텐츠를 제어하는 탭 다음 **구조 깊이** 에서는 사이트 계층 구조에서 하위 페이지 아래의 하위 페이지를 수집하는 깊이를 결정합니다 **루트**. 를 사용할 수도 있습니다 **구조 패턴** 정규 표현식을 기반으로 추가 페이지를 필터링하는 필드입니다.

   업데이트 **구조 깊이** to **2개**:

   ![구조 깊이 업데이트](assets/navigation-routing/update-structure-depth.png)

   클릭 **완료** 정책에 대한 변경 사항을 저장합니다.

1. JSON 모델 다시 열기 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   다음 사항에 주의하십시오. **3페이지** 경로가 제거되었습니다. `/content/wknd-spa-react/us/en/home/page-2/page-3` 기본 JSON 모델에서 입니다. 왜냐하면 **3페이지** 는 계층 구조의 수준 3에 있으며, 수준 2의 최대 깊이에 콘텐츠만 포함하도록 정책을 업데이트했습니다.

1. SPA 홈 페이지를 다시 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 브라우저의 개발자 도구를 엽니다.

   페이지를 새로 고치면 XHR 요청이 표시됩니다. `/content/wknd-spa-react/us/en.model.json`: SPA 루트입니다. 자습서에서 이전에 만든 SPA 루트 템플릿에 대한 계층 구조 깊이 구성을 기준으로 세 개의 하위 페이지만 포함됩니다. 여기에는 포함되지 않습니다 **3페이지**.

   ![초기 JSON 요청 - SPA 루트](assets/navigation-routing/initial-json-request.png)

1. 개발자 도구를 연 상태에서 `Navigation` 구성 요소를으로 바로 이동 **3페이지**:

   다음에 대한 새 XHR 요청이 수행되는지 확인합니다. `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![3페이지 XHR 요청](assets/navigation-routing/page-3-xhr-request.png)

   AEM 모델 관리자는 **3페이지** JSON 콘텐츠는 사용할 수 없으며 추가 XHR 요청을 자동으로 트리거합니다.

1. 다음으로 직접 이동하여 딥 링크를 실험합니다. [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 또한 브라우저의 뒤로 단추가 계속 작동하는 것을 확인합니다.

## Inspect React 라우팅  {#react-routing}

탐색 및 라우팅은 [React 라우터](https://reactrouter.com/). React Router는 React 응용 프로그램에 대한 탐색 구성 요소의 컬렉션입니다. [AEM React 코어 구성 요소](https://github.com/adobe/aem-react-core-wcm-components-base) 는 React 라우터의 기능을 사용하여 **탐색** 이전 단계에서 사용되는 구성 요소입니다.

다음으로 React Router가 SPA과 통합되는 방식을 검사하고 React Router의 [링크](https://reactrouter.com/web/api/Link) 구성 요소.

1. IDE에서 파일을 엽니다 `index.js` at `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   다음 사항에 주의하십시오. `App` 는 `Router` 구성 요소 [React 라우터](https://reacttraining.com/react-router/). 다음 `ModelManager`가 AEM SPA Editor JS SDK에서 제공되면 JSON 모델 API를 기반으로 AEM 페이지에 동적 경로가 추가됩니다.

1. 파일을 엽니다. `Page.js` at `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   다음 `Page` SPA 구성 요소는 `MapTo` 매핑 함수 **페이지** 를 입력합니다. 다음 `withRoute` 유틸리티는 SPA을 `cqPath` 속성을 사용합니다.

1. 를 엽니다. `Header.js` 구성 요소 위치 `ui.frontend/src/components/Header/Header.js`.
1. 업데이트 `Header` 를 `<h1>` 태그에 다음 코드 행을 추가하여 [링크](https://reactrouter.com/web/api/Link) 홈 페이지:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   기본값을 사용하는 대신 `<a>` 사용하는 앵커 태그 `<Link>` React 라우터에서 제공. As a `to=` 유효한 경로를 가리키면 SPA이 해당 경로로 전환되고 **not** 전체 페이지 새로 고침을 수행합니다. 여기서는 홈 페이지의 링크를 하드코딩하여 `Link`.

1. 에서 테스트를 업데이트합니다. `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   에 참조된 정적 구성 요소 내에서 React Router의 기능을 사용하고 있으므로 `App.js` 우리는 그것을 설명하기 위해 단위 테스트를 업데이트해야 합니다.

1. 터미널을 열고 프로젝트의 루트로 이동한 다음 Maven 기술을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEM에서 SPA 페이지 중 하나로 이동합니다. [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   를 사용하는 대신 `Navigation` 탐색할 구성 요소는 `Header`.

   ![헤더 링크](assets/navigation-routing/header-link.png)

   전체 페이지 새로 고침이 **not** 트리거되며 SPA 라우팅이 작동하는지 확인합니다.

1. 원할 경우, `Header.js` 표준을 사용한 파일 `<a>` 앵커 태그:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   이렇게 하면 SPA 라우팅과 일반 웹 페이지 링크 간의 차이를 알 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아보았습니다. 동적 탐색이 React Router를 사용하여 구현되었으며 `Header` 구성 요소.
