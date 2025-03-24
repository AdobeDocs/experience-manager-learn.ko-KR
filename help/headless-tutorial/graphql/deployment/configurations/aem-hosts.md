---
title: AEM GraphQL에 대한 AEM 호스트 관리
description: AEM Headless 앱에서 AEM 호스트를 구성하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# AEM 호스트 관리

AEM Headless 애플리케이션을 배포하려면 올바른 AEM 호스트/도메인이 사용되도록 AEM URL을 구성하는 방법에 주의를 기울여야 합니다. 알아야 하는 기본 URL/요청 유형은 다음과 같습니다.

+ __[AEM GraphQL API](#aem-graphql-api-requests)__&#x200B;에 대한 HTTP 요청
+ 콘텐츠 조각에서 참조하고 AEM에서 제공한 이미지 에셋에 대한 __[이미지 URL](#aem-image-urls)__

일반적으로 AEM Headless 앱은 GraphQL API 및 이미지 요청 모두에 대해 단일 AEM 서비스와 상호 작용합니다. AEM 서비스는 AEM Headless 앱 배포를 기반으로 다음과 같이 변경됩니다.

| AEM Headless 배포 유형 | AEM 환경 | AEM 서비스 |
|-------------------------------|:---------------------:|:----------------:|
| 프로덕션 | 프로덕션 | 게시 |
| 작성 미리보기 | 프로덕션 | 미리보기 |
| 개발 | 개발 | 게시 |

배포 유형 순열을 처리하기 위해 각 앱 배포는 연결할 AEM 서비스를 지정하는 구성을 사용하여 빌드됩니다. 그런 다음 구성된 AEM 서비스의 호스트/도메인을 사용하여 AEM GraphQL API URL 및 이미지 URL을 구성합니다. 빌드 종속 구성 관리에 대한 올바른 접근 방식을 결정하려면 프레임워크에 따라 접근 방식이 다르므로 AEM Headless 앱의 프레임워크(예: React, iOS™ Android 등) 설명서를 참조하십시오.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM 호스트 구성 | ✔ | ✔ | ✔ | ✔ |

다음은 인기 있는 여러 Headless 프레임워크 및 플랫폼에 대해 [AEM GraphQL API](#aem-graphql-api-requests) 및 [이미지 요청](#aem-image-requests)에 대한 URL을 구성하는 가능한 접근 방식의 예입니다.

## AEM GraphQL API 요청

위의 [표](#managing-aem-hosts)에 설명된 대로 Headless 앱에서 AEM의 GraphQL API로의 HTTP GET 요청은 올바른 AEM 서비스와 상호 작용하도록 구성해야 합니다.

[AEM Headless SDK](../../how-to/aem-headless-sdk.md)(브라우저 기반 JavaScript, 서버 기반 JavaScript 및 Java™에서 사용 가능)를 사용하는 경우 AEM 호스트는 연결할 AEM 서비스를 사용하여 AEM Headless 클라이언트 개체를 초기화할 수 있습니다.

사용자 지정 AEM Headless 클라이언트를 개발할 때 AEM 서비스의 호스트가 빌드 매개 변수를 기반으로 매개 변수화가 가능한지 확인하십시오.

### 예

다음은 AEM GraphQL API 요청에서 다양한 Headless 앱 프레임워크에 대해 AEM 호스트 값을 구성할 수 있도록 하는 방법의 예입니다.

+++ React 예

이 예제는 [AEM Headless React 앱](../../example-apps/react-app.md)을 기반으로 하며, 환경 변수를 기반으로 다른 AEM 서비스에 연결하도록 AEM GraphQL API 요청을 구성하는 방법을 보여 줍니다.

React 앱은 [JavaScript용 AEM Headless 클라이언트](../../how-to/aem-headless-sdk.md)를 사용하여 AEM의 GraphQL API와 상호 작용해야 합니다. JavaScript용 AEM Headless 클라이언트에서 제공하는 AEM Headless 클라이언트는 연결되는 AEM 서비스 호스트로 초기화되어야 합니다.

#### React 환경 파일

React는 프로젝트 루트에 저장된 [사용자 지정 환경 파일](https://create-react-app.dev/docs/adding-custom-environment-variables/) 또는 `.env`개 파일을 사용하여 빌드별 값을 정의합니다. 예를 들어 `.env.development` 파일에는 개발 중에 사용되는 값이 들어 있지만 `.env.production`에는 프로덕션 빌드에 사용되는 값이 들어 있습니다.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env`과(와) 의미 체계 설명자(예: `.env.stage` 또는 `.env.production`)를 후정하여 [다른 용도로 `.env`개 파일을 지정할 수 있습니다](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used). `npm` 명령을 실행하기 전에 `REACT_APP_ENV`을(를) 설정하여 React 앱을 실행하거나 빌드할 때 다른 `.env` 파일을 사용할 수 있습니다.

예를 들어 React 앱의 `package.json`에 다음 `scripts` 구성이 포함될 수 있습니다.

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

[JavaScript용 AEM Headless 클라이언트](../../how-to/aem-headless-sdk.md)에는 AEM의 GraphQL API에 대한 HTTP 요청을 하는 AEM Headless 클라이언트가 포함되어 있습니다. AEM Headless 클라이언트는 활성 `.env` 파일의 값을 사용하여 상호 작용하는 AEM 호스트로 초기화되어야 합니다.

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

#### 구성 요소 반응

사용자 지정 useEffect 후크 `useAdventureByPath`을(를) 가져와서 AEM Headless 클라이언트를 사용하여 데이터를 가져오고 최종 사용자에게 콘텐츠를 렌더링하는 데 사용합니다.

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

[예제 AEM Headless iOS™ 앱](../../example-apps/ios-swiftui-app.md)을 기반으로 하는 이 예제는 AEM GraphQL API 요청을 [빌드별 구성 변수](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)를 기반으로 다른 AEM 호스트에 연결하도록 구성하는 방법을 보여 줍니다.

iOS™ 앱을 사용하려면 사용자 지정 AEM Headless 클라이언트가 AEM의 GraphQL API와 상호 작용해야 합니다. AEM 서비스 호스트를 구성할 수 있도록 AEM Headless 클라이언트를 작성해야 합니다.

#### 빌드 구성

XCode 구성 파일에는 기본 구성 세부 사항이 포함되어 있습니다.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 사용자 지정 AEM Headless 클라이언트 초기화

[예제 AEM 헤드리스 iOS 앱](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)은(는) `AEM_SCHEME` 및 `AEM_HOST`에 대한 구성 값으로 초기화된 사용자 지정 AEM 헤드리스 클라이언트를 사용합니다.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

사용자 지정 AEM Headless 클라이언트(`api/Aem.swift`)에 구성된 AEM `scheme` 및 `host`(으)로 AEM GraphQL API 요청 앞에 오는 메서드 `makeRequest(..)`이(가) 포함되어 있습니다.

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

다른 AEM 서비스에 연결하려면 [새 빌드 구성 파일을 만들 수 있습니다](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3). `AEM_SCHEME` 및 `AEM_HOST`에 대한 빌드별 값은 XCode에서 선택한 빌드를 기반으로 사용되므로 사용자 지정 AEM Headless 클라이언트가 올바른 AEM 서비스와 연결합니다.

+++

+++ Android™ 예

[예제 AEM Headless Android™ 앱](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)을 기반으로 하는 이 예제는 빌드별(또는 버전) 구성 변수를 기반으로 다른 AEM 서비스에 연결하도록 AEM GraphQL API 요청을 구성하는 방법을 보여 줍니다.

Android™ 앱(Java™으로 작성된 경우)은 [AEM Headless Client for Java™](https://github.com/adobe/aem-headless-client-java)을 사용하여 AEM의 GraphQL API와 상호 작용해야 합니다. Java™용 AEM Headless 클라이언트에서 제공하는 AEM Headless 클라이언트는 연결되는 AEM 서비스 호스트로 초기화되어야 합니다.

#### 구성 파일 작성

Android™ 앱은 다양한 용도로 아티팩트를 빌드하는 데 사용되는 &quot;productFlavors&quot;를 정의합니다.
이 예는 개발(`dev`) 및 프로덕션(`prod`)에서 사용하는 서로 다른 AEM 서비스 호스트(`AEM_HOST`) 값을 제공하는 두 개의 Android™ 제품 버전을 정의하는 방법을 보여줍니다.

앱의 `build.gradle` 파일에 이름이 `env`인 새 `flavorDimension`이(가) 만들어집니다.

`env` 차원에서 `productFlavors` 두 개가 정의되었습니다. `dev` 및 `prod`. 각 `productFlavor`은(는) `buildConfigField`을(를) 사용하여 연결할 AEM 서비스를 정의하는 빌드별 변수를 설정합니다.

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

Java™용 AEM Headless 클라이언트에서 제공한 `AEMHeadlessClient` 빌더를 `buildConfigField` 필드의 `AEM_HOST` 값으로 초기화합니다.

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

다른 용도로 Android™ 앱을 빌드하는 경우 `env` 버전을 지정하고 해당 AEM 호스트 값을 사용합니다.

+++

## AEM 이미지 URL

[위의 표](#managing-aem-hosts)에 설명된 대로 AEM에 대한 Headless 앱의 이미지 요청은 올바른 AEM 서비스와 상호 작용하도록 구성해야 합니다.

Adobe에서는 AEM GraphQL API의 `_dynamicUrl` 필드를 통해 사용할 수 있는 [최적화된 이미지](../../how-to/images.md)를 사용할 것을 권장합니다. `_dynamicUrl` 필드는 AEM GraphQL API를 쿼리하는 데 사용되는 AEM 서비스 호스트에 접두사가 추가될 수 있는 호스트 없는 URL을 반환합니다. GraphQL 응답의 `_dynamicUrl` 필드에 대한 모습은 다음과 같습니다.

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 예

다음은 다양한 Headless 앱 프레임워크에 대해 구성할 수 있도록 만들어진 AEM 호스트 값에 이미지 URL이 접두사로 사용될 수 있는 방법의 예입니다. 예제에서는 `_dynamicUrl` 필드를 사용하여 이미지 참조를 반환하는 GraphQL 쿼리를 사용한다고 가정합니다.

예:

#### GraphQL 지속 쿼리

이 GraphQL 쿼리는 이미지 참조의 `_dynamicUrl`을(를) 반환합니다. 호스트를 제외하는 [GraphQL 응답](#examples-react-graphql-response)에서 볼 수 있습니다.

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

이 GraphQL 응답은 호스트를 제외하는 이미지 참조의 `_dynamicUrl`을(를) 반환합니다.

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

[예제 AEM Headless React 앱](../../example-apps/react-app.md)을 기반으로 하는 이 예제는 환경 변수를 기반으로 올바른 AEM 서비스에 연결하도록 이미지 URL을 구성하는 방법을 보여 줍니다.

이 예제에서는 구성 가능한 `REACT_APP_AEM_HOST` React 환경 변수와 함께 이미지 참조 `_dynamicUrl` 필드를 접두사로 사용하는 방법을 보여 줍니다.

#### React 환경 파일

React는 프로젝트 루트에 저장된 [사용자 지정 환경 파일](https://create-react-app.dev/docs/adding-custom-environment-variables/) 또는 `.env`개 파일을 사용하여 빌드별 값을 정의합니다. 예를 들어 `.env.development` 파일에는 개발 중에 사용되는 값이 들어 있지만 `.env.production`에는 프로덕션 빌드에 사용되는 값이 들어 있습니다.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env`과(와) 의미 체계 설명자(예: `.env.stage` 또는 `.env.production`)를 후정하여 [다른 용도로 `.env`개 파일을 지정할 수 있습니다](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used). `npm` 명령을 실행하기 전에 `REACT_APP_ENV`을(를) 설정하여 React 앱을 실행하거나 빌드할 때 다른 `.env` 파일을 사용할 수 있습니다.

예를 들어 React 앱의 `package.json`에 다음 `scripts` 구성이 포함될 수 있습니다.

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

#### 구성 요소 반응

React 구성 요소는 `REACT_APP_AEM_HOST` 환경 변수를 가져오고 이미지 `_dynamicUrl` 값 앞에 추가하여 완전히 해결 가능한 이미지 URL을 제공합니다.

동일한 `REACT_APP_AEM_HOST` 환경 변수를 사용하여 AEM에서 GraphQL 데이터를 가져오는 데 사용되는 `useAdventureByPath(..)` 사용자 지정 useEffect 후크에서 사용되는 AEM Headless 클라이언트를 초기화합니다. 동일한 변수를 사용하여 GraphQL API 요청을 이미지 URL로 구성합니다. React 앱이 두 사용 사례 모두에 대해 동일한 AEM 서비스와 상호 작용하는지 확인하십시오.

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

[예제 AEM Headless iOS™ 앱](../../example-apps/ios-swiftui-app.md)을 기반으로 하는 이 예제는 [빌드별 구성 변수](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)를 기반으로 다른 AEM 호스트에 연결하도록 AEM 이미지 URL을 구성하는 방법을 보여 줍니다.

#### 빌드 구성

XCode 구성 파일에는 기본 구성 세부 사항이 포함되어 있습니다.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 이미지 URL 생성기

사용자 지정 AEM Headless 클라이언트 구현인 `Aem.swift`에서 사용자 지정 함수 `imageUrl(..)`은(는) GraphQL 응답의 `_dynamicUrl` 필드에 제공된 대로 이미지 경로를 가져와서 AEM 호스트 앞에 추가합니다. 그런 다음 이미지가 렌더링될 때마다 iOS 보기에서 이 함수가 호출됩니다.

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

iOS 보기에서 이미지 `_dynamicUrl` 값을 접두사로 사용하여 확인할 수 있는 이미지 URL을 제공합니다.

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

다른 AEM 서비스에 연결하려면 [새 빌드 구성 파일을 만들 수 있습니다](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3). `AEM_SCHEME` 및 `AEM_HOST`에 대한 빌드별 값은 XCode에서 선택한 빌드를 기반으로 사용되므로 사용자 지정 AEM Headless 클라이언트가 올바른 AEM 서비스와 상호 작용합니다.

+++

+++ Android™ 예

[예제 AEM Headless Android™ 앱](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)을 기반으로 하는 이 예제는 빌드별(또는 버전) 구성 변수를 기반으로 다른 AEM 서비스에 연결하도록 AEM 이미지 URL을 구성하는 방법을 보여 줍니다.

#### 구성 파일 작성

Android™ 앱은 다양한 용도로 아티팩트를 빌드하는 데 사용되는 &quot;productFlavors&quot;를 정의합니다.
이 예는 개발(`dev`) 및 프로덕션(`prod`)에서 사용하는 서로 다른 AEM 서비스 호스트(`AEM_HOST`) 값을 제공하는 두 개의 Android™ 제품 버전을 정의하는 방법을 보여줍니다.

앱의 `build.gradle` 파일에 이름이 `env`인 새 `flavorDimension`이(가) 만들어집니다.

`env` 차원에서 `productFlavors` 두 개가 정의되었습니다. `dev` 및 `prod`. 각 `productFlavor`은(는) `buildConfigField`을(를) 사용하여 연결할 AEM 서비스를 정의하는 빌드별 변수를 설정합니다.

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

#### AEM 이미지 로드 중

Android™은 `ImageGetter`을(를) 사용하여 AEM에서 이미지 데이터를 가져오고 로컬로 캐시합니다. `prepareDrawableFor(..)`에서 활성 빌드 구성에 정의된 AEM 서비스 호스트를 사용하여 AEM에 해결 가능한 URL을 만드는 이미지 경로의 접두사가 됩니다.

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

Android™ 보기는 GraphQL 응답의 `_dynamicUrl` 값을 사용하여 `RemoteImagesCache`을(를) 통해 이미지 데이터를 가져옵니다.

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

다른 용도로 Android™ 앱을 빌드하는 경우 `env` 버전을 지정하고 해당 AEM 호스트 값을 사용합니다.

+++
