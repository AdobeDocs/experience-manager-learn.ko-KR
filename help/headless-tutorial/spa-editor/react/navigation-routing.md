---
title: 탐색 및 라우팅 추가 | AEM SPA 편집기 및 반응 시작하기
description: SPA Editor SDK를 사용하여 SPA 페이지에 매핑하여 AEM의 여러 보기를 지원하는 방법에 대해 알아봅니다. 동적 탐색은 React Router 및 React 핵심 구성 요소를 사용하여 구현됩니다.
feature: SPA Editor
version: Cloud Service
jira: KT-4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
duration: 437
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 0%

---

# 탐색 및 라우팅 추가 {#navigation-routing}

SPA Editor SDK를 사용하여 SPA 페이지에 매핑하여 AEM의 여러 보기를 지원하는 방법에 대해 알아봅니다. 동적 탐색은 React Router 및 React 핵심 구성 요소를 사용하여 구현됩니다.

## 목표

1. SPA Editor를 사용할 때 사용할 수 있는 SPA 모델 라우팅 옵션을 이해합니다.
1. 사용 방법 알아보기 [React 라우터](https://reacttraining.com/react-router) 를 클릭하여 SPA의 다른 보기 사이를 이동합니다.
1. AEM React 핵심 구성 요소를 사용하여 AEM 페이지 계층 구조에 의해 실행되는 동적 탐색을 구현합니다.

## 빌드할 내용

이 장에서는 AEM의 SPA에 탐색을 추가합니다. 탐색 메뉴는 AEM 페이지 계층 구조에 의해 구동되며,에서 제공하는 JSON 모델을 사용합니다. [탐색 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/navigation.html).

![탐색 추가됨](assets/navigation-routing/navigation-added.png)

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment). 이 장은 의 연속입니다. [구성 요소 매핑](map-components.md) 챕터에서는 로컬 AEM 인스턴스에 배포된 SPA 사용 AEM 프로젝트만 수행하면 됩니다.

## 템플릿에 탐색 추가 {#add-navigation-template}

