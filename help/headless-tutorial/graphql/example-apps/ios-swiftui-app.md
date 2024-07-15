---
title: iOS 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 iOS 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM as a Cloud Service Headless" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 iOS 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.

![AEM Headless가 포함된 iOS SwiftUI 앱](./assets/ios-swiftui-app/ios-app.png)

GitHub에서 [소스 코드 보기](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Xcode](https://developer.apple.com/xcode/)(macOS 필요)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

iOS 애플리케이션은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포를 사용하려면 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)을(를) 설치해야 합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR)를 사용하여 로컬 설정

iOS 응용 프로그램은 __AEM Publish__ 환경에 연결하도록 디자인되었지만, iOS 응용 프로그램의 구성에서 인증을 제공하는 경우 AEM 작성자의 콘텐츠를 소싱할 수 있습니다.

## 사용 방법

1. `adobe/aem-guides-wknd-graphql` 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. [Xcode](https://developer.apple.com/xcode/)을(를) 열고 `ios-app` 폴더를 엽니다.
1. `Config.xcconfig` 파일을 수정하고 대상 AEM Publish 서비스와 일치하도록 `AEM_SCHEME` 및 `AEM_HOST`을(를) 업데이트합니다.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   AEM 작성자에 연결하는 경우 `AEM_AUTH_TYPE` 및 지원 인증 속성을 `Config.xcconfig`에 추가하십시오.

   __기본 인증__

   `AEM_USERNAME` 및 `AEM_PASSWORD`은(는) WKND GraphQL 콘텐츠에 액세스하여 로컬 AEM 사용자를 인증합니다.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __토큰 인증__

   `AEM_TOKEN`은(는) WKND GraphQL 콘텐츠에 액세스할 수 있는 AEM 사용자를 인증하는 [액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)입니다.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Xcode를 사용하여 애플리케이션을 빌드하고 앱을 iOS 시뮬레이터에 배포합니다.
1. WKND 사이트의 모험 목록이 애플리케이션에 표시되어야 합니다. 모험을 선택하면 어드벤처 세부 정보가 열립니다. 모험 목록 보기에서 를 가져와서 AEM에서 데이터를 새로 고칩니다.

## 코드

다음은 iOS 애플리케이션이 빌드되는 방법, GraphQL 지속 쿼리를 사용하여 콘텐츠를 검색하기 위해 AEM Headless에 연결하는 방법 및 해당 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)에서 찾을 수 있습니다.

### 지속 쿼리

AEM Headless 우수 사례에 따라 iOS 애플리케이션은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 응용 프로그램에서는 두 개의 지속 쿼리를 사용합니다.

+ 속성 집합이 포함된 AEM의 모든 모험을 반환하는 `wknd/adventures-all` 지속 쿼리입니다. 이 지속 쿼리는 초기 보기의 모험 목록을 구동합니다.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ 전체 속성 집합이 있는 `slug`(모험을 고유하게 식별하는 사용자 지정 속성)이 단일 모험을 반환하는 `wknd/adventure-by-slug` 지속 쿼리입니다. 이 지속 쿼리는 모험 세부 사항 보기를 실행합니다.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

### GraphQL 지속 쿼리 실행

AEM의 지속 쿼리는 HTTP GET을 통해 실행되므로 Apollo와 같은 HTTP POST을 사용하는 일반적인 GraphQL 라이브러리를 사용할 수 없습니다. 대신 AEM에 대한 지속 쿼리 HTTP GET 요청을 실행하는 사용자 지정 클래스를 만듭니다.

`AEM/Aem.swift`은(는) AEM Headless와의 모든 상호 작용에 사용되는 `Aem` 클래스를 인스턴스화합니다. 패턴은 다음과 같습니다.

1. 각 지속 쿼리에는 해당 공개 함수(예: `getAdventures(..)` 또는 `getAdventureBySlug(..)`) 모험 데이터를 가져오기 위해 iOS 응용 프로그램의 보기를 호출합니다.
1. 공용 함수는 AEM Headless에 비동기 HTTP GET 요청을 호출하고 JSON 데이터를 반환하는 개인 함수 `makeRequest(..)`을(를) 호출합니다.
1. 각 퍼블릭 펀드는 JSON 데이터를 디코딩한 다음, Adventure 데이터를 보기에 반환하기 전에 필요한 확인 또는 변환을 수행합니다.

   + AEM의 GraphQL JSON 데이터는 AEM Headless가 반환한 JSON 개체에 매핑되는 `AEM/Models.swift`에 정의된 구조/클래스를 사용하여 디코딩됩니다.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
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

