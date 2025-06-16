---
title: 탐색 및 라우팅 추가 | AEM SPA 편집기 및 반응 시작하기
description: SPA 편집기 SDK을 사용하여 AEM 페이지에 매핑하여 SPA의 여러 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 React Router 및 React 핵심 구성 요소를 사용하여 구현됩니다.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
duration: 337
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 0%

---

# 탐색 및 라우팅 추가 {#navigation-routing}

{{spa-editor-deprecation}}

SPA 편집기 SDK을 사용하여 AEM 페이지에 매핑하여 SPA의 여러 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 React Router 및 React 핵심 구성 요소를 사용하여 구현됩니다.

## 목표

1. SPA 편집기를 사용할 때 사용할 수 있는 SPA 모델 라우팅 옵션을 이해합니다.
1. [React Router](https://reacttraining.com/react-router)를 사용하여 SPA의 다양한 보기 사이를 탐색하는 방법을 알아봅니다.
1. AEM React 핵심 구성 요소를 사용하여 AEM 페이지 계층 구조에 의해 실행되는 동적 탐색을 구현합니다.

## 빌드할 내용

이 장에서는 AEM의 SPA에 탐색 기능을 추가합니다. 탐색 메뉴는 AEM 페이지 계층 구조에 의해 구동되며 [탐색 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/navigation.html?lang=ko)에서 제공하는 JSON 모델을 사용합니다.

![탐색 추가됨](assets/navigation-routing/navigation-added.png)

## 사전 요구 사항

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. 이 챕터는 [구성 요소 매핑](map-components.md) 챕터의 연속이지만, 필요한 사항은 로컬 AEM 인스턴스에 배포된 SPA 사용 AEM 프로젝트입니다.

## 템플릿에 탐색 추가 {#add-navigation-template}

1. 브라우저를 열고 AEM([http://localhost:4502/](http://localhost:4502/))에 로그인합니다. 시작 코드 베이스를 이미 배포해야 합니다.
1. **SPA 페이지 템플릿**(으)로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 가장 바깥쪽의 **루트 레이아웃 컨테이너**&#x200B;를 선택하고 **정책** 아이콘을 클릭합니다. 작성을 위해 잠기지 않은 **레이아웃 컨테이너**&#x200B;를 선택하려면 **not**&#x200B;에 주의하십시오.

   ![루트 레이아웃 컨테이너 정책 아이콘 선택](assets/navigation-routing/root-layout-container-policy.png)

1. 이름이 **SPA 구조**&#x200B;인 새 정책 만들기:

   ![SPA 구조 정책](assets/navigation-routing/spa-policy-update.png)

   **허용된 구성 요소** > **일반** > **레이아웃 컨테이너** 구성 요소를 선택합니다.

   **허용된 구성 요소** > **WKND SPA REACT - 구조**&#x200B;에서 **탐색** 구성 요소를 선택합니다.

   ![탐색 구성 요소 선택](assets/navigation-routing/select-navigation-component.png)

   **허용된 구성 요소** > **WKND SPA REACT - 콘텐츠**&#x200B;에서 **이미지** 및 **텍스트** 구성 요소를 선택합니다. 총 4개의 구성 요소를 선택해야 합니다.

   **완료**&#x200B;를 클릭하여 변경 내용을 저장합니다.

1. 페이지를 새로 고치고 잠기지 않은 **레이아웃 컨테이너** 위에 **탐색** 구성 요소를 추가하십시오.

   ![템플릿에 탐색 구성 요소 추가](assets/navigation-routing/add-navigation-component.png)

1. **탐색** 구성 요소를 선택하고 **정책** 아이콘을 클릭하여 정책을 편집합니다.
1. **SPA 탐색**&#x200B;의 **정책 제목**&#x200B;을 사용하여 새 정책을 만듭니다.

   **속성**&#x200B;에서:

   * **탐색 루트**&#x200B;을(를) `/content/wknd-spa-react/us/en`(으)로 설정합니다.
   * **루트 수준 제외**&#x200B;를 **1**(으)로 설정합니다.
   * **모든 하위 페이지 수집**&#x200B;을 선택 취소합니다.
   * **탐색 구조 깊이**&#x200B;을(를) **3**(으)로 설정합니다.

   ![탐색 정책 구성](assets/navigation-routing/navigation-policy.png)

   `/content/wknd-spa-react/us/en` 아래에 있는 탐색 수준을 수집합니다.

1. 변경 내용을 저장하면 템플릿의 일부로 채워진 `Navigation`이(가) 표시됩니다.

   ![채워진 탐색 구성 요소](assets/navigation-routing/populated-navigation.png)

## 하위 페이지 만들기

그런 다음 SPA에서 다양한 보기 역할을 할 AEM에서 추가 페이지를 만듭니다. 또한 AEM에서 제공하는 JSON 모델의 계층 구조도 검사할 것입니다.

1. **사이트** 콘솔로 이동합니다. [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). **WKND SPA React 홈 페이지**&#x200B;를 선택하고 **만들기** > **페이지**&#x200B;를 클릭합니다.

   ![새 페이지 만들기](assets/navigation-routing/create-new-page.png)

1. **템플릿**&#x200B;에서 **SPA 페이지**&#x200B;을(를) 선택합니다. **속성**&#x200B;에서 **제목** 및 **페이지-1**&#x200B;에 대한 **페이지 1**&#x200B;을(를) 이름으로 입력하십시오.

   ![초기 페이지 속성을 입력하십시오](assets/navigation-routing/initial-page-properties.png)

   **만들기**&#x200B;를 클릭하고 대화 상자 팝업에서 **열기**&#x200B;를 클릭하여 AEM SPA 편집기에서 페이지를 엽니다.

1. 새 **Text** 구성 요소를 기본 **레이아웃 컨테이너**&#x200B;에 추가합니다. 구성 요소를 편집하고 RTE 및 **H2** 요소를 사용하여 **페이지 1** 텍스트를 입력하십시오.

   ![샘플 콘텐츠 페이지 1](assets/navigation-routing/page-1-sample-content.png)

   이미지와 같은 콘텐츠를 자유롭게 추가할 수 있습니다.

1. AEM Sites 콘솔로 돌아가서 위의 단계를 반복하여 **페이지 2**&#x200B;라는 두 번째 페이지를 **페이지 1**&#x200B;의 형제 페이지로 만듭니다.
1. 마지막으로 세 번째 페이지인 **페이지 3**&#x200B;을(를) 만들고 **페이지 2**&#x200B;의 **자식**(으)로 만듭니다. 사이트 계층 구조가 완료되면 다음과 같이 표시됩니다.

   ![샘플 사이트 계층 구조](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 이제 탐색 구성 요소를 사용하여 SPA의 다양한 영역으로 이동할 수 있습니다.

   ![탐색 및 라우팅](assets/navigation-routing/navigation-working.gif)

1. AEM 편집기 외부의 페이지를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). **탐색** 구성 요소를 사용하여 앱의 다양한 보기로 이동합니다.

1. 탐색할 때 브라우저의 개발자 도구를 사용하여 네트워크 요청을 검사합니다. 아래 스크린샷은 Google Chrome 브라우저에서 캡처됩니다.

   ![네트워크 요청 관찰](assets/navigation-routing/inspect-network-requests.png)

   초기 페이지 로드 후 후속 탐색으로 인해 전체 페이지가 새로 고쳐지지 않고 이전에 방문한 페이지로 돌아갈 때 네트워크 트래픽이 최소화되는지 확인하십시오.

## 계층 페이지 JSON 모델 {#hierarchy-page-json-model}

그런 다음 SPA의 다중 보기 경험을 구동하는 JSON 모델을 검사합니다.

1. 새 탭에서 AEM에서 제공하는 JSON 모델 API를 엽니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 브라우저 확장을 사용하여 [JSON 형식 지정](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)을 수행하는 것이 유용할 수 있습니다.

   SPA가 처음 로드될 때 이 JSON 콘텐츠가 요청됩니다. 외부 구조는 다음과 같습니다.

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

   `:children`에서 만들어진 각 페이지에 대한 항목이 표시됩니다. 모든 페이지의 컨텐츠는 이 초기 JSON 요청에 있습니다. 탐색 라우팅을 사용하면 클라이언트측에서 콘텐츠를 이미 사용할 수 있으므로 SPA의 후속 보기가 빠르게 로드됩니다.

   초기 JSON 요청에서 SPA의 **ALL** 콘텐츠를 로드하는 것은 좋지 않습니다. 이렇게 하면 초기 페이지 로드 속도가 느려질 수 있습니다. 다음으로 페이지의 계층 구조 깊이를 어떻게 수집하는지 살펴보겠습니다.

1. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)에서 **SPA 루트** 템플릿으로 이동합니다.

   **페이지 속성 메뉴** > **페이지 정책**&#x200B;을 클릭합니다.

   ![SPA 루트에 대한 페이지 정책 열기](assets/navigation-routing/open-page-policy.png)

1. **SPA 루트** 템플릿에는 수집된 JSON 콘텐츠를 제어하는 추가 **계층 구조** 탭이 있습니다. **구조 깊이**&#x200B;은(는) 사이트 계층 구조에서 **루트** 아래에 있는 하위 페이지를 수집하는 정도를 결정합니다. **구조 패턴** 필드를 사용하여 정규 표현식에 따라 추가 페이지를 필터링할 수도 있습니다.

   **구조 깊이**&#x200B;을(를) **2**(으)로 업데이트:

   ![구조 깊이 업데이트](assets/navigation-routing/update-structure-depth.png)

   정책에 대한 변경 내용을 저장하려면 **완료**&#x200B;를 클릭합니다.

1. JSON 모델 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)을(를) 다시 엽니다.

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

   **페이지 3** 경로가 초기 JSON 모델에서 제거되었습니다. `/content/wknd-spa-react/us/en/home/page-2/page-3`. **페이지 3**&#x200B;이(가) 계층 구조의 수준 3에 있으며 최대 수준 2의 콘텐츠만 포함하도록 정책을 업데이트했기 때문입니다.

1. SPA 홈 페이지 [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)을(를) 다시 열고 브라우저의 개발자 도구를 엽니다.

   페이지를 새로 고치면 SPA 루트인 `/content/wknd-spa-react/us/en.model.json`에 대한 XHR 요청이 표시됩니다. 자습서에서 이전에 만든 SPA 루트 템플릿에 대한 계층 구조 깊이 구성에 따라 하위 페이지가 세 개만 포함됩니다. 여기에는 **페이지 3**&#x200B;이(가) 포함되지 않습니다.

   ![초기 JSON 요청 - SPA 루트](assets/navigation-routing/initial-json-request.png)

1. 개발자 도구를 연 상태에서 `Navigation` 구성 요소를 사용하여 **페이지 3**(으)로 바로 이동합니다.

   `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`에 새 XHR 요청이 수행되었는지 확인합니다.

   ![XHR 요청 3개 페이지](assets/navigation-routing/page-3-xhr-request.png)

   AEM 모델 관리자는 **페이지 3** JSON 콘텐츠를 사용할 수 없다는 것을 이해하고 추가 XHR 요청을 자동으로 트리거합니다.

1. [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)&#x200B;(으)로 직접 이동하여 딥링크를 실험해 보십시오. 또한 브라우저의 뒤로 단추가 계속 작동하는지 확인하십시오.

## React 라우팅 검사  {#react-routing}

[React 라우터](https://reactrouter.com/en/main)를 사용하여 탐색 및 라우팅을 구현했습니다. React Router는 React 애플리케이션의 탐색 구성 요소 모음입니다. [AEM React 핵심 구성 요소](https://github.com/adobe/aem-react-core-wcm-components-base)는 React 라우터의 기능을 사용하여 이전 단계에서 사용한 **탐색** 구성 요소를 구현합니다.

그런 다음 React Router가 SPA와 어떻게 통합되었는지 확인하고 React Router의 [Link](https://reactrouter.com/en/main/components/link) 구성 요소를 사용하여 실험하십시오.

1. IDE에서 `ui.frontend/src/index.js`에 있는 `index.js` 파일을 엽니다.

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

   `App`이(가) [React Router](https://reacttraining.com/react-router)의 `Router` 구성 요소에 래핑되어 있습니다. AEM SPA Editor JS SDK에서 제공하는 `ModelManager`은(는) JSON 모델 API를 기반으로 AEM Pages에 동적 경로를 추가합니다.

1. `ui.frontend/src/components/Page/Page.js`에서 `Page.js` 파일을 엽니다.

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

   `Page` SPA 구성 요소는 `MapTo` 함수를 사용하여 AEM의 **페이지**&#x200B;를 해당 SPA 구성 요소에 매핑합니다. `withRoute` 유틸리티를 사용하면 SPA를 `cqPath` 속성에 따라 적절한 AEM 하위 페이지로 동적으로 라우팅할 수 있습니다.

1. `ui.frontend/src/components/Header/Header.js`에서 `Header.js` 구성 요소를 엽니다.
1. `Header`을(를) 업데이트하여 [Link](https://reactrouter.com/en/main/components/link)에서 `<h1>` 태그를 홈 페이지에 래핑합니다.

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

   기본 `<a>` 앵커 태그를 사용하는 대신 React Router에서 제공한 `<Link>`을(를) 사용합니다. `to=`이(가) 올바른 경로를 가리키면 SPA가 해당 경로로 전환되고 **not**&#x200B;이(가) 전체 페이지 새로 고침을 수행합니다. 여기서는 `Link`의 사용을 설명하기 위해 홈 페이지에 대한 링크를 하드 코딩합니다.

1. `ui.frontend/src/App.test.js`의 `App.test.js`에서 테스트를 업데이트합니다.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   `App.js`에서 참조된 정적 구성 요소 내에서 React Router의 기능을 사용하고 있으므로 이를 설명하기 위해 단위 테스트를 업데이트해야 합니다.

1. 터미널을 열고 프로젝트의 루트로 이동한 다음 Maven 기술을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEM에서 SPA의 페이지 중 하나로 이동합니다. [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   `Navigation` 구성 요소를 사용하여 탐색하는 대신 `Header`에서 링크를 사용하십시오.

   ![헤더 링크](assets/navigation-routing/header-link.png)

   전체 페이지 새로 고침이 **트리거되지 않음**&#x200B;이며 SPA 라우팅이 작동하는지 확인합니다.

1. 필요한 경우 표준 `<a>` 앵커 태그를 사용하여 `Header.js` 파일로 테스트합니다.

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   이렇게 하면 SPA 라우팅과 일반 웹 페이지 링크의 차이점을 보여주는 데 도움이 될 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA 편집기 SDK을 사용하여 AEM 페이지에 매핑하여 SPA의 여러 보기를 지원하는 방법에 대해 알아보았습니다. React Router를 사용하여 동적 탐색을 구현하고 `Header` 구성 요소에 추가했습니다.