1. 브라우저를 열고 AEM에 로그인합니다. [http://localhost:4502/](http://localhost:4502/). 시작 코드 베이스를 이미 배포해야 합니다.
1. 다음 위치로 이동 **SPA 페이지 템플릿**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 가장 바깥쪽 선택 **루트 레이아웃 컨테이너** 및 클릭 **정책** 아이콘. 조심해 **아님** 을(를) 선택하려면 **레이아웃 컨테이너** 작성을 위해 잠금 해제되었습니다.

   ![루트 레이아웃 컨테이너 정책 아이콘 선택](assets/navigation-routing/root-layout-container-policy.png)

1. 이름이 인 새 정책 만들기 **SPA 구조**:

   ![SPA 구조 정책](assets/navigation-routing/spa-policy-update.png)

   아래 **허용된 구성 요소** > **일반** > 다음을 선택합니다. **레이아웃 컨테이너** 구성 요소.

   아래 **허용된 구성 요소** > **WKND SPA REACT - 구조** > 다음을 선택합니다. **탐색** 구성 요소:

   ![탐색 구성 요소 선택](assets/navigation-routing/select-navigation-component.png)

   아래 **허용된 구성 요소** > **WKND SPA REACT - 콘텐츠** > 다음을 선택합니다. **이미지** 및 **텍스트** 구성 요소. 총 4개의 구성 요소를 선택해야 합니다.

   **완료**&#x200B;를 클릭하여 변경 내용을 저장합니다.

1. 페이지를 새로 고치고 **탐색** 잠금 해제됨 위의 구성 요소 **레이아웃 컨테이너**:

   ![템플릿에 탐색 구성 요소 추가](assets/navigation-routing/add-navigation-component.png)

1. 다음 항목 선택 **탐색** 구성 요소 및 클릭 **정책** 아이콘을 클릭하여 정책을 편집합니다.
1. 을(를) 사용하여 새 정책 만들기 **정책 제목** / **SPA 탐색**.

   아래 **속성**:

   * 설정 **탐색 루트** 끝 `/content/wknd-spa-react/us/en`.
   * 설정 **루트 수준 제외** 끝 **1**.
   * 선택 취소 **모든 하위 페이지 수집**.
   * 설정 **탐색 구조 깊이** 끝 **3**.

   ![탐색 정책 구성](assets/navigation-routing/navigation-policy.png)

   이렇게 하면 2단계 아래에 탐색이 수집됩니다. `/content/wknd-spa-react/us/en`.

1. 변경 사항을 저장하면 가 채워집니다. `Navigation` 템플릿의 일부로:

   ![채워진 탐색 구성 요소](assets/navigation-routing/populated-navigation.png)

## 하위 페이지 만들기

그런 다음 AEM에서 다른 보기 역할을 할 추가 페이지를 SPA에서 만듭니다. AEM에서 제공하는 JSON 모델의 계층 구조도 함께 살펴볼 것이다.

1. 다음 위치로 이동 **사이트** 콘솔: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 다음 항목 선택 **WKND SPA React 홈 페이지** 및 클릭 **만들기** > **페이지**:

   ![새 페이지 만들기](assets/navigation-routing/create-new-page.png)

1. 아래 **템플릿** 선택 **SPA 페이지**. 아래 **속성** 입력 **페이지 1** 대상: **제목** 및 **page-1** 을(를) 이름으로 사용하십시오.

   ![초기 페이지 속성 입력](assets/navigation-routing/initial-page-properties.png)

   클릭 **만들기** 대화 상자 팝업에서 **열기** 를 클릭하여 AEM SPA 편집기에서 페이지를 엽니다.

1. 새 항목 추가 **텍스트** 주 구성 요소 **레이아웃 컨테이너**. 구성 요소를 편집하고 다음 텍스트를 입력합니다. **페이지 1** rte 및 **H2** 요소를 생성하지 않습니다.

   ![샘플 콘텐츠 페이지 1](assets/navigation-routing/page-1-sample-content.png)

   이미지와 같은 콘텐츠를 자유롭게 추가할 수 있습니다.

1. AEM Sites 콘솔로 돌아가서 위의 단계를 반복하여 이라는 두 번째 페이지를 만듭니다. **페이지 2** 의 형제 **페이지 1**.
1. 마지막으로 세 번째 페이지를 만듭니다. **페이지 3** 하지만 **하위** / **페이지 2**. 사이트 계층 구조가 완료되면 다음과 같이 표시됩니다.

   ![샘플 사이트 계층](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 이제 탐색 구성 요소를 사용하여 SPA의 다양한 영역으로 이동할 수 있습니다.

   ![탐색 및 라우팅](assets/navigation-routing/navigation-working.gif)

1. AEM 편집기 외부에서 페이지를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 사용 **탐색** 앱의 다른 보기로 이동할 구성 요소입니다.

1. 탐색할 때 브라우저의 개발자 도구를 사용하여 네트워크 요청을 검사합니다. 아래 스크린샷은 Google Chrome 브라우저에서 캡처됩니다.

   ![네트워크 요청 관찰](assets/navigation-routing/inspect-network-requests.png)

   초기 페이지 로드 후 후속 탐색으로 인해 전체 페이지가 새로 고쳐지지 않고 이전에 방문한 페이지로 돌아갈 때 네트워크 트래픽이 최소화되는지 확인하십시오.

## 계층 페이지 JSON 모델 {#hierarchy-page-json-model}

다음으로 SPA의 다중 보기 경험을 구동하는 JSON 모델을 검사합니다.

1. 새 탭에서 AEM에서 제공하는 JSON 모델 API를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 브라우저 확장 프로그램을 사용하여 다음을 수행하는 것이 유용할 수 있습니다. [json 포맷](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   이 JSON 콘텐츠는 SPA이 처음 로드될 때 요청됩니다. 외부 구조는 다음과 같습니다.

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

   아래 `:children` 작성된 각 페이지에 대한 항목이 표시됩니다. 모든 페이지의 컨텐츠는 이 초기 JSON 요청에 있습니다. 콘텐츠가 이미 클라이언트측에서 사용 가능하기 때문에 탐색 라우팅을 사용하면 SPA의 후속 보기가 빠르게 로드됩니다.

   짐을 싣는 것은 현명하지 않다 **모두** 초기 JSON 요청에 있는 SPA 콘텐츠의 경우, 초기 페이지 로드 속도가 느려집니다. 다음으로 페이지의 계층 구조 깊이를 어떻게 수집하는지 살펴보겠습니다.

1. 다음 위치로 이동 **SPA 루트** 템플릿 위치: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   다음을 클릭합니다. **페이지 속성 메뉴** > **페이지 정책**:

   ![SPA 루트에 대한 페이지 정책 열기](assets/navigation-routing/open-page-policy.png)

1. 다음 **SPA 루트** 템플릿에 추가 항목이 있습니다. **계층 구조** 탭으로 이동하여 수집된 JSON 콘텐츠를 제어합니다. 다음 **구조 깊이** 사이트 계층 구조에서 하위 페이지를 수집할 깊이를 결정합니다. **루트**. 다음을 사용할 수도 있습니다 **구조 패턴** 정규 표현식을 기반으로 추가 페이지를 필터링할 필드입니다.

   업데이트 **구조 깊이** 끝 **2**:

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

   다음 사항에 주목하십시오. **페이지 3** 경로가 제거되었습니다. `/content/wknd-spa-react/us/en/home/page-2/page-3` 초기 JSON 모델에서. 이유는 다음과 같습니다. **페이지 3** 는 계층 구조에서 레벨 3에 있으며 최대 레벨 2의 컨텐츠만 포함하도록 정책을 업데이트했습니다.

1. SPA 홈 페이지를 다시 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 브라우저의 개발자 도구를 엽니다.

   페이지를 새로 고치면 다음에 대한 XHR 요청이 표시됩니다. `/content/wknd-spa-react/us/en.model.json`, SPA 루트 자습서에서 이전에 만든 SPA Root 템플릿에 대한 계층 깊이 구성에 따라 세 개의 하위 페이지만 포함됩니다. 여기에는 포함되지 않습니다 **페이지 3**.

   ![초기 JSON 요청 - SPA 루트](assets/navigation-routing/initial-json-request.png)

1. 개발자 도구를 연 상태에서 `Navigation` 바로 이동할 구성 요소 **페이지 3**:

   다음에 대한 새 XHR 요청이 수행되는지 확인합니다. `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![페이지 3 XHR 요청](assets/navigation-routing/page-3-xhr-request.png)

   AEM 모델 관리자는 **페이지 3** JSON 콘텐츠를 사용할 수 없으며 자동으로 추가 XHR 요청을 트리거합니다.

1. 다음으로 직접 이동하여 딥링크 실험: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 또한 브라우저의 뒤로 단추가 계속 작동하는지 확인하십시오.

## Inspect React 라우팅  {#react-routing}

탐색 및 라우팅은 [React 라우터](https://reactrouter.com/en/main). React Router는 React 애플리케이션의 탐색 구성 요소 모음입니다. [AEM React 핵심 구성 요소](https://github.com/adobe/aem-react-core-wcm-components-base) React Router의 기능을 사용하여 **탐색** 이전 단계에서 사용된 구성 요소입니다.

그런 다음 React Router가 SPA과 어떻게 통합되는지 확인하고 React Router를 사용하여 실험합니다 [링크](https://reactrouter.com/en/main/components/link) 구성 요소.

1. IDE에서 파일을 엽니다. `index.js` 위치: `ui.frontend/src/index.js`.

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

   다음 사항에 주목하십시오. `App` 은(는) 로 래핑됩니다. `Router` 구성 요소 출처 [React 라우터](https://reacttraining.com/react-router). 다음 `ModelManager`AEM SPA Editor JS SDK에서 제공하는 는 JSON 모델 API를 기반으로 AEM Pages에 동적 경로를 추가합니다.

1. 파일 열기 `Page.js` 위치: `ui.frontend/src/components/Page/Page.js`

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

   다음 `Page` SPA 구성 요소는 `MapTo` 매핑할 함수 **페이지** AEM에서 해당 SPA 구성 요소로 이동합니다. 다음 `withRoute` 유틸리티를 사용하면 다음에 준하여 SPA을 적절한 AEM 하위 페이지로 동적으로 라우팅할 수 있습니다. `cqPath` 속성.

1. 를 엽니다. `Header.js` 구성 요소 `ui.frontend/src/components/Header/Header.js`.
1. 업데이트 `Header` 을(를) 둘러싸려면 `<h1>` 의 태그 [링크](https://reactrouter.com/en/main/components/link) 홈 페이지:

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

   기본값 사용 대신 `<a>` 사용하는 앵커 태그 `<Link>` React Router 제공. 다음 기간 동안 `to=` 은 유효한 경로를 가리킵니다. SPA은 해당 경로로 전환되고 **아님** 전체 페이지 새로 고침을 수행합니다. 여기서는 의 사용을 설명하기 위해 홈 페이지에 대한 링크를 하드 코딩합니다. `Link`.

1. 다음 위치에 테스트 업데이트: `App.test.js` 위치: `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   에서 참조한 정적 구성 요소 내에서 React Router의 기능을 사용하고 있으므로 `App.js` 이를 감안하여 단위 테스트를 업데이트해야 합니다.

1. 터미널을 열고 프로젝트의 루트로 이동한 다음 Maven 기술을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEM의 SPA 페이지 중 하나로 이동합니다. [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   를 사용하지 않고 `Navigation` 탐색할 구성 요소에서에 있는 링크 사용 `Header`.

   ![헤더 링크](assets/navigation-routing/header-link.png)

   전체 페이지 새로 고침은 **아님** 가 트리거되고 SPA 라우팅이 작동하는지 확인합니다.

1. 필요한 경우 `Header.js` 표준을 사용한 파일 `<a>` 앵커 태그:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   이렇게 하면 SPA 라우팅과 일반 웹 페이지 링크의 차이점을 보여주는 데 도움이 됩니다.

## 축하합니다! {#congratulations}

축하합니다. SPA Editor SDK를 사용하여 AEM Pages에 매핑함으로써 SPA의 여러 보기를 지원하는 방법에 대해 알아보았습니다. 동적 탐색은 React Router를 사용하여 구현되었으며 `Header` 구성 요소.
