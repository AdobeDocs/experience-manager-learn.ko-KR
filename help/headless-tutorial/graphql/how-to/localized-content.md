---
title: AEM Headless에서 현지화된 컨텐츠 사용
description: GraphQL을 사용하여 현지화된 콘텐츠를 AEM에 쿼리하는 방법을 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 4fa84b0461cbdf2e25336259c4128be5585b8787
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 2%

---


# AEM Headless를 사용하여 지역화된 컨텐츠

AEM에서 제공 [번역 통합 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 헤드리스 컨텐츠의 경우 컨텐츠 조각 및 지원 자산을 로케일 간에 사용할 수 있도록 쉽게 변환할 수 있습니다. 페이지, 경험 조각, 자산 및 Forms과 같은 다른 AEM 컨텐츠를 변환하는 데 사용되는 것과 동일한 프레임워크입니다. 한 번 [헤드리스 컨텐츠가 번역됨](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=ko), 및 게시하면 헤드리스 애플리케이션에서 소비할 수 있습니다.

## 자산 폴더 구조{#assets-folder-structure}

AEM의 현지화된 컨텐츠 조각이 [권장 로컬라이제이션 구조](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![현지화된 AEM 자산 폴더](./assets/localized-content/asset-folders.jpg)

로케일 폴더는 동일한 항목이어야 하며 제목 대신 폴더 이름이 유효해야 합니다 [ISO 639-1 코드](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 폴더에 포함된 컨텐츠의 로케일을 나타냅니다.

로케일 코드는 GraphQL 쿼리에서 반환되는 컨텐츠 조각을 필터링하는 데 사용되는 값이기도 합니다.

| 로케일 코드 | AEM 경로 | 콘텐츠 로케일 |
|--------------------------------|----------|----------|
| de의 multi-field 값으로 변경되지 않습니다 | /content/dam/.../**de**/.. | 독일어 콘텐츠 |
| en | /content/dam/.../**en**/.. | 영어 콘텐츠 |
| es | /content/dam/.../**es**/.. | 스페인어 콘텐츠 |

## GraphQL 지속적인 쿼리

AEM에서 제공 `_locale` 로케일 코드 별로 컨텐츠를 자동으로 필터링하는 GraphQL 필터 . 예를 들어 [WKND 참조 데모 프로젝트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) 새 지속적인 쿼리로 수행할 수 있습니다 `wknd-shared/adventures-by-locale` 다음으로 정의:

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

다음 `$locale` 변수를에서 `_locale` 필터에 로케일 코드가 필요합니다(예: `en`, `en_us`, 또는 `de`)에 지정된 대로 [AEM 자산 폴더 기반 현지화 규칙](#assets-folder-structure).

## React 예

로케일 선택기를 사용하여 AEM에서 쿼리할 Adventure 컨텐츠를 제어하는 간단한 React 애플리케이션을 만들겠습니다 `_locale` 필터.

When __영어__ 이 로케일 선택기에서 선택되고 아래의 영어 Adventure 컨텐츠 조각이 선택됩니다 `/content/dam/wknd/en` 반환될 때 __스페인어__ 이 선택되고 의 스페인어 컨텐츠 조각이 선택됩니다 `/content/dam/wknd/es`등.

![현지화된 React 예제 앱](./assets/localized-content/react-example.png)

### 만들기 `LocaleContext`{#locale-context}

먼저, [React 컨텍스트](https://reactjs.org/docs/context.html) React 응용 프로그램의 구성 요소에서 로케일을 사용할 수 있도록 합니다.

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

### 만들기 `LocaleSwitcher` React 구성 요소{#locale-switcher}

다음으로, [로케일 컨텍스트](#locale-context) 값을 지정한 경우 이해할 수 있도록 해줍니다.

이 로케일 값은 GraphQL 쿼리를 구동하는 데 사용되며 선택한 로케일과 일치하는 컨텐츠만 반환하도록 합니다.

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

### 을 사용하여 컨텐츠 쿼리 `_locale` 필터{#adventures}

모험 구성 요소는 로케일별로 모든 모험을 AEM에 쿼리하고 해당 제목을 나열합니다. 이 작업은 React 컨텍스트에 저장된 로케일 값을 `_locale` 필터.

이 접근 방식은 응용 프로그램의 다른 쿼리로 확장될 수 있으므로 모든 쿼리에 사용자의 로케일 선택에 의해 지정된 콘텐츠만 포함되도록 할 수 있습니다.

AEM에 대한 쿼리는 사용자 지정 React 후크에서 수행됩니다 [getAdventureByLocale을 참조하십시오. AEM GraphQL 쿼리 설명서에 대해 자세히 설명합니다](./aem-headless-sdk.md).

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

### 을(를) 정의합니다 `App.js`{#app-js}

마지막으로 React 애플리케이션을 와 함께 래핑하여 모두 함께 연결합니다. `LanguageContext.Provider` 로케일 값을 설정합니다. 이렇게 하면 다른 React 구성 요소가 허용됩니다. [로케일 전환기](#locale-switcher), 및 [모험](#adventures) 로케일 선택 상태를 공유하려면

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
