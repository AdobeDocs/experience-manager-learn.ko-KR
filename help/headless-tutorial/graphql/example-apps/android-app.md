---
title: Android 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 Android 애플리케이션은 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM as a Cloud Service Headless" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---

# Android 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 Android 애플리케이션은 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)는 GraphQL 쿼리를 실행하고 데이터를 Java 개체에 매핑하여 앱을 실행하는 데 사용됩니다.

![AEM Headless가 포함된 Android Java 앱](./assets/android-java-app/android-app.png)


GitHub에서 [소스 코드 보기](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Android 애플리케이션은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포를 사용하려면 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)을(를) 설치해야 합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

Android 응용 프로그램은 __AEM Publish__ 환경에 연결하도록 디자인되었지만, Android 응용 프로그램의 구성에서 인증을 제공하는 경우 AEM 작성자의 콘텐츠를 소싱할 수 있습니다.

## 사용 방법

1. `adobe/aem-guides-wknd-graphql` 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. [Android Studio](https://developer.android.com/studio)를 열고 `android-app` 폴더를 엽니다.
1. `app/src/main/assets/config.properties`에서 `config.properties` 파일을 수정하고 대상 AEM 환경과 일치하도록 `contentApi.endpoint`을(를) 업데이트합니다.

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __기본 인증__

   `contentApi.user` 및 `contentApi.password`은(는) WKND GraphQL 콘텐츠에 액세스하여 로컬 AEM 사용자를 인증합니다.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. [Android 가상 장치](https://developer.android.com/studio/run/managing-avds)(최소 API 28)를 다운로드합니다.
1. Android 에뮬레이터를 사용하여 앱을 빌드하고 배포합니다.


### AEM 환경에 연결

AEM 작성자 환경 [인증](https://github.com/adobe/aem-headless-client-java#using-authorization)에 연결해야 하는 경우. [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java)에서는 [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)을 사용할 수 있습니다. 토큰 기반 인증을 사용하려면 `AdventureLoader.java` 및 `AdventuresLoader.java`에서 클라이언트 빌더를 업데이트하십시오.

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## 코드

다음은 애플리케이션 작동에 사용되는 중요한 파일과 코드에 대한 간략한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)에서 찾을 수 있습니다.

### 지속 쿼리

AEM Headless 우수 사례에 따라 iOS 애플리케이션은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 응용 프로그램에서는 두 개의 지속 쿼리를 사용합니다.

+ 속성 집합이 포함된 AEM의 모든 모험을 반환하는 `wknd/adventures-all` 지속 쿼리입니다. 이 지속 쿼리는 초기 보기의 모험 목록을 구동합니다.

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
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ 전체 속성 집합이 있는 `slug`(모험을 고유하게 식별하는 사용자 지정 속성)이 단일 모험을 반환하는 `wknd/adventure-by-slug` 지속 쿼리입니다. 이 지속 쿼리는 모험 세부 사항 보기를 실행합니다.

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
          _dynamicUrl
          _path
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

### GraphQL 지속 쿼리 실행

AEM의 지속 쿼리는 HTTP GET을 통해 실행되므로, [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)를 사용하여 AEM에 대해 지속 GraphQL 쿼리를 실행하고 어드벤처 콘텐츠를 앱에 로드합니다.

각 지속 쿼리에는 AEM HTTP GET 끝점을 비동기적으로 호출하고 사용자 지정 정의 [데이터 모델](#data-models)을(를) 사용하여 모험 데이터를 반환하는 해당 &quot;loader&quot; 클래스가 있습니다.

+ `loader/AdventuresLoader.java`

  `wknd-shared/adventures-all` 지속 쿼리를 사용하여 응용 프로그램의 홈 화면에서 모험 목록을 가져옵니다.

+ `loader/AdventureLoader.java`

  `wknd-shared/adventure-by-slug` 지속 쿼리를 사용하여 `slug` 매개 변수를 통해 선택한 단일 모험 가져오기

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL 응답 데이터 모델{#data-models}

`Adventure.java`은(는) GraphQL 요청의 JSON 데이터로 초기화된 Java POJO이며 Android 애플리케이션의 보기에서 사용할 모험 모델을 생성합니다.

### 보기

Android 애플리케이션은 두 개의 보기를 사용하여 모바일 경험에서 어드벤처 데이터를 표시합니다.

+ `AdventureListFragment.java`

  `AdventuresLoader`을(를) 호출하고 반환된 모험을 목록에 표시합니다.

+ `AdventureDetailFragment.java`

  `AdventureListFragment` 보기에서 모험 선택을 통해 전달된 `slug` 매개 변수를 사용하여 `AdventureLoader`을(를) 호출하고 단일 모험 정보를 표시합니다.

### 원격 이미지

`loader/RemoteImagesCache.java`은(는) Android UI 요소와 함께 사용할 수 있도록 캐시에서 원격 이미지를 준비하는 데 도움이 되는 유틸리티 클래스입니다. 어드벤처 콘텐츠는 URL을 통해 AEM Assets의 이미지를 참조하며, 이 클래스는 해당 콘텐츠를 표시하는 데 사용됩니다.

## 추가 리소스

+ [AEM Headless 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ Java용 [AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)
