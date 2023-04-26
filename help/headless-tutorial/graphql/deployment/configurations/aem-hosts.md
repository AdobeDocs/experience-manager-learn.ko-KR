---
title: AEM GraphQL용 AEM 호스트 관리
description: AEM Headless 앱에서 AEM 호스트를 구성하는 방법을 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
source-git-commit: ec2609ed256ebe6cdd7935f3e8d476c1ff53b500
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 1%

---

# AEM 호스트 관리

AEM Headless 애플리케이션을 배포하려면 올바른 AEM 호스트/도메인이 사용되도록 AEM URL을 구성하는 방법에 주의를 기울여야 합니다. 알고 있어야 하는 기본 URL/요청 유형은 다음과 같습니다.

+ 에 대한 HTTP 요청 __[AEM GraphQL API](#aem-graphql-api-requests)__
+ __[이미지 URL](#aem-image-urls)__ 컨텐츠 조각에서 참조되고 AEM에서 전달하는 이미지 자산

일반적으로 AEM Headless 앱은 GraphQL API 및 이미지 요청 모두에 대한 단일 AEM 서비스와 상호 작용합니다. AEM 서비스는 AEM Headless 앱 배포에 따라 변경됩니다.

| AEM Headless 배포 유형 | AEM 환경 | AEM 서비스 |
|-------------------------------|:---------------------:|:----------------:|
| 프로덕션 | 프로덕션 | 게시 |
| 작성 미리 보기 | 프로덕션 | 미리보기 |
| 개발 | 개발 | 게시 |

배포 유형 순열을 처리하기 위해 연결할 AEM 서비스를 지정하는 구성을 사용하여 각 앱 배포를 빌드합니다. 그런 다음 구성된 AEM 서비스의 호스트/도메인을 사용하여 AEM GraphQL API URL 및 이미지 URL을 구성합니다. 빌드 종속 구성을 관리하는 올바른 방법을 결정하려면 프레임워크에 따라 접근 방식이 다양하므로 AEM Headless 앱의 프레임워크(예: React, iOS, Android™ 등) 설명서를 참조하십시오.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM 호스트 구성 | ✔ | ✔ | ✔ | ✔ |

다음은 의 URL을 구성하는 가능한 접근 방식의 예입니다 [AEM GraphQL API](#aem-graphql-api-requests) 및 [이미지 요청](#aem-image-requests): 인기 있는 헤드리스 프레임워크 및 플랫폼에 대한 것입니다.

## AEM GraphQL API 요청

헤드리스 앱에서 AEM GraphQL API로 연결되는 HTTP GET 요청은 다음에 설명된 대로 올바른 AEM 서비스와 상호 작용하도록 구성해야 합니다 [위 표](#managing-aem-hosts).

사용 시 [AEM Headless SDK](../../how-to/aem-headless-sdk.md) (브라우저 기반 JavaScript, 서버 기반 JavaScript 및 Java™에 사용 가능), AEM 호스트는 연결할 AEM Service로 AEM Headless 클라이언트 개체를 초기화할 수 있습니다.

사용자 지정 AEM Headless 클라이언트를 개발할 때 AEM 서비스의 호스트가 빌드 매개 변수를 기반으로 매개 변수화할 수 있는지 확인합니다.

### 예

다음은 AEM GraphQL API 요청이 다양한 헤드리스 앱 프레임워크에 대해 AEM 호스트 값을 구성할 수 있는 방법의 예입니다.

+++ React 예

이 예제는 [AEM Headless React 앱](../../example-apps/react-app.md)에서는 환경 변수를 기반으로 다른 AEM 서비스에 연결하도록 AEM GraphQL API 요청을 구성하는 방법을 보여줍니다.

React 앱은 [JavaScript용 AEM Headless 클라이언트](../../how-to/aem-headless-sdk.md) AEM GraphQL API와 상호 작용할 수 있습니다. JavaScript용 AEM Headless 클라이언트가 제공하는 AEM Headless 클라이언트는 JavaScript가 연결되는 AEM 서비스 호스트로 초기화해야 합니다.

#### React 환경 파일

React 사용 [사용자 지정 환경 파일](https://create-react-app.dev/docs/adding-custom-environment-variables/), 또는 `.env` 빌드 특정 값을 정의하기 위해 프로젝트의 루트에 저장된 파일. 예: `.env.development` 파일에는 개발 중에 사용되는 값이 포함되어 있지만 `.env.production` 프로덕션 빌드에 사용되는 값을 포함합니다.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 다른 용도로 사용할 수 있는 파일 [지정할 수 있습니다.](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 포스트픽싱 `.env` 그리고 다음과 같은 의미론적 설명자 `.env.stage` 또는 `.env.production`. 다름 `.env` React 앱을 실행 또는 작성할 때, `REACT_APP_ENV` 실행 전 `npm` 명령.

예를 들어 React 앱의 `package.json` 다음을 포함할 수 있습니다. `scripts` 구성:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM 헤드리스 클라이언트

다음 [JavaScript용 AEM Headless 클라이언트](../../how-to/aem-headless-sdk.md) AEM GraphQL API에 대한 HTTP 요청을 수행하는 AEM Headless 클라이언트를 포함합니다. AEM Headless 클라이언트는 활성 상태의 값을 사용하여 상호 작용하는 AEM 호스트로 초기화해야 합니다 `.env` 파일.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) 후크

사용자 지정 React useEffect 후크는 보기를 렌더링하는 React 구성 요소를 대신하여 AEM 호스트로 초기화된 AEM Headless 클라이언트를 호출합니다.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### React 구성 요소

사용자 정의 사용 효과 후크, `useAdventureByPath` 는 가져온 후 AEM Headless 클라이언트를 사용하여 데이터를 가져오고 궁극적으로 최종 사용자에게 컨텐츠를 렌더링하는 데 사용됩니다.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™ 예

이 예제는 [예제 AEM Headless iOS™ 앱](../../example-apps/ios-swiftui-app.md)는 AEM GraphQL API 요청을 다음 기준에 따라 다른 AEM 호스트에 연결하도록 구성하는 방법을 보여줍니다 [빌드별 구성 변수](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™ 앱에서는 AEM GraphQL API와 상호 작용하려면 사용자 지정 AEM Headless 클라이언트가 필요합니다. AEM Headless 클라이언트를 작성해야 AEM 서비스 호스트를 구성할 수 있습니다.

#### 구성 작성

XCode 구성 파일에 기본 구성 세부 사항이 포함되어 있습니다.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 사용자 지정 AEM 헤드리스 클라이언트 초기화

다음 [AEM Headless iOS 앱 예제](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) 에서는 구성 값으로 초기화된 사용자 지정 AEM 헤드리스 클라이언트를 사용합니다. `AEM_SCHEME` 및 `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

사용자 지정 AEM 헤드리스 클라이언트(`api/Aem.swift`)에 메서드가 포함되어 있습니다. `makeRequest(..)` 이 접두사는 구성된 AEM으로 AEM GraphQL API 요청의 접두사를 지정합니다 `scheme` 및 `host`.

+ `api/Aem.swift`

```swift
/// #makeRequest(..)
/// Generic method for constructing and executing AEM GraphQL persisted queries
private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
    // Encode optional parameters as required by AEM
    let persistedQueryParams = params.map { (param) -> String in
        encode(string: ";\(param.key)=\(param.value)")
    }.joined(separator: "")
    
    // Construct the AEM GraphQL persisted query URL, including optional query params
    let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

    var request = URLRequest(url: URL(string: url)!);
    
    return request;
}
```

[새 빌드 구성 파일을 만들 수 있습니다](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 를 눌러 다른 AEM 서비스에 연결합니다. 에 대한 빌드별 값 `AEM_SCHEME` 및 `AEM_HOST` 는 XCode에서 선택한 빌드를 기반으로 하여 사용되어 사용자 지정 AEM Headless 클라이언트가 올바른 AEM 서비스와 연결할 수 있습니다.

+++

+++ Android™ 예

이 예제는 [예제 AEM Headless Android™ 앱](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)에서는 빌드별(또는 향) 구성 변수를 기반으로 다른 AEM Services에 연결하도록 AEM GraphQL API 요청을 구성하는 방법을 보여줍니다.

Android™ 앱(Java™으로 작성된 경우)은 [Java™용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) AEM GraphQL API와 상호 작용할 수 있습니다. Java™용 AEM Headless 클라이언트가 제공하는 AEM Headless 클라이언트는 연결된 AEM 서비스 호스트로 초기화해야 합니다.

#### 구성 파일 작성

Android™ 앱은 다양한 용도로 아티팩트를 작성하는 데 사용되는 &quot;productFlavors&quot;를 정의합니다.
이 예는 서로 다른 AEM 서비스 호스트(`AEM_HOST`) 개발 값(`dev`) 및 프로덕션( )`prod`)에서 사용할 수 있습니다.

앱의 `build.gradle` 파일, 새 `flavorDimension` 명명된 이름 `env` 가 만들어집니다.

에서 `env` 차원, 2 `productFlavors` 정의된 항목: `dev` 및 `prod`. 각 `productFlavor` 사용 `buildConfigField` 연결할 AEM 서비스를 정의하는 빌드별 변수를 설정하려면 다음을 수행하십시오.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™ 로더

초기화 `AEMHeadlessClient` Java™용 AEM Headless Client에서 제공하는 빌더 `AEM_HOST` 값에서 `buildConfigField` 필드.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

다양한 용도로 Android™ 앱을 빌드할 때 `env` 향과 해당 AEM 호스트 값이 사용됩니다.

+++

## AEM 이미지 URL

헤드리스 앱에서 AEM으로 이미지 요청은 다음 설명서에 설명된 대로 올바른 AEM 서비스와 상호 작용하도록 구성해야 합니다. [위 표](#managing-aem-hosts).

Adobe은 [최적화된 이미지](../../how-to/images.md) 사용 가능한 `_dynamicUrl` AEM GraphQL API의 필드. 다음 `_dynamicUrl` 필드는 AEM GraphQL API를 쿼리하는 데 사용되는 AEM 서비스 호스트 접두사로 사용할 수 있는 호스트가 없는 URL을 반환합니다. 대상 `_dynamicUrl` GraphQL 응답의 필드는 다음과 같습니다.

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 예

다음은 이미지 URL이 다양한 헤드리스 앱 프레임워크에 대해 구성할 수 있는 AEM 호스트 값에 접두사를 추가할 수 있는 방법의 예입니다. 이 예제에서는 `_dynamicUrl` 필드.

예:

#### GraphQL 지속적인 쿼리

이 GraphQL 쿼리는 이미지 참조의 `_dynamicUrl`. 에 표시됨 [GraphQL 응답](#examples-react-graphql-response) 호스트를 제외합니다.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL 응답

이 GraphQL 응답은 이미지 참조의 `_dynamicUrl` 호스트를 제외합니다.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ React 예

이 예제는 [AEM Headless React 앱 예](../../example-apps/react-app.md)에서는 환경 변수를 기반으로 올바른 AEM 서비스에 연결하도록 이미지 URL을 구성할 수 있는 방법을 보여줍니다.

다음 예에서는 이미지 참조의 접두사를 보여 줍니다 `_dynamicUrl` 필드, 구성 가능 `REACT_APP_AEM_HOST` React 환경 변수.

#### React 환경 파일

React 사용 [사용자 지정 환경 파일](https://create-react-app.dev/docs/adding-custom-environment-variables/), 또는 `.env` 빌드 특정 값을 정의하기 위해 프로젝트의 루트에 저장된 파일. 예: `.env.development` 파일에는 개발 중에 사용되는 값이 포함되어 있지만 `.env.production` 프로덕션 빌드에 사용되는 값을 포함합니다.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 다른 용도로 사용할 수 있는 파일 [지정할 수 있습니다.](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 포스트픽싱 `.env` 그리고 다음과 같은 의미론적 설명자 `.env.stage` 또는 `.env.production`. 다름 `.env` React 앱을 실행하거나 빌드할 때, `REACT_APP_ENV` 실행 전 `npm` 명령.

예를 들어 React 앱의 `package.json` 다음을 포함할 수 있습니다. `scripts` 구성:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### React 구성 요소

React 구성 요소는 `REACT_APP_AEM_HOST` 환경 변수 및 접두사 `_dynamicUrl` 값을 사용하여 이미지 URL을 확인합니다.

동일한 항목 `REACT_APP_AEM_HOST` 환경 변수는 `useAdventureByPath(..)` AEM에서 GraphQL 데이터를 가져오는 데 사용되는 사용자 지정 useEffect 후크입니다. 동일한 변수를 사용하여 GraphQL API 요청을 이미지 URL로 구성하려면 React 앱이 두 사용 사례 모두에 대해 동일한 AEM 서비스와 상호 작용하는지 확인하십시오.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™ 예

이 예제는 [예제 AEM Headless iOS™ 앱](../../example-apps/ios-swiftui-app.md)에서는 을(를) 기반으로 다른 AEM 호스트에 연결하도록 AEM 이미지 URL을 구성하는 방법을 보여줍니다 [빌드별 구성 변수](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### 구성 작성

XCode 구성 파일에 기본 구성 세부 사항이 포함되어 있습니다.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 이미지 URL 생성기

in `Aem.swift`, 사용자 지정 AEM 헤드리스 클라이언트 구현, 사용자 지정 함수 `imageUrl(..)` 에서 제공하는 대로 이미지 경로를 취합니다. `_dynamicUrl` 필드를 GraphQL 응답에 추가하고 AEM 호스트에 추가합니다. 이 함수는 이미지를 렌더링할 때마다 iOS 보기에서 호출됩니다.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS 보기

iOS 보기와 접두사가 이미지에 적용됩니다 `_dynamicUrl` 값을 사용하여 이미지 URL을 확인합니다.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[새 빌드 구성 파일을 만들 수 있습니다](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 를 눌러 다른 AEM 서비스에 연결합니다. 에 대한 빌드별 값 `AEM_SCHEME` 및 `AEM_HOST` 는 XCode에서 선택한 빌드를 기반으로 하여 사용되어 사용자 지정 AEM Headless 클라이언트가 올바른 AEM 서비스와 상호 작용합니다.

+++

+++ Android™ 예

이 예제는 [예제 AEM Headless Android™ 앱](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)에서는 빌드별(또는 향) 구성 변수를 기반으로 다른 AEM 서비스에 연결하도록 AEM 이미지 URL을 구성하는 방법을 보여줍니다.

#### 구성 파일 작성

Android™ 앱은 다양한 용도로 아티팩트를 작성하는 데 사용되는 &quot;productFlavors&quot;를 정의합니다.
이 예는 서로 다른 AEM 서비스 호스트(`AEM_HOST`) 개발 값(`dev`) 및 프로덕션( )`prod`)에서 사용할 수 있습니다.

앱의 `build.gradle` 파일, 새 `flavorDimension` 명명된 이름 `env` 가 만들어집니다.

에서 `env` 차원, 2 `productFlavors` 정의된 항목: `dev` 및 `prod`. 각 `productFlavor` 사용 `buildConfigField` 연결할 AEM 서비스를 정의하는 빌드별 변수를 설정하려면 다음을 수행하십시오.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### AEM 이미지 로드

Android™은 `ImageGetter` AEM에서 이미지 데이터를 가져오고 로컬로 캐시하려면 in `prepareDrawableFor(..)` 활성 빌드 구성에 정의된 AEM 서비스 호스트는 확인 가능한 URL을 만드는 이미지 경로의 접두사를 AEM에 추가하는 데 사용됩니다.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™ 보기

Android™ 보기는 다음을 통해 이미지 데이터를 가져옵니다 `RemoteImagesCache` 사용 `_dynamicUrl` 값을 생성합니다.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

다양한 용도로 Android™ 앱을 빌드할 때 `env` 향과 해당 AEM 호스트 값이 사용됩니다.

+++
