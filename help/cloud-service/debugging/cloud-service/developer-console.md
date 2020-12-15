---
title: 개발자 콘솔
description: AEM as a Cloud Service은 디버깅에 유용한 실행 중인 AEM 서비스의 다양한 세부 사항을 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# 개발자 콘솔을 사용하여 AEM을 Cloud Service으로 디버깅

AEM as a Cloud Service은 디버깅에 유용한 실행 중인 AEM 서비스의 다양한 세부 사항을 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.

Cloud Service 환경으로서 각 AEM에는 자체 개발자 콘솔이 있습니다.

## 개발자 콘솔 액세스

개발자 콘솔에 액세스하고 사용하려면 [Adobe Admin Console](https://adminconsole.adobe.com)을 통해 개발자의 Adobe ID에 다음 권한을 부여해야 합니다.

1. Adobe 조직 전환기에서 Cloud Service 제품으로 Cloud Manager 및 AEM을 영향을 준 Adobe 조직이 활성화되어 있는지 확인합니다.
1. 개발자는 클라우드 관리자 제품의 __개발자 - Cloud Service__ 제품 프로필의 멤버여야 합니다.
   + 이 멤버십이 없는 경우 개발자는 개발자 콘솔에 로그인할 수 없습니다.
1. 개발자는 AEM 작성자 및 게시 서비스의 __AEM 관리자__ 제품 프로필의 구성원이어야 합니다.
   + 이 멤버십이 없는 경우 [status](#status) 덤프가 401 권한 없음 오류로 인해 시간 초과됩니다.

### 개발자 콘솔 액세스 문제 해결

#### 401 상태를 덤핑하는 동안 권한 없는 오류 발생

![개발자 콘솔 - 401 권한 없음](./assets/developer-console/troubleshooting__401-unauthorized.png)

상태를 덤핑하는 경우 401 권한 없음 오류가 보고되면 사용자가 Cloud Service으로 AEM에 필요한 권한이 아직 없거나 로그인 토큰 사용이 잘못되었거나 만료되었음을 의미합니다.

401 권한 없는 문제를 해결하려면 다음을 수행하십시오.

1. 사용자가 Cloud Service 제품 인스턴스로 개발자 콘솔과 연결된 AEM에 대한 적절한 Adobe IMS 제품 프로필(AEM 관리자 또는 AEM 사용자)의 멤버인지 확인합니다.
   + Developer Console은 2개의 Adobe IMS 제품 인스턴스에 액세스합니다.aem은 Cloud Service 작성자 및 게시 제품 인스턴스로 표시되므로 개발자 콘솔을 통해 액세스해야 하는 서비스 계층에 따라 올바른 제품 프로필이 사용되는지 확인하십시오.
1. AEM에 Cloud Service(작성자 또는 게시)로 로그인하고 사용자 및 그룹이 AEM에 제대로 동기화되었는지 확인합니다.
   + 개발자 콘솔에서는 해당 서비스 계층에 인증하기 위해 해당 AEM 서비스 계층에서 사용자 레코드를 만들어야 합니다.
1. 브라우저 쿠키와 애플리케이션 상태(로컬 저장소)를 지우고 개발자 콘솔에 다시 로그인하여 액세스 토큰의 개발자 콘솔이 올바로 사용되고 있고 만료되지 않도록 합니다.

## 창

CLOUD SERVICE 작성자 및 게시 서비스인 AEM은 다운타임 없이 트래픽 가변성과 롤링 업데이트를 처리하기 위해 각각 여러 개의 인스턴스로 구성됩니다. 이러한 인스턴스를 [창]이라고 합니다. 개발자 콘솔의 창 선택 항목은 다른 컨트롤을 통해 노출되는 데이터의 범위를 정의합니다.

![개발자 콘솔 - 창](./assets/developer-console/pod.png)

+ 창은 AEM 서비스(작성자 또는 게시)의 일부인 개별 인스턴스입니다.
+ 포드(Pods)는 일시적이며, Cloud Service이 필요에 따라 창을 만들고 손상시키는 AEM을 의미합니다
+ Cloud Service 환경으로 연관된 AEM의 일부인 포드만 해당 환경의 개발자 콘솔의 창 전환기로 나열됩니다.
+ [창 전환기] 아래쪽에 있는 편리한 옵션을 사용하여 서비스 유형별로 창을 선택할 수 있습니다.
   + 모든 작성자
   + 모든 게시자
   + 모든 인스턴스

## 상태

상태는 텍스트 또는 JSON 출력에서 특정 AEM 런타임 상태를 출력하기 위한 옵션을 제공합니다. 개발자 콘솔은 AEM SDK의 로컬 quickstart의 OSGi 웹 콘솔과 유사한 정보를 제공하지만 개발자 콘솔은 읽기 전용이라는 뚜렷한 차이점이 있습니다.

![개발자 콘솔 - 상태](./assets/developer-console/status.png)

### 번들

번들에는 AEM의 모든 OSGi 번들이 나열됩니다. 이 기능은 ](http://localhost:4502/system/console/bundles)AEM SDK의 로컬 quickstart의 OSGi 번들[과 유사합니다.`/system/console/bundles`

디버깅에 도움이 되는 번들:

+ 서비스로 AEM에 배포된 모든 OSGi 번들 목록
+ 각 OSGi 번들 상태 나열;활성 상태인지 여부를 포함하는 경우
+ OSGi 번들이 활성화되지 않도록 하는 해결되지 않은 종속성 세부 정보 제공

### 구성 요소

구성 요소는 AEM의 모든 OSGi 구성 요소를 나열합니다. 이 기능은 ](http://localhost:4502/system/console/components)AEM SDK의 로컬 quickstart의 OSGi 구성 요소[와 유사합니다.`/system/console/components`

구성 요소는 다음을 통해 디버깅에 도움이 됩니다.

+ Cloud Service으로 AEM에 배포된 모든 OSGi 구성 요소 목록
+ 각 OSGi 구성 요소의 상태 제공;활성 상태이거나 만족스럽지 않은 경우 포함
+ 충족되지 않은 서비스 참조에 세부 사항을 제공하면 OSGi 구성 요소가 활성 상태가 될 수 있습니다.
+ OSGi 속성 및 해당 값이 OSGi 구성 요소에 바인딩되어 있는 목록

### 구성

구성은 모든 OSGi 구성 요소의 구성(OSGi 속성 및 값)을 나열합니다. 이 기능은 `/system/console/configMgr`의 [AEM SDK의 로컬 quickstart의 OSGi 구성 관리자](http://localhost:4502/system/console/configMgr)와 유사합니다.

디버깅에 도움이 되는 구성:

+ OSGi 속성 및 해당 값을 OSGi 구성 요소로 나열
+ 잘못 구성된 속성 찾기 및 식별

### Oak 색인

Oak 인덱스는 `/oak:index` 아래에 정의된 노드의 덤프를 제공합니다. AEM 색인을 수정할 때 발생하는 병합된 색인은 표시되지 않습니다.

Oak 인덱스는 디버깅에 다음과 같은 방법을 사용합니다.

+ AEM에서 검색 쿼리가 실행되는 방식에 대한 통찰력을 제공하는 모든 Oak 색인 정의를 나열합니다. AEM 색인에 수정된 내용이 여기에 반영되지 않는다는 점을 명심하십시오. 이 보기는 AEM에서만 제공되거나 사용자 지정 코드로 제공된 색인에만 유용합니다.

### OSGi 서비스

구성 요소에는 모든 OSGi 서비스가 나열됩니다. 이 기능은 ](http://localhost:4502/system/console/services)AEM SDK의 로컬 quickstart의 OSGi 서비스[와 유사합니다.`/system/console/services`

OSGi Services 도움말은 다음을 통해 디버깅할 수 있습니다.

+ AEM의 모든 OSGi 서비스와 OSGi 번들 및 이 번들 제품을 사용하는 모든 OSGi 번들 목록

### Sling 작업

Sling 작업은 모든 Sling 작업 대기열을 나열합니다. 이 기능은 ](http://localhost:4502/system/console/slingevent)AEM SDK의 로컬 quickstart 작업[과 유사합니다.`/system/console/slingevent`

Sling Jobs는 디버깅에 다음과 같은 방법을 제공합니다.

+ Sling 작업 큐 및 구성 목록
+ AEM의 Sling Jobs에서 워크플로우, 임시 워크플로우 및 기타 작업을 수행하는 문제를 디버깅하는 데 유용한 활성, 큐에 대기 및 처리된 Sling 작업 수에 대한 통찰력을 제공합니다.

## Java 패키지

Java 패키지를 사용하면 AEM에서 Cloud Service으로 사용할 수 있는 Java 패키지 및 버전을 확인할 수 있습니다. 이 기능은 `/system/console/depfinder`의 [AEM SDK의 로컬 quickstart의 종속성 파인더](http://localhost:4502/system/console/depfinder)과 동일합니다.

![개발자 콘솔 - Java 패키지](./assets/developer-console/java-packages.png)

Java Packages는 확인되지 않은 가져오기 문제 또는 스크립트(HTL, JSP 등)의 해결되지 않은 클래스 때문에 번들이 시작되지 않는 데 문제가 있습니다. Java Packages가 Java 패키지를 내보내지 않는 번들을 보고하는 경우(또는 버전이 OSGi 번들에 의해 가져온 번들 버전과 일치하지 않음):

+ 프로젝트의 AEM API 개발자 종속성 버전이 환경의 AEM 릴리스 버전과 일치하는지 확인하고 가능한 경우 모든 버전을 최신 버전으로 업데이트합니다.
+ Maven 프로젝트에서 추가 Mawen 종속성을 사용하는 경우
   + AEM SDK API 종속성이 제공하는 대체 API를 대신 사용할 수 있는지 확인합니다.
   + 추가 종속성이 필요한 경우, 기본 OSGi 번들이 아닌 OSGi 번들(JAR 대신)로 제공되어야 하며 핵심 OSGi 번들이 `ui.apps` 패키지에 포함된 방식과 유사한 방식으로 프로젝트의 코드 패키지(`ui.apps`)에 포함되어 있는지 확인합니다.

## Servlets

서블릿은 AEM이 요청을 궁극적으로 처리하는 Java 서블릿 또는 스크립트(HTL, JSP)로 URL을 해결하는 방법에 대한 통찰력을 제공하는 데 사용됩니다. 이 기능은 ](http://localhost:4502/system/console/servletresolver)에서 AEM SDK의 로컬 Quickstart의 Sling Servlet Resolver[과 동일합니다.`/system/console/servletresolver`

![개발자 콘솔 - 서비스](./assets/developer-console/servlets.png)

Servlets를 사용하여 다음을 확인할 수 있습니다.

+ URL이 주소 지정 가능한 부분(리소스, 선택기, 확장)으로 분해되는 방법.
+ URL이 인식하는 서블릿 또는 스크립트로서, 체계화된 URL 또는 잘못 등록된 서블릿/스크립트를 식별할 수 있습니다.

## 쿼리

쿼리는 AEM에서 검색 쿼리가 실행되는 방법과 방식에 대한 통찰력을 제공합니다. 이 기능은 [AEM SDK의 로컬 quickstart의 도구 > 쿼리 성능 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 콘솔과 동일합니다.

쿼리는 특정 창이 선택된 상태에서 해당 창의 [쿼리 성능] 웹 콘솔을 열어 개발자가 AEM 서비스에 로그인할 수 있도록 해야 하는 경우에만 작동합니다.

![개발자 콘솔 - 쿼리 - 쿼리 설명](./assets/developer-console/queries__explain-query.png)

쿼리는 다음을 통해 디버깅하는 데 도움이 됩니다.

+ Oak에서 쿼리가 해석되고 분석되고 실행되는 방식을 설명합니다. 쿼리가 느린 이유를 추적하고 속도를 높일 수 있는 방법을 이해하는 경우 이 작업은 매우 중요합니다.
+ AEM에서 실행 중인 가장 인기 있는 쿼리를 나열하고 이를 설명합니다.
+ AEM에서 실행 중인 가장 느린 쿼리를 나열하고 이를 설명합니다.
