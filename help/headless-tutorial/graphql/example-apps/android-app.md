---
title: Android 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 Android 애플리케이션은 AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다.
version: Cloud Service
mini-toc-levels: 2
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 8b2c116ceb6ab8c3a009dcec6629c2e97d815b7b
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 4%

---

# Android 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 Android 애플리케이션은 AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다. 다음 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) GraphQL 쿼리를 실행하고 데이터를 Java 개체에 매핑하여 앱을 실행하는 데 사용됩니다.

![AEM Headless를 사용한 Android Java 앱](./assets/android-java-app/android-app.png)


보기 [GitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 사전 요구 사항 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Android 애플리케이션은 다음 AEM 배포 옵션을 사용하여 작동합니다. 모든 배포에는 [WKND 사이트 v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 를 사용하여 로컬 설정 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

Android 애플리케이션은 __AEM 게시__ 그러나 Android 애플리케이션의 구성에 인증이 제공된 경우 AEM 작성자의 컨텐츠를 소스 지정할 수 있습니다.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 폴더를 열고 `android-app`
1. 파일 수정 `config.properties` at `app/src/main/assets/config.properties` 및 업데이트 `contentApi.endpoint` target AEM 환경과 일치하려면:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __기본 인증__

   다음 `contentApi.user` 및 `contentApi.password` WKND GraphQL 컨텐츠에 액세스할 수 있는 로컬 AEM 사용자를 인증합니다.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 다운로드 [Android 가상 장치](https://developer.android.com/studio/run/managing-avds) (최소 API 28).
1. Android 에뮬레이터를 사용하여 앱을 빌드하고 배포합니다.


### AEM 환경에 연결

`10.0.2.2` is [특수 별칭 IP](https://developer.android.com/studio/run/emulator-networking) 에뮬레이터를 사용할 때 localhost용 `10.0.2.2:4502` 은 `localhost:4502`. AEM 게시 환경에 연결하는 경우(권장), 인증이 필요하지 않으며 `contentAPi.user` 및 `contentApi.password` 비워 둘 수 있습니다.

AEM 작성 환경에 연결하는 경우 [권한](https://github.com/adobe/aem-headless-client-java#using-authorization) 는 필수입니다. 기본적으로 응용 프로그램은 사용자 이름과 암호를 사용하는 기본 인증을 사용하도록 설정되어 있습니다 `admin:admin`. 다음 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 는 사용 기능을 제공합니다 [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 에서 토큰 기반 인증 업데이트 클라이언트 빌더를 사용하려면 `AdventureLoader.java` 및 `AdventuresLoader.java`:

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

다음은 애플리케이션 전원을 지원하는 데 사용되는 중요한 파일 및 코드에 대한 간략한 요약 정보입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

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

AEM 지속적인 쿼리는 HTTP GET을 통해 실행되므로 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) AEM에 대해 지속된 GraphQL 쿼리를 실행하고 모험 컨텐츠를 앱에 로드하는 데 사용됩니다.

각 지속적인 쿼리에는 AEM HTTP GET 종료 지점을 비동기적으로 호출하고 정의된 사용자 지정 항목을 사용하여 모험 데이터를 반환하는 해당 &quot;loader&quot; 클래스가 있습니다 [데이터 모델](#data-models).

+ `loader/AdventuresLoader.java`

   를 사용하여 애플리케이션의 홈 화면에서 모험 목록을 가져옵니다 `wknd-shared/adventures-all` 지속형 쿼리입니다.

+ `loader/AdventureLoader.java`

   하나의 모험으로 선택할 수 있습니다 `slug` 매개 변수, `wknd-shared/adventure-by-slug` 지속형 쿼리입니다.

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

`Adventure.java` 는 GraphQL 요청의 JSON 데이터로 초기화된 Java POJO이며 Android 애플리케이션의 보기에서 사용할 모험을 모델링합니다.

### 보기

Android 애플리케이션은 두 개의 보기를 사용하여 모바일 경험에 모험 데이터를 표시합니다.

+ `AdventureListFragment.java`

   를 호출합니다 `AdventuresLoader` 및 은 반환된 모험을 목록에 표시합니다.

+ `AdventureDetailFragment.java`

   를 호출합니다 `AdventureLoader` 사용 `slug` 매개 변수 `AdventureListFragment` 하나의 모험에 대한 세부 사항을 보고 표시합니다.

### 원격 이미지

`loader/RemoteImagesCache.java` 는 Android UI 요소와 함께 사용할 수 있도록 캐시에서 원격 이미지를 준비하는 데 도움이 되는 유틸리티 클래스입니다. 모험 콘텐츠는 URL을 통해 AEM Assets의 이미지를 참조하며 이 클래스를 사용하여 해당 콘텐츠를 표시합니다.

## 추가 리소스

+ [AEM 헤드리스 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)
