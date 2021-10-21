---
title: Android 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여 주는 Android 애플리케이션이 제공됩니다. Apollo Client Android는 GraphQL 쿼리를 생성하고 데이터를 Swift 개체에 매핑하여 앱을 작동시키는 데 사용됩니다. SwiftUI는 컨텐츠의 간단한 목록 및 세부 사항 보기를 렌더링하는 데 사용됩니다.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 4%

---


# Android SwiftUI 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. AEM의 GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여 주는 Android 애플리케이션이 제공됩니다. 다음 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) GraphQL 쿼리를 실행하고 데이터를 Java 개체에 매핑하여 앱을 실행하는 데 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## 전제 조건 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

* [Android Studio](https://developer.android.com/studio)
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

1. Launch [Android Studio](https://developer.android.com/studio) 폴더를 열고 `android-app`
1. 파일 수정 `config.properties` at `app/src/main/assets/config.properties` 및 업데이트 `contentApi.endpoint` target AEM 환경과 일치하려면:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 다운로드 [Android 가상 장치](https://developer.android.com/studio/run/managing-avds) (최소 API 28)
1. Android 에뮬레이터를 사용하여 앱을 빌드하고 배포합니다.


### AEM 환경에 연결

`10.0.2.2` is [특수 별칭](https://developer.android.com/studio/run/emulator-networking) 에뮬레이터를 사용할 때 localhost에 사용됩니다. 그래서 `10.0.2.2:4502` 은 `localhost:4502`. AEM 게시 환경에 연결하는 경우(권장), 인증이 필요하지 않으며 `contentAPi.user` 및 `contentApi.password` 비워 둘 수 있습니다.

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

### 컨텐츠 가져오기

다음 [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java) 앱에서 AEM에 대해 GraphQL 쿼리를 실행하고 모험 컨텐츠를 앱에 로드하는 데 사용됩니다.

`AdventuresLoader.java` 은 애플리케이션의 홈 화면에서 Adventure의 초기 목록을 가져와서 로드하는 파일입니다. 이것은 [지속되는 쿼리](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) 다음 중 [사전 패키지](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) WKND 참조 사이트 사용. 끝점은 다음과 같습니다. `/wknd/adventures-all`. `AEMHeadlessClientBuilder` 에 설정된 api 끝점을 기반으로 새 인스턴스를 인스턴스화합니다. `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` 은 각 세부 사항 보기에 대해 Adventure 컨텐츠를 가져오고 로드하는 파일입니다. 다시 `AEMHeadlessClient` 쿼리를 실행하는 데 사용됩니다. 일반 graphQL 쿼리는 Adventure 컨텐츠 조각의 경로를 기반으로 실행됩니다. 쿼리는 이름이 인 별도의 파일에서 유지됩니다 [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` 는 GraphQL 요청의 JSON 데이터로 초기화되는 POJO입니다.

`RemoteImagesCache.java` 는 Android UI 요소와 함께 사용할 수 있도록 캐시에서 원격 이미지를 준비하는 데 도움이 되는 유틸리티 클래스입니다. Adventure 콘텐츠는 URL을 통해 AEM Assets의 이미지를 참조하며 이 클래스는 해당 콘텐츠를 표시하는 데 사용됩니다.

### 보기

`AdventureListFragment.java` - 를 호출할 때 트리거됩니다 `AdventuresLoader` 및 은 반환된 모험을 목록에 표시합니다.

`AdventureDetailFragment.java` - 초기화 `AdventureLoader` 그리고 하나의 모험에 대한 세부 사항을 표시합니다.

## 추가 리소스

* [AEM 헤드리스 시작하기 - GraphQL 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Java용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-java)

