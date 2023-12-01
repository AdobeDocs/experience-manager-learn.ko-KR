---
title: 개발자 콘솔
description: AEM as a Cloud Service에서는 디버깅에 도움이 되는 실행 중인 AEM 서비스의 다양한 세부 정보를 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---

# Developer Console을 사용하여 AEM as a Cloud Service 디버깅

AEM as a Cloud Service에서는 디버깅에 도움이 되는 실행 중인 AEM 서비스의 다양한 세부 정보를 표시하는 각 환경에 대한 개발자 콘솔을 제공합니다.

각 AEM as a Cloud Service 환경에는 자체 개발자 콘솔이 있습니다.

## 개발자 콘솔로 이동

AEM 개발자 콘솔은 Cloud Manager를 통해 as a Cloud Service 환경별로 액세스됩니다.

![개발자 콘솔로 이동](./assets/developer-console/navigate.png)

1. 다음으로 이동 __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. 를 엽니다. __프로그램__ 에는 Developer Console을 열 수 있는 AEM as a Cloud Service 환경이 포함되어 있습니다.
3. 를 찾습니다. __환경__&#x200B;을(를) 클릭하고 `...`.
4. 선택 __개발자 콘솔__ 드롭다운 목록에서 클릭합니다.


## 개발자 콘솔 액세스

