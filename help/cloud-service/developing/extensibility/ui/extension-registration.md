---
title: AEM UI 확장 등록
description: AEM UI 확장을 등록하는 방법에 대해 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# 확장 등록

AEM UI 확장은 React를 기반으로 하는 특수한 App Builder 앱이며 [반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/) UI 프레임워크.

AEM UI 확장이 표시되는 위치와 방법을 정의하려면 확장의 App Builder 앱에 앱 라우팅과 확장 등록이라는 두 가지 구성이 필요합니다.

## 앱 경로{#app-routes}

확장 `App.js` 다음 선언: [React 라우터](https://reactrouter.com/en/main) 에는 AEM UI에 확장을 등록하는 인덱스 경로가 포함됩니다.

인덱스 경로는 AEM UI가 처음 로드될 때 호출되며, 이 경로의 대상은 확장이 콘솔에 표시되는 방법을 정의합니다.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 확장 등록

`ExtensionRegistration.js` 는 확장의 인덱스 경로를 통해 즉시 로드되어야 하며, 확장의 등록 포인트를 처리합니다.

다음의 경우에 선택된 AEM UI 확장 템플릿 기반 [App Builder 앱 확장 초기화](./app-initialization.md), 다양한 확장 지점이 지원됩니다.

+ [콘텐츠 조각 UI 확장 지점](./content-fragments/overview.md#extension-points)


## 조건부로 확장 포함

AEM UI 확장은 사용자 지정 로직을 실행하여 확장이 표시되는 AEM 환경을 제한할 수 있습니다. 이 검사는 다음 작업 전에 수행됩니다. `register` 을 호출합니다. `ExtensionRegistration` 구성 요소를 반환하고, 확장을 표시하지 말아야 하는 경우 즉시 를 반환합니다.

이 검사에는 사용할 수 있는 제한된 컨텍스트가 있습니다.

+ 확장이 로드되는 AEM 호스트입니다.
+ 현재 사용자의 AEM 액세스 토큰입니다.

확장 로드에 대한 가장 일반적인 검사는 다음과 같습니다.

+ AEM 호스트 사용(`new URLSearchParams(window.location.search).get('repo')`)를 클릭하여 확장을 로드해야 하는지 확인합니다.
   + 특정 프로그램의 일부인 AEM 환경에서만 확장을 표시합니다(아래 예제에 표시됨).
   + 특정 AEM 환경(AEM 호스트)에서만 확장을 표시합니다.
+ 사용 [Adobe I/O Runtime 작업](./runtime-action.md) AEM에 대한 HTTP 호출을 수행하여 현재 사용자에게 확장이 표시되는지 확인합니다.

아래 예제는 확장을 프로그램의 모든 환경으로 제한하는 방법을 보여 줍니다 `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
