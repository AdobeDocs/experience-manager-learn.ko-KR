---
title: AEM Headless로 현지화된 콘텐츠 사용
description: GraphQL을 사용하여 AEM에 지역화된 콘텐츠를 쿼리하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# AEM Headless로 현지화된 콘텐츠

AEM은 Headless 콘텐츠를 위한 [번역 통합 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=ko)를 제공하여 콘텐츠 조각 및 지원 에셋을 로케일에서 사용하기 위해 쉽게 번역할 수 있도록 합니다. 페이지, 경험 조각, Assets 및 Forms과 같은 다른 AEM 콘텐츠를 번역하는 데 사용되는 것과 동일한 프레임워크입니다. [Headless 콘텐츠를 번역](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=ko)하고 게시하면 Headless 애플리케이션에서 사용할 수 있습니다.

## Assets 폴더 구조{#assets-folder-structure}

AEM의 지역화된 콘텐츠 조각이 [권장된 지역화 구조](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html?lang=ko#recommended-structure)를 따르는지 확인하십시오.

![지역화된 AEM 자산 폴더](./assets/localized-content/asset-folders.jpg)

로케일 폴더는 형제여야 하며, 폴더 이름은 제목이 아니라 폴더에 포함된 컨텐츠의 로케일을 나타내는 올바른 [ISO 639-1 코드](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)여야 합니다.

로케일 코드는 GraphQL 쿼리에서 반환되는 콘텐츠 조각을 필터링하는 데 사용되는 값이기도 합니다.

| 로케일 코드 | AEM 경로 | 컨텐츠 로케일 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | 독일어 콘텐츠 |
| en | /content/dam/.../**en**/... | 영어 콘텐츠 |
| es | /content/dam/.../**es**/... | 스페인어 콘텐츠 |

## GraphQL 지속 쿼리

AEM에서는 로케일 코드 별로 콘텐츠를 자동으로 필터링하는 `_locale` GraphQL 필터를 제공합니다. 예를 들어 [WKND 사이트 프로젝트](https://github.com/adobe/aem-guides-wknd)의 모든 영문 모험을 쿼리하는 작업은 다음과 같이 정의된 새 지속 쿼리 `wknd-shared/adventures-by-locale`을(를) 사용하여 수행할 수 있습니다.

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

`_locale` 필터에 사용되는 `$locale` 변수에는 [AEM의 자산 폴더 기반 로컬라이제이션 규칙](#assets-folder-structure)에 지정된 로케일 코드(예: `en`, `en_us` 또는 `de`)가 필요합니다.

## React 예

`_locale` 필터를 사용하여 로케일 선택기를 기반으로 AEM에서 쿼리할 Adventure 콘텐츠를 제어하는 간단한 React 응용 프로그램을 만들어 보겠습니다.

로케일 선택기에서 __영어__&#x200B;을(를) 선택하면 `/content/dam/wknd/en`의 영어 Adventure 콘텐츠 조각이 반환되고 __스페인어__&#x200B;를 선택하면 `/content/dam/wknd/es`의 스페인어 콘텐츠 조각 등이 반환됩니다.

![지역화된 React 예제 앱](./assets/localized-content/react-example.png)

### `LocaleContext` 만들기{#locale-context}

먼저 React 응용 프로그램의 구성 요소에서 로케일을 사용할 수 있도록 [React 컨텍스트](https://reactjs.org/docs/context.html)를 만듭니다.

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

### `LocaleSwitcher` React 구성 요소 만들기{#locale-switcher}

그런 다음 사용자의 선택 항목으로 [LocaleContext&#39;s](#locale-context) 값을 설정하는 로케일 전환기 React 구성 요소를 만듭니다.

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

### `_locale` 필터를 사용하여 콘텐츠 쿼리{#adventures}

모험 구성 요소는 AEM에서 로케일별로 모든 모험 페이지를 쿼리하고 제목을 나열합니다. 이 작업은 React 컨텍스트에 저장된 로케일 값을 `_locale` 필터를 사용하여 쿼리에 전달함으로써 수행됩니다.

이 접근 방식은 애플리케이션의 다른 쿼리로 확장되어 모든 쿼리에 사용자의 로케일 선택에 의해 지정된 콘텐츠만 포함되도록 할 수 있습니다.

AEM에 대한 쿼리는 사용자 지정 React 후크 [getAdventuresByLocale에서 수행됩니다. 자세한 내용은 AEM GraphQL 쿼리 설명서](./aem-headless-sdk.md)를 참조하십시오.

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

### `App.js` 정의{#app-js}

마지막으로, 의 React 응용 프로그램을 `LanguageContext.Provider`(으)로 래핑하고 로케일 값을 설정하여 이 응용 프로그램을 모두 연결합니다. 이렇게 하면 다른 React 구성 요소인 [LocaleSwitcher](#locale-switcher) 및 [Adventures](#adventures)에서 로케일 선택 상태를 공유할 수 있습니다.

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