iOS에서는 입력 데이터 모델보다 JSON 개체 매핑을 선호합니다.

`src/AEM/Models.swift`은(는) AEM의 JSON 응답에서 반환된 AEM JSON 응답에 매핑되는 [디코딩 가능](https://developer.apple.com/documentation/swift/decodable) Swift 구조 및 클래스를 정의합니다.

### 보기

SwiftUI는 애플리케이션의 다양한 보기에 사용됩니다. Apple은 [SwiftUI를 사용하여 목록 작성 및 탐색](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)을 위한 시작 자습서를 제공합니다.

+ `WKNDAdventuresApp.swift`

  응용 프로그램의 항목이며 `aem.getAdventures()`을(를) 통해 모든 모험 데이터를 가져오는 데 `.onAppear` 이벤트 처리기를 사용하는 `AdventureListView`이(가) 포함되어 있습니다. 공유 `aem` 개체가 여기에서 초기화되어 다른 보기에 [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject)(으)로 노출됩니다.

+ `Views/AdventureListView.swift`

  `aem.getAdventures()`의 데이터를 기반으로 모험 목록을 표시하고 `AdventureListItemView`을(를) 사용하여 각 모험 목록 항목을 표시합니다.

+ `Views/AdventureListItemView.swift`

  모험 목록(`Views/AdventureListView.swift`)의 각 항목을 표시합니다.

+ `Views/AdventureDetailView.swift`

  제목, 설명, 가격, 활동 유형 및 기본 이미지를 포함한 모험의 세부 정보를 표시합니다. 이 보기는 `aem.getAdventureBySlug(slug: slug)`을(를) 사용하여 AEM에 전체 모험에 대한 세부 정보를 쿼리합니다. 여기서 `slug` 매개 변수는 선택 목록 행을 기준으로 전달됩니다.

### 원격 이미지

어드벤처 콘텐츠 조각에서 참조하는 이미지는 AEM에서 제공합니다. 이 iOS 앱은 GraphQL 응답의 경로 `_dynamicUrl` 필드를 사용하며 `AEM_SCHEME` 및 `AEM_HOST` 접두사를 사용하여 정규화된 URL을 만듭니다. AE SDK에 대해 개발하는 경우 `_dynamicUrl`이(가) null을 반환하므로 개발 대체 항목이 이미지의 `_path` 필드로 설정됩니다.

인증이 필요한 AEM의 보호된 리소스에 연결하는 경우 자격 증명을 이미지 요청에도 추가해야 합니다.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 및 [SDWebImage](https://github.com/SDWebImage/SDWebImage)은(는) `AdventureListItemView` 및 `AdventureDetailView` 보기에서 어드벤처 이미지를 채우는 AEM의 원격 이미지를 로드하는 데 사용됩니다.

`aem` 클래스(`AEM/Aem.swift`의)는 두 가지 방법으로 AEM 이미지의 사용을 용이하게 합니다.

1. `aem.imageUrl(path: String)`은(는) 보기에서 AEM 구성표 앞에 추가하고 이미지의 경로를 호스팅하여 정규화된 URL을 만드는 데 사용됩니다.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. `Aem`의 `convenience init(..)`이(가) iOS 응용 프로그램 구성을 기반으로 이미지 HTTP 요청에 대해 HTTP 권한 부여 헤더를 설정합니다.

   + __기본 인증__&#x200B;이 구성된 경우 기본 인증이 모든 이미지 요청에 첨부됩니다.

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

   + __토큰 인증__&#x200B;이 구성된 경우 토큰 인증이 모든 이미지 요청에 연결됩니다.

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

   + __인증이 구성되지 않은__&#x200B;경우 이미지 요청에 인증이 첨부되지 않습니다.

SwiftUI 네이티브 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage)에도 유사한 접근 방식을 사용할 수 있습니다. `AsyncImage`은(는) iOS 15.0 이상에서 지원됩니다.

## 추가 리소스

+ [AEM Headless 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI 목록 및 탐색 자습서](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
