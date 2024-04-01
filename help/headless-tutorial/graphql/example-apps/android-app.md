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
badgeVersions: label="AEM 헤드리스 as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 190
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---

# Android 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 Android 애플리케이션은 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 다음 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) 는 GraphQL 쿼리를 실행하고 데이터를 Java 개체에 매핑하여 앱을 구동하는 데 사용됩니다.

![AEM Headless가 포함된 Android Java 앱](./assets/android-java-app/android-app.png)


보기 [gitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Android 스튜디오](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Android 애플리케이션은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포에는 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

Android 애플리케이션은 __AEM 게시__ 그러나 Android 애플리케이션의 구성에서 인증이 제공되는 경우 AEM 작성자의 콘텐츠를 소싱할 수 있습니다.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 열기 [Android 스튜디오](https://developer.android.com/studio) 및 폴더 열기 `android-app`
1. 파일 수정 `config.properties` 위치: `app/src/main/assets/config.properties` 및 업데이트 `contentApi.endpoint` target AEM 환경과 일치시키려면 다음을 수행하십시오.

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __기본 인증__

   다음 `contentApi.user` 및 `contentApi.password` wknd GraphQL 콘텐츠에 액세스하여 로컬 AEM 사용자를 인증합니다.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. 다운로드 [Android 가상 장치](https://developer.android.com/studio/run/managing-avds) (최소 API 28).
1. Android 에뮬레이터를 사용하여 앱을 빌드하고 배포합니다.


### AEM 환경에 연결

AEM 작성자 환경에 연결하는 경우 [authorization](https://github.com/adobe/aem-headless-client-java#using-authorization) 필수 항목입니다. 다음 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 는 을 사용할 수 있는 기능을 제공합니다. [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 에서 토큰 기반 인증 업데이트 클라이언트 빌더를 사용하려면 `AdventureLoader.java` 및 `AdventuresLoader.java`:

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

다음은 애플리케이션 작동에 사용되는 중요한 파일과 코드에 대한 간략한 요약입니다. 전체 코드는에서 찾을 수 있습니다 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 지속 쿼리

AEM Headless 우수 사례에 따라 iOS 애플리케이션은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 응용 프로그램에서는 두 개의 지속 쿼리를 사용합니다.

+ `wknd/adventures-all` 지속 쿼리 - 속성 세트가 간략히 포함되어 AEM의 모든 모험을 반환합니다. 이 지속 쿼리는 초기 보기의 모험 목록을 구동합니다.

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

+ `wknd/adventure-by-slug` 지속 쿼리 - 단일 모험 반환 기준 `slug` (모험을 고유하게 식별하는 사용자 지정 속성) 전체 속성 세트를 포함합니다. 이 지속 쿼리는 모험 세부 사항 보기를 실행합니다.

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

AEM 지속 쿼리는 HTTP GET을 통해 실행되므로 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) AEM에 대해 지속 GraphQL 쿼리를 실행하고 어드벤처 콘텐츠를 앱에 로드하는 데 사용됩니다.

각 지속 쿼리에는 AEM HTTP GET 끝점을 비동기적으로 호출하고 정의된 사용자 지정을 사용하여 모험 데이터를 반환하는 해당 &quot;로더&quot; 클래스가 있습니다 [데이터 모델](#data-models).

+ `loader/AdventuresLoader.java`

  를 사용하여 애플리케이션의 홈 화면에서 모험 목록을 가져옵니다. `wknd-shared/adventures-all` 지속 쿼리.

+ `loader/AdventureLoader.java`

  다음을 통해 선택한 단일 모험 가져오기 `slug` 매개 변수, 사용 `wknd-shared/adventure-by-slug` 지속 쿼리.

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

`Adventure.java` 는 GraphQL 요청의 JSON 데이터로 초기화된 Java POJO이며, Android 애플리케이션의 보기에서 사용할 모험 모델을 생성합니다.

### 보기

Android 애플리케이션은 두 개의 보기를 사용하여 모바일 경험에서 어드벤처 데이터를 표시합니다.

+ `AdventureListFragment.java`

  호출 `AdventuresLoader` 반환된 모험을 목록에 표시합니다.

+ `AdventureDetailFragment.java`

  호출 `AdventureLoader` 사용 `slug` 에서 모험 선택을 통해 전달된 매개 변수 `AdventureListFragment` 단일 모험의 세부 정보를 보고 표시합니다.

### 원격 이미지

`loader/RemoteImagesCache.java` 는 Android UI 요소와 함께 사용할 수 있도록 캐시에서 원격 이미지를 준비하는 데 도움이 되는 유틸리티 클래스입니다. 어드벤처 콘텐츠는 URL을 통해 AEM Assets의 이미지를 참조하며, 이 클래스는 해당 콘텐츠를 표시하는 데 사용됩니다.

## 추가 리소스

+ [AEM Headless 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)
