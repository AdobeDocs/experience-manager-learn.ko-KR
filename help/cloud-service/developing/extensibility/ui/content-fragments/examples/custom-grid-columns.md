---
title: 콘텐츠 조각 콘솔의 사용자 지정 그리드 열
description: 사용자 지정 그리드 열을 콘텐츠 조각 콘솔에 추가하는 방법에 대해 알아봅니다.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
exl-id: 87143cf9-e932-4ad6-afe2-cce093c520f4
duration: 198
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 사용자 정의 격자 열

![콘텐츠 조각 콘솔 사용자 지정 눈금 열](./assets/custom-grid-columns/hero.png){align="center"}

사용자 지정 표 열은 `contentFragmentGrid` 확장 지점을 사용하여 콘텐츠 조각 콘솔에 추가할 수 있습니다. 이 예에서는 마지막 수정 날짜를 기반으로 콘텐츠 조각 페이지를 표시하는 사용자 정의 열을 사람이 읽을 수 있는 형식으로 추가하는 방법을 보여 줍니다.

## 확장 지점

이 예제는 확장 지점 `contentFragmentGrid`까지 확장하여 콘텐츠 조각 콘솔에 사용자 지정 열을 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- |
| [콘텐츠 조각 콘솔](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [표 형태 창](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## 확장 예

다음 예제에서는 사람이 읽을 수 있는 형식으로 콘텐츠 조각의 날짜를 표시하는 사용자 지정 열 `Age`을(를) 만듭니다. 기간은 콘텐츠 조각의 마지막 수정 날짜부터 계산됩니다.

이 코드는 콘텐츠 조각의 메타데이터를 확장의 등록 파일에서 얻는 방법과 콘텐츠 조각의 JSON 콘텐츠를 변환하는 방법을 보여 줍니다.

이 예제에서는 [Luxon](https://moment.github.io/luxon/) 라이브러리를 사용하여 `npm i luxon`을(를) 통해 설치된 콘텐츠 조각의 사용 기간을 계산합니다.

### 확장 등록

index.html 경로에 매핑된 `ExtensionRegistration.js`은(는) AEM 확장의 진입점이며 다음을 정의합니다.

+ 확장 위치는 AEM 작성 환경에서 자체(`contentFragmentGrid`)를 주입합니다
+ `getColumns()` 함수의 사용자 지정 열 정의
+ 행별 각 사용자 정의 열에 대한 값

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { Duration } from "luxon";

/**
 * Set up a in-memory cache of the custom column data.
 * This is important if the work to contain column data is expensive, such as making HTTP requests to obtain the value.
 * 
 * The caching of computed value is optional, but recommended if the work to compute the column data is expensive.
 */
const COLUMN_AGE_ID = "age";
const cache = {
  [COLUMN_AGE_ID]: {},
};

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        contentFragmentGrid: {
          getColumns() {
            return [
              {
                id: COLUMN_AGE_ID,          // Give the column a unique ID.
                label: "Age",               // The label to display
                render: async function (fragments) {
                  // This function is run for each row in the grid.
                  // The fragments parameter is an array of all the fragments (aka each row) in the grid.
                  const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();

                  // Iterate over each fragment in the grid
                  for (const fragment of fragments) {
                    // Check if a previous pass has computed the value for this fragment.id. If it has, we can skip it.
                    if (!cache[COLUMN_AGE_ID][fragment.id]) {
                      // If the fragment.id has not been computed and cached, then compute the value and cache it.
                      cache[COLUMN_AGE_ID][fragment.id] = await computeAgeColumnValue(fragment, context);
                    }
                  }
                  // Return the populated cache of the custom column data.
                  return cache[COLUMN_AGE_ID];
                }
              },
              // Add other custom columns here...
            ];
          },
        },
      },
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

async function computeAgeColumnValue(fragment, context) {
  // Various data is available in the sharedContext, such as the AEM host, the IMS token, and the fragment itself.
  
  // Accessing AEM APIs requires the IMS token, which is available in the sharedContext, and also supporting CORS configurations deployed to AEM Author.
  //const aemHost = context.aemHost;
  //const accessToken = context.auth.imsToken;
  
  // Get the user's locale to format the value of the column.
  const locale = context.locale;

  // Compute the value of the column, in this case we are computing the age of the fragment from its last modified date.
  const duration = Duration.fromMillis(Date.now() - fragment.modifiedDate).rescale();

  let largestUnit = {};

  if (duration.years > 0) {
    largestUnit = {years: duration.years};
  } else if (duration.months > 0) {
    largestUnit = {months: duration.months};
  } else if (duration.days > 0) {
    largestUnit = {days: duration.days};
  } else if (duration.hours > 0) {
    largestUnit = {hours: duration.hours};
  } else if (duration.minutes > 0) {
    largestUnit = {minutes: duration.minutes};
  } else if (duration.seconds > 0) {
    largestUnit = {seconds: duration.seconds};
  }

  // Convert the largest unit of the age to human readable format.
  const columnValue = Duration.fromObject(largestUnit, {locale: locale}).toHuman();

  // Return the value of the column.
  return columnValue;
}

export default ExtensionRegistration;
```

#### 컨텐츠 조각 데이터

`render(..)`의 `getColumns()` 메서드가 조각 배열로 전달됩니다. 배열의 각 개체는 그리드의 행을 나타내며 콘텐츠 조각에 대한 다음 메타데이터를 포함합니다. 이 메타데이터는 그리드에서 자주 사용되는 사용자 정의 열에 사용할 수 있습니다.


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

`fragments` 메서드에서 `render(..)` 매개 변수의 요소로 사용할 수 있는 예제 콘텐츠 조각 JSON입니다.

```json
{
    "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
    "name": "alaskan-adventures",
    "title": "Alaskan Adventures",
    "status": "draft",
    "statusPreview": null,
    "model": {
        "name": "Article",
        "id": "/conf/wknd-shared/settings/dam/cfm/models/article"
    },
    "folderId": "/content/dam/wknd-shared/en/magazine/alaska-adventure",
    "folderName": "Alaska Adventure",
    "createdBy": "admin",
    "createdDate": 1684875665786,
    "modifiedBy": "admin",
    "modifiedDate": 1654104774889,
    "publishedBy": "",
    "publishedDate": 0,
    "locale": "",
    "main": true,
    "translations": {
        "locale": "en",
        "languageCopies": [
            {
                "path": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
                "title": "Alaskan Adventures",
                "locale": "en",
                "model": "Article",
                "status": "DRAFT",
                "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures"
            }
        ]
    },
    "transientStatus": {},
    "extensions": {
        "age": "1 year old"
    }
}
```

사용자 지정 열을 채우는 데 다른 데이터가 필요한 경우 AEM 작성자에게 데이터를 검색하도록 HTTP 요청을 할 수 있습니다.

>[!IMPORTANT]
>
> AEM 작성자 인스턴스가 AppBuilder 앱이 실행 중인 원본에서 [원본 간 요청](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ko)을 허용하도록 구성되어 있는지 확인하십시오. 허용되는 원본에는 `https://localhost:9080`, AppBuilder Stage 원본 및 AppBuilder 프로덕션 원본이 포함됩니다.
>
> 또는 확장 프로그램이 확장 프로그램을 대신하여 AEM 작성자에게 요청하는 사용자 지정 [AppBuilder 작업](../../runtime-action.md)을 호출할 수 있습니다.


```javascript
const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();
...
// Fetch the Content Fragment JSON from AEM Author using the AEM Assets HTTP API
const response = await fetch(`${context.aemHost}${fragment.id.slice('/content/dam'.length)}.json`, {
    headers: {
        'Authorization': `Bearer ${context.auth.imsToken}`
    }
})
...
```

#### 열 정의

렌더링 메서드의 결과는 키가 콘텐츠 조각(또는 `fragment.id`)의 경로이고 값이 열에 표시할 값인 JavaScript 개체입니다.

예를 들어, `age` 열에 대한 이 확장의 결과는 다음과 같습니다.

```json
{
    "/content/dam/wknd-shared/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks": "22 minutes",
    "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp": "1 hour",
    "/content/dam/wknd-shared/en/magazine/western-australia/western-australia-by-camper-van": "1 hour",
    "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand": "10 months",
    "/content/dam/wknd-shared/en/magazine/skitouring/skitouring": "1 year",
    "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland": "1 year",
    "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures": "1 year",
    "/content/dam/wknd-shared/en/magazine/arctic-surfing/aloha-spirits-in-northern-norway": "1 year",
    "/content/dam/wknd-shared/en/magazine/san-diego-surf-spots/san-diego-surfspots": "1 year",
    "/content/dam/wknd-shared/en/magazine/fly-fishing-amazon/fly-fishing": "1 year",
    "/content/dam/wknd-shared/en/adventures/napa-wine-tasting/napa-wine-tasting": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah": "1 year",
    "/content/dam/wknd-shared/en/adventures/gastronomic-marais-tour/gastronomic-marais-tour": "1 year",
    "/content/dam/wknd-shared/en/adventures/tahoe-skiing/tahoe-skiing": "1 year",
    "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica": "1 year",
    "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking": "1 year",
    "/content/dam/wknd-shared/en/adventures/whistler-mountain-biking/whistler-mountain-biking": "1 year",
    "/content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing": "1 year",
    "/content/dam/wknd-shared/en/adventures/ski-touring-mont-blanc/ski-touring-mont-blanc": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany": "1 year",
    "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling": "1 year",
    "/content/dam/wknd-shared/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming": "1 year",
    "/content/dam/wknd-shared/en/adventures/riverside-camping-australia/riverside-camping-australia": "1 year",
    "/content/dam/wknd-shared/en/contributors/ian-provo": "1 year",
    "/content/dam/wknd-shared/en/contributors/sofia-sj-berg": "1 year",
    "/content/dam/wknd-shared/en/contributors/justin-barr": "1 year",
    "/content/dam/wknd-shared/en/contributors/jake-hammer": "1 year",
    "/content/dam/wknd-shared/en/contributors/jacob-wester": "1 year",
    "/content/dam/wknd-shared/en/contributors/stacey-roswells": "1 year",
    "/content/dam/wknd-shared/en/contributors/kumar-selveraj": "1 year"
}
```
