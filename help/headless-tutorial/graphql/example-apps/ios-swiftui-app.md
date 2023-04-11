---
title: iOS 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 iOS 애플리케이션은 지속된 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 4%

---

# iOS 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 iOS 애플리케이션은 지속된 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다.

![AEM Headless를 사용한 iOS SwiftUI 앱](./assets/ios-swiftui-app/ios-app.png)

보기 [GitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 사전 요구 사항 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

+ [Xcode](https://developer.apple.com/xcode/) (macOS 필요)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

iOS 애플리케이션은 다음 AEM 배포 옵션을 사용하여 작동합니다. 모든 배포에는 [WKND 사이트 v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 를 사용하여 로컬 설정 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko-KR?lang=en#install-local-aem-instances)

iOS 애플리케이션은 __AEM 게시__ 그러나 iOS 애플리케이션의 구성에 인증이 제공되면 AEM 작성자의 컨텐츠를 소스 지정할 수 있습니다.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 폴더를 열고 `ios-app`
1. 파일 수정 `Config.xcconfig` 파일 및 업데이트 `AEM_SCHEME` 및 `AEM_HOST` 를 사용하여 Target AEM 게시 서비스와 일치시킬 수 있습니다.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   AEM 작성자에 연결하는 경우 `AEM_AUTH_TYPE` 인증 속성을 지원 `Config.xcconfig`.

   __기본 인증__

   다음 `AEM_USERNAME` 및 `AEM_PASSWORD` WKND GraphQL 컨텐츠에 대한 액세스 권한을 사용하여 로컬 AEM 사용자를 인증합니다.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __토큰 인증__

   다음 `AEM_TOKEN` is [액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) WKND GraphQL 컨텐츠에 액세스할 수 있는 AEM 사용자에게 인증을 받습니다.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Xcode를 사용하여 애플리케이션을 빌드하고 앱을 iOS 시뮬레이터에 배포합니다.
1. WKND 사이트의 모험 목록이 애플리케이션에 표시되어야 합니다. 모험을 선택하면 모험 세부 사항이 열립니다. 모험 목록 보기에서 AEM의 데이터를 새로 고치려면 가져옵니다.

## 코드

다음은 iOS 애플리케이션이 빌드되는 방법, AEM Headless에 연결하여 GraphQL 지속적인 쿼리를 사용하여 컨텐츠를 검색하는 방법 및 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 지속되는 쿼리

AEM Headless 우수 사례에 따라 iOS 애플리케이션은 AEM GraphQL 지속적인 쿼리를 사용하여 모험 데이터를 쿼리합니다. 이 응용 프로그램에서는 다음과 같은 두 가지 지속적인 쿼리를 사용합니다.

+ `wknd/adventures-all` AEM의 모든 모험을 반환하는 질의가 완료된 속성 집합을 사용하여 유지됩니다. 이렇게 지속된 쿼리가 초기 보기의 모험 목록을 구동합니다.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 지속형 쿼리, `slug` 전체 속성 세트를 사용하여 모험을 고유하게 식별하는 사용자 지정 속성입니다. 이 지속되는 쿼리는 모험 세부 사항 보기를 가능하게 합니다.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL 지속적인 쿼리 실행

AEM 지속적인 쿼리는 HTTP GET을 통해 실행되므로 Apollo와 같은 HTTP POST을 사용하는 일반적인 GraphQL 라이브러리는 사용할 수 없습니다. 대신 AEM에 대한 지속적인 쿼리 HTTP GET 요청을 실행하는 사용자 지정 클래스를 만듭니다.

`AEM/Aem.swift` 인스턴스화 `Aem` AEM Headless를 사용하는 모든 상호 작용에 사용되는 클래스입니다. 패턴은 다음과 같습니다.

1. 지속되는 각 쿼리에는 해당 공용 함수(예: )가 있습니다. `getAdventures(..)` 또는 `getAdventureBySlug(..)`) iOS 애플리케이션의 보기를 호출하여 모험 데이터를 가져옵니다.
1. 공용 함수는 전용 함수를 호출합니다 `makeRequest(..)` 는 AEM Headless에 대한 비동기 HTTP GET 요청을 호출하고 JSON 데이터를 반환합니다.
1. 각 공용 함수는 JSON 데이터를 디코딩하고 필요한 검사나 변형을 수행한 후 Adventure 데이터를 보기로 반환합니다.

   + AEM GraphQL JSON 데이터는 `AEM/Models.swift`: JSON 개체에 매핑되어 내 AEM Headless를 반환했습니다.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }

    ...

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

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL 응답 데이터 모델

iOS에서는 형식화된 데이터 모델에 JSON 개체를 매핑하는 것을 선호합니다.

다음 `src/AEM/Models.swift` 는 을 정의합니다. [탈착](https://developer.apple.com/documentation/swift/decodable) AEM JSON 응답에서 반환된 AEM JSON 응답에 매핑되는 Swift 구조체 및 클래스입니다.

### 보기

SwiftUI는 애플리케이션의 다양한 보기에 사용됩니다. Apple은 용 시작 자습서를 제공합니다 [swiftUI를 사용하여 목록 및 탐색 만들기](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   애플리케이션 항목에는 `AdventureListView` 누구 `.onAppear` 이벤트 처리기는 를 통해 모든 모험 데이터를 가져오는 데 사용됩니다. `aem.getAdventures()`. 공유 `aem` 개체는 여기에서 초기화되며 다른 보기에 [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   다음 데이터의 데이터를 기반으로 모험 목록을 표시합니다 `aem.getAdventures()`)을 사용하여 각 모험에 대한 목록 항목을 표시합니다 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   모험 목록에 각 항목을 표시합니다(`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   제목, 설명, 가격, 활동 유형 및 기본 이미지를 포함하여 모험의 세부 사항을 표시합니다. 이 보기는 AEM에서 `aem.getAdventureBySlug(slug: slug)`, 여기서 `slug` 선택 목록 행에 따라 매개 변수가 전달됩니다.

### 원격 이미지

모험 컨텐츠 조각에서 참조하는 이미지는 AEM에서 제공합니다. 이 iOS 앱에서는 경로를 사용합니다 `_path` GraphQL 응답의 필드 및 접두사 `AEM_SCHEME` 및 `AEM_HOST` 를 입력하여 정규화된 URL을 생성합니다.

인증이 필요한 AEM에서 보호된 리소스에 연결하는 경우 이미지 요청에도 자격 증명을 추가해야 합니다.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 및 [SDWebImage](https://github.com/SDWebImage/SDWebImage) AEM에서 Adventure 이미지를 채우는 원격 이미지를 로드하는 데 사용됩니다. `AdventureListItemView` 및 `AdventureDetailView` 보기.

다음 `aem` 클래스(인) `AEM/Aem.swift`)을 사용하면 두 가지 방법으로 AEM 이미지를 사용할 수 있습니다.

1. `aem.imageUrl(path: String)` 는 이미지의 경로를 앞에 추가하고 호스팅하기 위해 보기에 사용되며 정규화된 URL을 만듭니다.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. 다음 `convenience init(..)` in `Aem` iOS 애플리케이션 구성을 기반으로 이미지 HTTP 요청에서 HTTP 인증 헤더를 설정합니다.

   + If __기본 인증__ 가 구성되어 있으면 기본 인증이 모든 이미지 요청에 연결됩니다.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + If __토큰 인증__ 가 구성되어 있으면 토큰 인증이 모든 이미지 요청에 연결됩니다.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + If __인증 없음__ 가 구성되면 이미지 요청에 대한 인증이 첨부되지 않습니다.



SwiftUI 기반의 네이티브 와 유사한 접근 방식을 사용할 수 있습니다 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` 은 iOS 15.0+에서 지원됩니다.

## 추가 리소스

+ [AEM Headless 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ko-KR)
+ [SwiftUI 목록 및 탐색 자습서](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
