---
title: iOS SwiftUI 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여주는 iOS 애플리케이션이 제공됩니다. Apollo Client iOS은 GraphQL 쿼리를 생성하고 데이터를 Swift 개체에 매핑하여 앱을 작동시키는 데 사용됩니다. SwiftUI는 컨텐츠의 간단한 목록 및 세부 사항 보기를 렌더링하는 데 사용됩니다.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 3%

---


# iOS SwiftUI 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 iOS 애플리케이션은 AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다. Apollo Client iOS은 GraphQL 쿼리를 생성하고 데이터를 Swift 개체에 매핑하여 앱을 작동시키는 데 사용됩니다. SwiftUI는 컨텐츠의 간단한 목록 및 세부 사항 보기를 렌더링하는 데 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## 전제 조건 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## AEM 요구 사항

응용 프로그램은 AEM에 연결하도록 설계되었습니다 **게시** 최신 릴리스가 있는 환경 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치되었습니다.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=ko-KR)

추천합니다 [Cloud Service 환경에 WKND 참조 사이트 배포](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 을 사용하여 로컬 설정 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 또는 [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 사용할 수도 있습니다.

## 사용 방법

1. 복제 `aem-guides-wknd-graphql` 저장소:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 폴더를 열고 `ios-swiftui-app`
1. 파일 수정 `Config.xcconfig` 파일 및 업데이트 `AEM_HOST` target AEM 게시 환경과 일치시키기 위해

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Xcode를 사용하여 애플리케이션을 빌드하고 앱을 iOS 시뮬레이터에 배포합니다.
1. WKND 참조 사이트의 모험 목록이 애플리케이션에 표시되어야 합니다.

## 코드

다음은 애플리케이션 전원을 지원하는 데 사용되는 중요한 파일 및 코드에 대한 간략한 요약 정보입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### 아폴로 iOS

다음 [아폴로 iOS](https://www.apollographql.com/docs/ios/) 클라이언트는 앱에서 AEM에 대해 GraphQL 쿼리를 실행하는 데 사용됩니다. 공식 [아폴로 자습서](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) 는 설치 및 사용 방법에 대해 자세히 설명합니다.

`schema.json` 는 WKND 참조 사이트가 설치된 AEM 환경에서 GraphQL 스키마를 나타내는 파일입니다. `schema.json` 이 AEM에서 다운로드되어 프로젝트에 추가되었습니다. Apollo 클라이언트는 확장자를 사용하여 모든 파일을 검사합니다 `.graphql` 사용자 지정 빌드 단계의 일부로 사용됩니다. 그런 다음 Apollo 클라이언트가 `schema.json` 파일 및 모든 파일 `.graphql` 파일을 자동으로 생성하는 쿼리 `API.swift`.

이렇게 하면 결과를 나타내는 쿼리 및 모델을 실행할 수 있도록 강력한 형식의 모델이 응용 프로그램에 제공됩니다.

![Xcode 사용자 지정 빌드 단계](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` 모험을 쿼리하는 데 사용되는 쿼리를 포함합니다.

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` 구문 `ApolloClient`. 다음 `endpointURL` 는 `Config.xcconfig` 파일. AEM에 연결하려면 **작성자** 인스턴스 및 인증을 위한 추가 헤더를 추가해야 하는 경우 `ApolloClient` 여기 있습니다.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### Adventure Data

이 응용 프로그램은 모험의 목록과 각 모험의 세부 보기를 표시하도록 디자인되었습니다.

`AdventuresDataModel.swift` 는 함수를 포함하는 클래스입니다 `fetchAdventures()`. 이 함수는 `ApolloClient` 쿼리를 실행하려면 성공적인 쿼리 시 결과 배열은 유형이 됩니다 `AdventureListQuery.Data.AdventureList.Item`에 의해 자동으로 생성됨 `API.swift` 파일.

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

사용이 가능합니다 `AdventureListQuery.Data.AdventureList.Item` 응용 프로그램을 실행할 수 있습니다. 그러나 일부 데이터가 불완전하여 일부 속성이 null일 수 있습니다.

`Adventure.swift` 는 Apollo에서 생성된 모델의 래퍼 역할을 하는 사용자 지정 모델입니다. `Adventure` 는 `AdventureListQuery.Data.AdventureList.Item`. A `typealias` 를 사용하여 코드를 보다 읽기 쉽게 만들 수 있습니다.

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

다음 `Adventure` 구조체가 `AdventureData` 개체:

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

이를 통해 기본값을 제공하고 불완전한 데이터에 대한 추가 검사를 수행할 수 있습니다. 그런 다음 `Adventure` 다양한 UI 요소를 작동시키기 위해 안전하게 모델링하고 null 값을 지속적으로 확인할 필요가 없습니다.

AEM에서 컨텐츠 조각은 `_path`. in `Adventure.swift` 우리는 `id` 값이 인 속성 `_path`. 이렇게 하면 `Adventure` 를 `Identifiable` 인터페이스 및 를 사용하면 배열이나 목록을 보다 쉽게 반복할 수 있습니다.

### 보기

SwiftUI는 애플리케이션의 다양한 보기에 사용됩니다. 유용한 [목록 작성 및 탐색](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) Apple의 개발자 사이트에서 찾을 수 있습니다. 이 응용 프로그램의 코드는 이 응용 프로그램에서 느슨하게 파생됩니다.

`WKNDAdventuresApp.swift` 은 애플리케이션의 항목입니다. 여기에는 다음이 포함됩니다 `AdventureListView` 그리고 `.onAppear` 이벤트는 모험 데이터를 가져오는 데 사용됩니다.

`AdventureListView.swift` - 만들기 `NavigationView` 그리고 `AdventureRowView`. 탐색 `AdventureDetailView` 이(가) 여기에서 설정되어 있습니다.

`AdventureRowView` - 모험의 1차 이미지와 어드벤처 제목을 연속적으로 표시합니다.

`AdventureDetailView` - 제목, 설명, 가격, 활동 유형 및 기본 이미지를 포함하여 개별 모험에 대한 전체 세부 사항을 표시합니다.

Apollo CLI가 실행되고 재생성되면 `API.swift` 이로 인해 미리 보기가 중지됩니다. 자동 미리 보기 기능을 사용하려면 **아폴로 CLI** 단계 작성 및 스크립트 실행 확인 **설치 빌드만**.

![설치 빌드만 확인](assets/ios-swiftui-app/update-build-phases.png)

### 원격 이미지

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 및 [SDWEbImage](https://github.com/SDWebImage/SDWebImage) 행 및 세부 사항 보기에서 Adventure 기본 이미지를 채우는 AEM의 원격 이미지를 로드하는 데 사용됩니다.

다음 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) 도 사용할 수 있는 기본 SwiftUI 보기입니다. `AsyncImage` 은 iOS 15.0+에서만 지원됩니다.

## 추가 리소스

* [AEM 헤드리스 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [SwiftUI 목록 및 탐색 자습서](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Apollo iOS 클라이언트 자습서](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

