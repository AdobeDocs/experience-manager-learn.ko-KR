---
title: AEM Headless로 현지화된 콘텐츠 사용
description: GraphQL을 사용하여 현지화된 콘텐츠를 AEM에 쿼리하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 192
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# AEM Headless를 사용한 현지화된 콘텐츠

AEM에서 다음을 제공합니다. [번역 통합 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) headless 콘텐츠의 경우 콘텐츠 조각 및 지원 에셋을 로케일에서 사용하기 위해 쉽게 번역할 수 있도록 합니다. 페이지, 경험 조각, 에셋 및 Forms과 같은 다른 AEM 콘텐츠를 번역하는 데 사용되는 것과 동일한 프레임워크입니다. 한 번 [헤드리스 콘텐츠 번역됨](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html), 그리고 게시되어 headless 애플리케이션에서 사용할 준비가 되었습니다.

## 에셋 폴더 구조{#assets-folder-structure}

AEM에서 현지화된 콘텐츠 조각이 다음을 따르는지 확인합니다. [권장 로컬라이제이션 구조](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![현지화된 AEM assets 폴더](./assets/localized-content/asset-folders.jpg)

로케일 폴더는 동위 멤버여야 하며 제목이 아닌 폴더 이름이 유효해야 합니다 [ISO 639-1 코드](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 폴더에 포함된 컨텐츠의 로케일을 나타냅니다.

로케일 코드는 GraphQL 쿼리에서 반환되는 콘텐츠 조각을 필터링하는 데 사용되는 값이기도 합니다.

| 로케일 코드 | AEM 경로 | 컨텐츠 로케일 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | 독일어 콘텐츠 |
| en | /content/dam/.../**en**/... | 영어 콘텐츠 |
| es | /content/dam/.../**es**/... | 스페인어 콘텐츠 |

## GraphQL 지속 쿼리

AEM에서 다음을 제공합니다. `_locale` 로케일 코드 별로 컨텐츠를 자동으로 필터링하는 GraphQL 필터 . 예를 들어 의 모든 영국 모험 쿼리 [WKND 사이트 프로젝트](https://github.com/adobe/aem-guides-wknd) 새 지속 쿼리로 수행할 수 있습니다. `wknd-shared/adventures-by-locale` 다음으로 정의됨:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

다음 `$locale` 에 사용되는 변수 `_locale` 필터에는 로케일 코드가 필요합니다(예: `en`, `en_us`, 또는 `de`에 지정된 ) [AEM 자산 폴더 기반 현지화 규칙](#assets-folder-structure).

## React 예

를 사용하여 로케일 선택기를 기반으로 AEM에서 쿼리할 Adventure 콘텐츠를 제어하는 간단한 React 애플리케이션을 만들어 보겠습니다. `_locale` 필터.

날짜 __영어__ 로케일 선택기에서 영어 Adventure Content Fragments가 `/content/dam/wknd/en` 다음과 같은 경우 반환됩니다. __스페인어__ 이(가) 선택된 다음 아래에서 스페인어 콘텐츠 조각 을 선택합니다. `/content/dam/wknd/es`기타 등등.

![현지화된 React 예제 앱](./assets/localized-content/react-example.png)

### 만들기 `LocaleContext`{#locale-context}

먼저 다음을 생성합니다. [React 컨텍스트](https://reactjs.org/docs/context.html) React 응용 프로그램의 구성 요소에서 로케일을 사용할 수 있도록 허용합니다.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### 만들기 `LocaleSwitcher` 구성 요소 반응{#locale-switcher}

그런 다음 로케일 전환기 React 구성 요소를 만듭니다. 이 구성 요소는 [LocaleContext의](#locale-context) 값을 사용자의 선택 항목으로 지정합니다.

이 로케일 값은 GraphQL 쿼리를 구동하는 데 사용되며 선택한 로케일과 일치하는 콘텐츠만 반환하도록 합니다.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### 를 사용하여 콘텐츠 쿼리 `_locale` 필터{#adventures}

모험 구성 요소는 모든 모험을 로케일별로 AEM에 쿼리하고 제목을 나열합니다. 이렇게 하려면 React 컨텍스트에 저장된 로케일 값을 `_locale` 필터.

이 접근 방식은 애플리케이션의 다른 쿼리로 확장되어 모든 쿼리에 사용자의 로케일 선택에 의해 지정된 콘텐츠만 포함되도록 할 수 있습니다.

AEM에 대한 쿼리는 사용자 지정 React 후크에서 수행됩니다 [getAdventuresByLocale, AEM GraphQL 쿼리 설명서에 자세히 설명됨](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### 다음을 정의합니다. `App.js`{#app-js}

마지막으로, React 애플리케이션을 와 함께 래핑하여 모두 결합합니다. `LanguageContext.Provider` 로케일 값 설정 이렇게 하면 다른 React 구성 요소가 [로케일 전환기](#locale-switcher), 및 [모험](#adventures) 로케일 선택 상태를 공유합니다.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
