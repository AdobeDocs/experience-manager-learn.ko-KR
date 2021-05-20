---
title: 개발자 콘솔
description: AEM as a Cloud Service은 디버깅에 유용한 실행 중인 AEM 서비스의 다양한 세부 사항을 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: 048a37a9813e7b61ff069c4606b8d23cc6b6844f
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 0%

---


# 개발자 콘솔을 사용하여 AEM as a Cloud Service 디버깅

AEM as a Cloud Service은 디버깅에 유용한 실행 중인 AEM 서비스의 다양한 세부 사항을 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.

각 AEM as a Cloud Service 환경에는 자체 개발자 콘솔이 있습니다.

## 개발자 콘솔 액세스

개발자 콘솔에 액세스하고 사용하려면 [Adobe의 Admin Console](https://adminconsole.adobe.com)를 통해 개발자의 Adobe ID에 다음 권한을 부여해야 합니다.

1. Cloud Service 제품으로 Cloud Manager 및 AEM을 사용한 Adobe 조직이 Adobe 조직 전환기에서 활성화되어 있는지 확인합니다.
1. 개발자는 Cloud Manager 제품 __개발자 - Cloud Service__ 제품 프로필의 구성원이어야 합니다.
   + 이 멤버십이 없으면 개발자는 개발자 콘솔에 로그인할 수 없습니다.
1. 개발자는 AEM 작성자 및/또는 게시에서 __AEM 사용자__ 또는 __AEM 관리자__ 제품 프로필의 구성원이어야 합니다.
   + 이 멤버십이 없으면 [status](#status) 덤프가 401 권한 없는 오류로 시간 초과됩니다.

### 개발자 콘솔 액세스 문제 해결

#### 401 상태를 덤핑하는 동안 권한 없는 오류가 발생했습니다.

![개발자 콘솔 - 401 권한 없음](./assets/developer-console/troubleshooting__401-unauthorized.png)

상태를 덤프할 때 401 허가되지 않은 오류가 보고되면 사용자가 아직 AEM에서 Cloud Service으로 필요한 권한이 없거나 로그인 토큰 사용이 잘못되었거나 만료되었음을 의미합니다.

401 권한 없는 문제를 해결하려면

1. 사용자가 Cloud Service 제품 인스턴스로 개발자 콘솔의 연결된 AEM에 대한 적절한 Adobe IMS 제품 프로필(AEM 관리자 또는 AEM 사용자)의 구성원인지 확인합니다.
   + 개발자 콘솔에서 2 Adobe IMS 제품 인스턴스에 액세스합니다.AEM as a Cloud Service 작성자 및 게시 제품 인스턴스이므로, 개발자 콘솔을 통해 액세스해야 하는 서비스 계층에 따라 올바른 제품 프로필이 사용되는지 확인하십시오.
1. AEM as a Cloud Service(작성자 또는 게시)에 로그인하고 사용자 및 그룹이 AEM에 제대로 동기화되었는지 확인합니다.
   + 개발자 콘솔에서는 해당 서비스 계층을 인증하기 위해 해당 AEM 서비스 계층에 사용자 레코드를 만들어야 합니다.
1. 브라우저 쿠키와 애플리케이션 상태(로컬 저장소)를 지우고 Developer Console에 다시 로그인하여 개발자 콘솔에서 사용 중인 액세스 토큰이 올바르고 만료되지 않도록 합니다.

## Pod

AEM as a Cloud Service 작성자 및 게시 서비스는 다운타임 없이 트래픽 가변성과 롤링 업데이트를 처리하기 위해 각각 여러 인스턴스로 구성됩니다. 이러한 인스턴스를 Pod라고 합니다. 개발자 콘솔의 Pod selection은 다른 컨트롤을 통해 노출되는 데이터의 범위를 정의합니다.

![개발자 콘솔 - Pod](./assets/developer-console/pod.png)

+ Pod는 AEM 서비스(작성자 또는 게시)의 일부인 개별 인스턴스입니다
+ pod 는 일시적입니다. 즉, Cloud Service으로서 AEM이 필요에 따라 만들고 소멸합니다
+ Cloud Service 환경으로서 연결된 AEM에 속하는 포드만 해당 환경의 개발자 콘솔의 Pod 전환기에 나열됩니다.
+ Pod switcher 하단에 있는 서비스 유형별로 Pod를 선택할 수 있는 편의 옵션을 제공합니다.
   + 모든 작성자
   + 모든 게시자
   + 모든 인스턴스

## 상태

상태는 텍스트 또는 JSON 출력에서 특정 AEM 런타임 상태를 출력하는 옵션을 제공합니다. 개발자 콘솔은 AEM SDK의 로컬 빠른 시작의 OSGi 웹 콘솔과 유사한 정보를 제공하며 개발자 콘솔이 읽기 전용이라는 표시된 차이를 제공합니다.

![개발자 콘솔 - 상태](./assets/developer-console/status.png)

### 번들

번들은 AEM의 모든 OSGi 번들을 나열합니다. 이 기능은 [AEM SDK의 로컬 빠른 시작인 OSGi Bundles](http://localhost:4502/system/console/bundles) (`/system/console/bundles`에 있음)와 유사합니다.

다음 방법으로 디버깅하는 데 도움이 되는 번들:

+ AEM as a Service에 배포된 모든 OSGi 번들 나열
+ 각 OSGi 번들 상태 나열활성 상태인지 여부를 포함합니다.
+ OSGi 번들이 활성화되지 않도록 하는 해결되지 않은 종속성에 세부 정보 제공

### 구성 요소

구성 요소는 AEM의 모든 OSGi 구성 요소를 나열합니다. 이 기능은 [AEM SDK의 로컬 빠른 시작인 OSGi Components](http://localhost:4502/system/console/components)(`/system/console/components`에 있음)와 유사합니다.

다음 방법으로 디버깅하는 데 도움이 되는 구성 요소:

+ AEM에 Cloud Service으로 배포된 모든 OSGi 구성 요소 나열
+ 각 OSGi 구성 요소의 상태 제공;활성 상태이거나 충족되지 않은 경우 포함
+ 서비스 참조에 세부 사항을 제공하면 OSGi 구성 요소가 활성 상태가 될 수 있습니다
+ OSGi 속성 및 OSGi 구성 요소에 바인딩된 값을 나열하는 중입니다

### 구성

구성은 모든 OSGi 구성 요소의 구성(OSGi 속성 및 값)을 나열합니다. 이 기능은 [AEM SDK의 로컬 빠른 시작에서 OSGi 구성 관리자](http://localhost:4502/system/console/configMgr)(`/system/console/configMgr`에 있음)와 유사합니다.

Configuration은 다음 방법으로 디버깅하는 데 도움이 됩니다.

+ OSGi 구성 요소별로 OSGi 속성 및 해당 값 나열
+ 잘못 구성된 속성 찾기 및 식별

### Oak 인덱스

Oak 인덱스는 `/oak:index` 아래에 정의된 노드의 덤프를 제공합니다. AEM 인덱스를 수정할 때 발생하는 병합된 색인은 표시되지 않습니다.

Oak 인덱스는 다음을 수행하여 디버깅을 지원합니다.

+ AEM에서 검색 쿼리가 실행되는 방법에 대한 통찰력을 제공하는 모든 Oak 색인 정의를 나열합니다. AEM 인덱스에 수정된 내용은 여기에 반영되지 않습니다. 이 보기는 AEM에서만 제공되거나 사용자 지정 코드에서 단독으로 제공하는 색인에만 유용합니다.

### OSGi 서비스

구성 요소는 모든 OSGi 서비스를 나열합니다. 이 기능은 [AEM SDK의 로컬 빠른 시작인 OSGi Services](http://localhost:4502/system/console/services)(`/system/console/services`에 있음)와 유사합니다.

다음을 통한 디버깅의 OSGi 서비스 도움말:

+ AEM에서 모든 OSGi 서비스, OSGi 번들 및 이를 소비하는 모든 OSGi 번들 나열

### Sling 작업

Sling 작업 에는 모든 Sling 작업 큐가 나열됩니다. 이 기능은 [AEM SDK의 로컬 빠른 시작인 Jobs](http://localhost:4502/system/console/slingevent)(`/system/console/slingevent`에 있음)와 유사합니다.

Sling 작업 은 다음을 수행하여 디버깅을 지원합니다.

+ Sling 작업 큐 및 해당 구성 목록
+ AEM에서 Sling 작업이 수행하는 워크플로우, 임시 워크플로우 및 기타 작업과 관련된 문제를 디버깅하는 데 유용한 활성, 큐 및 처리된 Sling 작업 수에 대한 인사이트를 제공합니다.

## Java 패키지

Java 패키지를 사용하면 AEM에서 Cloud Service으로 사용할 수 있는 Java 패키지 및 버전을 확인할 수 있습니다. 이 기능은 [AEM SDK의 로컬 빠른 시작에서 Dependency Finder](http://localhost:4502/system/console/depfinder)에 있는 것과 동일합니다.`/system/console/depfinder`

![개발자 콘솔 - Java 패키지](./assets/developer-console/java-packages.png)

Java Packages는 확인되지 않은 가져오기 또는 스크립트(HTL, JSP 등)의 확인되지 않은 클래스로 인해 번들이 시작되지 않도록 하는 데 사용됩니다. Java Packages가 Java 패키지를 내보내지 않는 번들을 보고하지 않는 경우(또는 버전이 OSGi 번들로 가져온 버전과 일치하지 않음):

+ 프로젝트의 AEM API Maven 종속성 버전이 환경의 AEM 릴리스 버전과 일치하는지(가능한 경우 모두 최신 버전으로 업데이트) 확인합니다.
+ Maven 프로젝트에서 추가 Maven 종속성이 사용되는 경우
   + AEM SDK API 종속성이 제공하는 대체 API를 대신 사용할 수 있는지 확인합니다.
   + 추가 종속성이 필요한 경우 OSGi 번들(일반 Jar가 아님)로 제공되어야 하고 코어 OSGi 번들이 `ui.apps` 패키지에 포함된 방식과 유사한 방식으로 프로젝트의 코드 패키지(`ui.apps`)에 포함되어 있는지 확인하십시오.

## 서블릿

서블릿은 AEM이 요청을 궁극적으로 처리하는 Java 서블릿 또는 스크립트(HTL, JSP)로 URL을 해결하는 방법에 대한 통찰력을 제공하는 데 사용됩니다. 이 기능은 [AEM SDK의 로컬 Quickstart의 Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver)(`/system/console/servletresolver`에 있음)와 동일합니다.

![개발자 콘솔 - 서블릿](./assets/developer-console/servlets.png)

서블릿은 다음을 결정하는 데 도움이 됩니다.

+ URL이 주소 지정 가능한 부분(리소스, 선택기, 확장)으로 분해되는 방법입니다.
+ URL이 확인되는 서블릿 또는 스크립트를 지정하여 형식이 잘못된 URL 또는 잘못 등록된 서블릿/스크립트를 식별하는 데 도움이 됩니다.

## 쿼리

쿼리는 AEM에서 검색 쿼리가 실행되는 방법 및 방법에 대한 인사이트를 제공하는 데 도움이 됩니다. 이 기능은 [AEM SDK의 로컬 빠른 시작의 도구 > 쿼리 성능 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 콘솔과 동일합니다.

쿼리는 특정 Pod를 선택한 경우에만 작동합니다. 해당 Pod의 Query Performance 웹 콘솔을 열면 개발자가 AEM 서비스에 로그인할 수 있어야 합니다.

![개발자 콘솔 - 쿼리 - 쿼리 설명](./assets/developer-console/queries__explain-query.png)

쿼리는 다음을 통해 디버깅하는 데 도움이 됩니다.

+ Oak에서 쿼리를 해석, 분석 및 실행하는 방법을 설명합니다. 이는 쿼리가 느린 이유를 추적하고 쿼리 속도를 높이는 방법을 이해할 때 매우 중요합니다.
+ AEM에서 실행 중인 가장 인기 있는 쿼리를 나열하고 이를 설명하는 기능을 제공합니다.
+ AEM에서 실행 중인 가장 느린 쿼리를 나열하고 설명을 할 수 있습니다.