Developer Console에 액세스하고 사용하려면 를 통해 개발자의 Adobe ID에 다음 권한을 부여해야 합니다. [Adobe Admin Console](https://adminconsole.adobe.com).

1. Cloud Manager 및 AEM as a Cloud Service 제품에 영향을 준 Adobe 조직이 Adobe 조직 전환기에서 활성화되었는지 확인합니다.
1. 개발자는 의 멤버여야 합니다. [Cloud Manager 제품 __개발자 - Cloud Service__ 제품 프로필](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer).
   + 이 멤버십이 없으면 개발자가 Developer Console에 로그인할 수 없습니다.
1. 개발자는 의 멤버여야 합니다. [__AEM 사용자__ 또는 __AEM 관리자__ AEM 작성자 및/또는 게시의 제품 프로필](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + 이 멤버십이 없으면 [상태](#status) 401 Unauthorized 오류로 인해 덤프가 시간 초과됩니다.

### 개발자 콘솔 액세스 문제 해결

#### 401 상태를 덤프할 때 승인되지 않은 오류 발생

![Developer Console - 401 권한 없음](./assets/developer-console/troubleshooting__401-unauthorized.png)

AEM 상태를 덤프하는 경우 401 Unauthorized 오류가 보고되면 사용자가 아직 as a Cloud Service에 필요한 권한을 가지고 있지 않거나 로그인 토큰 사용이 잘못되었거나 만료되었음을 의미합니다.

승인되지 않은 401 문제를 해결하려면 다음을 수행하십시오.

1. 사용자가 Developer Console의 관련 AEM as a Cloud Service 제품 인스턴스에 적절한 Adobe IMS 제품 프로필(AEM 관리자 또는 AEM 사용자)의 멤버인지 확인합니다.
   + AEM Developer Console은 2개의 Adobe IMS 제품 인스턴스, 즉 as a Cloud Service 작성자 및 게시 제품 인스턴스에 액세스하므로 Developer Console을 통한 액세스가 필요한 서비스 계층에 따라 올바른 제품 프로필이 사용되는지 확인하십시오.
1. AEM as a Cloud Service(작성자 또는 게시)에 로그인하고 사용자 및 그룹이 AEM에 제대로 동기화되었는지 확인합니다.
   + Developer Console을 사용하려면 해당 AEM 서비스 계층에 사용자 레코드를 만들어야 해당 서비스 계층을 인증할 수 있습니다.
1. 브라우저 쿠키와 애플리케이션 상태(로컬 저장소)를 지우고 Developer Console에 다시 로그인하여 Developer Console이 사용 중인 액세스 토큰이 올바르고 만료되지 않았는지 확인합니다.

## Pod

AEM as a Cloud Service Author 및 Publish 서비스는 다운타임 없이 트래픽 가변성과 롤링 업데이트를 처리할 수 있도록 각각 여러 인스턴스로 구성됩니다. 이러한 인스턴스를 Pod라고 합니다. Developer Console의 Pod 선택 항목은 다른 컨트롤을 통해 노출되는 데이터 범위를 정의합니다.

![개발자 콘솔 - Pod](./assets/developer-console/pod.png)

+ pod는 AEM 서비스(작성자 또는 게시)의 일부인 개별 인스턴스입니다
+ Pod는 일시적입니다. 즉, AEM은 필요에 따라 as a Cloud Service으로 만들고 소멸시킵니다
+ 연결된 AEM as a Cloud Service 환경에 속하는 Pod만 해당 환경의 Developer Console Pod Switcher에 나열됩니다.
+ Pod switcher 하단에 있는 편의 옵션을 사용하여 서비스 유형별로 Pod을 선택할 수 있습니다.
   + 모든 작성자
   + 모든 게시자
   + 모든 인스턴스

## 상태

상태는 특정 AEM 런타임 상태를 텍스트 또는 JSON 출력으로 출력하는 옵션을 제공합니다. Developer Console은 AEM SDK의 로컬 빠른 시작의 OSGi 웹 콘솔과 유사한 정보를 제공하며 Developer Console은 읽기 전용입니다.

![개발자 콘솔 - 상태](./assets/developer-console/status.png)

### 번들

번들은 AEM의 모든 OSGi 번들을 나열합니다. 이 기능은 와 유사합니다 [AEM SDK의 로컬 빠른 시작의 OSGi 번들](http://localhost:4502/system/console/bundles) 위치: `/system/console/bundles`.

디버깅을 위한 번들 도움말:

+ AEM as a Service에 배포된 모든 OSGi 번들 나열
+ 각 OSGi 번들의 상태 나열(활성 여부 포함)
+ OSGi 번들이 활성화되지 않도록 하는 해결되지 않은 종속성에 세부 정보 제공

### 구성 요소

구성 요소 는 AEM의 모든 OSGi 구성 요소를 나열합니다. 이 기능은 와 유사합니다 [AEM SDK의 로컬 빠른 시작의 OSGi 구성 요소](http://localhost:4502/system/console/components) 위치: `/system/console/components`.

구성 요소는 다음을 통해 디버깅에 도움이 됩니다.

+ AEM as a Cloud Service으로 배포된 모든 OSGi 구성 요소 나열
+ 각 OSGi 구성 요소의 상태 제공(활성 또는 미충족 여부 포함)
+ 충족되지 않은 서비스 참조에 세부 정보를 제공하면 OSGi 구성 요소가 활성화될 수 있습니다
+ OSGi 구성 요소에 바인딩된 OSGi 속성 및 해당 값 나열.
   + 다음을 통해 삽입된 실제 값을 표시합니다. [OSGi 환경 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### 구성

구성은 모든 OSGi 구성 요소의 구성(OSGi 속성 및 값)을 나열합니다. 이 기능은 와 유사합니다 [AEM SDK의 로컬 빠른 시작의 OSGi 구성 관리자](http://localhost:4502/system/console/configMgr) 위치: `/system/console/configMgr`.

디버깅에 도움이 되는 구성:

+ OSGi 구성 요소별 OSGi 속성 및 해당 값 나열
   + 를 통해 삽입된 실제 값은 표시되지 않습니다. [OSGi 환경 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). 다음을 참조하십시오 [구성 요소](#components) 위의 (삽입된 값)
+ 잘못 구성된 속성 찾기 및 확인

### Oak 색인

Oak 인덱스는 아래에 정의된 노드 덤프를 제공합니다. `/oak:index`. AEM 색인을 수정할 때 발생하는 병합된 색인은 표시되지 않습니다.

Oak 색인은 다음을 통해 디버깅에 도움이 됩니다.

+ AEM에서 검색 쿼리가 실행되는 방식에 대한 통찰력을 제공하는 모든 Oak 색인 정의를 나열합니다. AEM 색인으로 수정된 사항은 여기에 반영되지 않습니다. 이 보기는 AEM에서 단독으로 제공하거나 사용자 지정 코드에서 단독으로 제공하는 인덱스에만 유용합니다.

### OSGi 서비스

구성 요소에 모든 OSGi 서비스가 나열됩니다. 이 기능은 와 유사합니다 [AEM SDK의 로컬 빠른 시작의 OSGi 서비스](http://localhost:4502/system/console/services) 위치: `/system/console/services`.

OSGi Services는 다음을 통해 디버깅에 도움이 됩니다.

+ 제공되는 OSGi 번들 및 이를 사용하는 모든 OSGi 번들과 함께 AEM의 모든 OSGi 서비스 나열

### Sling 작업

슬링 작업은 모든 슬링 작업 대기열을 나열합니다. 이 기능은 와 유사합니다 [AEM SDK의 로컬 빠른 시작의 작업](http://localhost:4502/system/console/slingevent) 위치: `/system/console/slingevent`.

Sling 작업 도움말 디버깅 방법:

+ Sling 작업 큐 및 구성 목록
+ 대기 중인 활성 Sling 작업 및 처리된 작업 수에 대한 통찰력을 제공하여 AEM의 Sling 작업에서 수행되는 워크플로, 임시 워크플로 및 기타 작업과 관련된 문제를 디버깅하는 데 유용합니다.

## Java 패키지

Java 패키지를 사용하면 Java 패키지 및 버전을 AEM에서 as a Cloud Service으로 사용할 수 있는지 확인할 수 있습니다. 이 기능은 와 동일합니다. [AEM SDK의 로컬 빠른 시작의 종속성 파인더](http://localhost:4502/system/console/depfinder) 위치: `/system/console/depfinder`.

![Developer Console - Java 패키지](./assets/developer-console/java-packages.png)

Java 패키지는 해결되지 않은 가져오기 또는 스크립트(HTL, JSP 등)의 해결되지 않은 클래스로 인해 번들이 시작되지 않는 문제를 해결하는 데 사용됩니다. Java Packages에서 어떤 번들도 Java 패키지를 내보내지 않는 경우(또는 버전이 OSGi 번들에 의해 가져온 버전과 일치하지 않는 경우):

+ 프로젝트의 AEM API Maven 종속 항목 버전이 환경의 AEM 릴리스 버전과 일치하는지 확인합니다(가능한 경우 모든 항목을 최신 버전으로 업데이트).
+ Maven 프로젝트에 추가 Maven 종속성이 사용되는 경우
   + AEM SDK API 종속성에서 제공하는 대체 API를 대신 사용할 수 있는지 여부를 결정합니다.
   + 추가 종속성이 필요한 경우 가 일반 Jar이 아닌 OSGi 번들로 제공되고 프로젝트의 코드 패키지(`ui.apps`)를 추가하여 핵심 OSGi 번들이 `ui.apps` 패키지.

## 서블릿

서블릿은 AEM이 궁극적으로 요청을 처리하는 Java 서블릿 또는 스크립트(HTL, JSP)에 대한 URL을 확인하는 방법에 대한 통찰력을 제공하는 데 사용됩니다. 이 기능은 와 동일합니다. [AEM SDK의 로컬 빠른 시작의 Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) 위치: `/system/console/servletresolver`.

![Developer Console - 서블릿](./assets/developer-console/servlets.png)

서블릿은 디버깅 결정에 도움이 됩니다.

+ URL이 대응 가능한 부분(리소스, 선택기, 확장)으로 분해되는 방법입니다.
+ URL이 확인하는 서블릿 또는 스크립트는 잘못된 형식의 URL이나 잘못 등록된 서블릿/스크립트를 식별하는 데 도움이 됩니다.

## 쿼리

쿼리는 AEM에서 검색 쿼리가 실행되는 내용과 그 방법에 대한 통찰력을 제공하는 데 도움이 됩니다. 이 기능은 와 동일합니다.  [AEM SDK의 로컬 빠른 시작의 도구 > 쿼리 성능](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 콘솔.

쿼리는 특정 pod를 선택한 경우에만 작동합니다. 해당 pod의 쿼리 성능 웹 콘솔이 열리면 개발자가 AEM 서비스에 로그인할 수 있는 액세스 권한을 보유해야 합니다.

![개발자 콘솔 - 쿼리 - 쿼리 설명](./assets/developer-console/queries__explain-query.png)

쿼리는 다음을 통해 디버깅에 도움이 됩니다.

+ Oak에서 쿼리를 해석, 분석 및 실행하는 방법에 대해 설명합니다. 이는 쿼리가 느린 이유를 추적하고 쿼리의 속도를 높이는 방법을 이해할 때 매우 중요합니다.
+ 설명 기능과 함께 AEM에서 실행되는 가장 인기 있는 쿼리를 나열합니다.
+ 설명 기능과 함께 AEM에서 실행 중인 가장 느린 쿼리를 나열합니다.
