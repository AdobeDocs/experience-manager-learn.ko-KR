---
title: 개발자 콘솔
description: AEM as a Cloud Service은 디버깅에 도움이 되는 실행 중인 AEM 서비스의 다양한 세부 정보를 노출하는 각 환경에 Developer Console을 제공합니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Developer Console을 사용하여 AEM as a Cloud Service 디버깅

AEM as a Cloud Service은 디버깅에 도움이 되는 실행 중인 AEM 서비스의 다양한 세부 정보를 노출하는 각 환경에 Developer Console을 제공합니다.

각 AEM as a Cloud Service 환경에는 자체 Developer Console이 있습니다.

## Developer Console으로 이동

Developer Console은 Cloud Manager을 통해 AEM as a Cloud Service 환경별로 액세스됩니다.

![Developer Console으로 이동](./assets/developer-console/navigate.png)

1. __[Cloud Manager](https://my.cloudmanager.adobe.com/)__(으)로 이동
2. AEM as a Cloud Service 환경이 포함된 __Program__&#x200B;을 열어 Developer Console을 엽니다.
3. __환경__&#x200B;을(를) 찾은 다음 `...`을(를) 선택합니다.
4. 드롭다운 목록에서 __Developer Console__&#x200B;을(를) 선택합니다.


## Developer Console 액세스

Developer Console에 액세스하여 사용하려면 [Adobe의 Admin Console](https://adminconsole.adobe.com)을(를) 통해 개발자의 Adobe ID에 다음 권한을 부여해야 합니다.

1. Adobe 조직 전환기에서 Developer Console에서 검사할 환경과 관련된 Adobe 조직을 볼 수 있는지 확인합니다.
1. Developer Console에 로그인하려면 개발자가 다음 역할 중 하나의 멤버여야 합니다.
   + [Cloud Manager 제품의 __개발자 - Cloud Service__ 제품 프로필](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer): 이 경우 개발자는 선택한 Developer Console URL에서 사용할 수 있는 환경의 전체 목록을 보게 됩니다. Cloud Manager에서 개발 환경 또는 RDE를 선택한 경우 동일한 프로그램의 다른 개발 환경 또는 RDE가 나타날 수 있습니다.
   + __AEM 작성자__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)의 [__AEM 관리자__ 제품 프로필: 이 경우 이전 글머리 기호에 설명된 환경 목록은 이 역할이 할당된 관련 제품 프로필로 제한됩니다.
1. 개발자는 [__AEM 사용자__ 또는 __AEM 관리자__ AEM 작성자 및/또는 게시의 제품 프로필](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)의 멤버여야 합니다.
   + 이 멤버십이 없으면 [상태](#status) 덤프가 시간 초과되고 승인되지 않은 오류 401이 발생합니다.

### Developer Console 액세스 문제 해결

#### 로그인할 때 찾고 있는 환경이 표시되지 않습니다.

다음을 확인하십시오.

+ Cloud Manager을 통해 선택한 환경에 대한 세 점을 클릭하여 올바른 Developer Console URL을 선택하고 Developer Console을 선택합니다.
+ [Cloud Manager 제품의 __개발자 - Cloud Service__ 제품 프로필](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer)을 통해 전체 환경 목록을 확인하거나, 찾을 수 없는 환경에 대해 __AEM 작성자__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)의 [__AEM 관리자__ 제품 프로필에 속해 있습니다.

#### 401 상태를 덤프할 때 승인되지 않은 오류 발생

![Developer Console - 401 권한 없음](./assets/developer-console/troubleshooting__401-unauthorized.png)

상태를 덤프하는 경우 401 Unauthorized 오류가 보고되면 사용자가 아직 AEM as a Cloud Service에 필요한 권한을 가지고 있지 않거나 로그인 토큰 사용이 잘못되었거나 만료되었음을 의미합니다.

승인되지 않은 401 문제를 해결하려면 다음을 수행하십시오.

1. 사용자가 Developer Console의 연결된 AEM as a Cloud Service 제품 인스턴스에 적절한 Adobe IMS 제품 프로필(AEM 관리자 또는 AEM 사용자)의 멤버인지 확인합니다.
   + Developer Console은 2개의 Adobe IMS 제품 인스턴스, 즉 AEM as a Cloud Service 작성자 및 게시 제품 인스턴스에 액세스하므로 Developer Console을 통한 액세스가 필요한 서비스 계층에 따라 올바른 제품 프로필이 사용되도록 해야 합니다.
1. AEM as a Cloud Service(작성자 또는 게시)에 로그인하고 사용자 및 그룹이 AEM에 제대로 동기화되었는지 확인합니다.
   + Developer ConsoleAEM 을 사용하려면 해당 서비스 계층에 사용자 레코드를 인증해야 합니다.
1. 브라우저 쿠키와 애플리케이션 상태(로컬 저장소)를 지우고 Developer Console에 다시 로그인하여 Developer Console이 사용 중인 액세스 토큰이 올바르고 만료되지 않았는지 확인합니다.

## Pod

AEM as a Cloud Service Author 및 Publish 서비스는 다운타임 없이 트래픽 변동성 및 롤링 업데이트를 처리하기 위해 각각 여러 인스턴스로 구성됩니다. 이러한 인스턴스를 Pod라고 합니다. Developer Console의 Pod selection은 다른 컨트롤을 통해 노출되는 데이터 범위를 정의합니다.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ pod는 AEM 서비스(작성자 또는 게시)의 일부인 개별 인스턴스입니다
+ 포드는 일시적인 것으로, 이는 AEM as a Cloud Service이 필요에 따라 포드를 만들고 파괴함을 의미합니다
+ 연결된 AEM as a Cloud Service 환경에 포함된 pod에만 해당 환경의 Developer Console Pod switcher가 나열됩니다.
+ Pod switcher 하단에 있는 편의 옵션을 사용하여 서비스 유형별로 Pod을 선택할 수 있습니다.
   + 모든 작성자
   + 모든 게시자
   + 모든 인스턴스

## 상태

상태는 특정 AEM 런타임 상태를 텍스트 또는 JSON 출력으로 출력하는 옵션을 제공합니다. Developer Console은 AEM SDK의 로컬 빠른 시작의 OSGi 웹 콘솔과 유사한 정보를 제공하며, Developer Console은 읽기 전용입니다.

![Developer Console - 상태](./assets/developer-console/status.png)

### 번들

번들은 AEM의 모든 OSGi 번들을 나열합니다. 이 기능은 `/system/console/bundles`의 [AEM SDK 로컬 빠른 시작의 OSGi 번들](http://localhost:4502/system/console/bundles)과(와) 유사합니다.

디버깅을 위한 번들 도움말:

+ AEM as a Service에 배포된 모든 OSGi 번들 나열
+ 각 OSGi 번들의 상태 나열(활성 여부 포함)
+ OSGi 번들이 활성화되지 않도록 하는 해결되지 않은 종속성에 세부 정보 제공

### 구성 요소

구성 요소 는 AEM의 모든 OSGi 구성 요소를 나열합니다. 이 기능은 `/system/console/components`의 [AEM SDK의 로컬 빠른 시작의 OSGi 구성 요소](http://localhost:4502/system/console/components)와(과) 유사합니다.

구성 요소는 다음을 통해 디버깅에 도움이 됩니다.

+ AEM as a Cloud Service에 배포된 모든 OSGi 구성 요소 나열
+ 각 OSGi 구성 요소의 상태 제공(활성 또는 미충족 여부 포함)
+ 충족되지 않은 서비스 참조에 세부 정보를 제공하면 OSGi 구성 요소가 활성화될 수 있습니다
+ OSGi 구성 요소에 바인딩된 OSGi 속성 및 해당 값 나열.
   + [OSGi 환경 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)를 통해 삽입된 실제 값을 표시합니다.

### 구성

구성은 모든 OSGi 구성 요소의 구성(OSGi 속성 및 값)을 나열합니다. 이 기능은 `/system/console/configMgr`의 [AEM SDK의 로컬 빠른 시작의 OSGi 구성 관리자](http://localhost:4502/system/console/configMgr)와 유사합니다.

디버깅에 도움이 되는 구성:

+ OSGi 구성 요소별 OSGi 속성 및 해당 값 나열
   + [OSGi 환경 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)를 통해 삽입된 실제 값은 표시되지 않습니다. 삽입된 값은 위의 [구성 요소](#components)를 참조하십시오.
+ 잘못 구성된 속성 찾기 및 확인

### Oak 인덱스

Oak 인덱스는 `/oak:index` 아래에 정의된 노드 덤프를 제공합니다. AEM 색인을 수정할 때 발생하는 병합된 색인은 표시되지 않습니다.

Oak 색인은 다음을 통해 디버깅에 도움이 됩니다.

+ AEM에서 검색 쿼리가 실행되는 방식에 대한 통찰력을 제공하는 모든 Oak 색인 정의를 나열합니다. AEM 색인으로 수정된 사항은 여기에 반영되지 않습니다. 이 보기는 AEM에서 단독으로 제공하거나 사용자 지정 코드에서 단독으로 제공하는 인덱스에만 유용합니다.

### OSGi 서비스

구성 요소에 모든 OSGi 서비스가 나열됩니다. 이 기능은 `/system/console/services`의 [AEM SDK의 로컬 빠른 시작의 OSGi 서비스](http://localhost:4502/system/console/services)와(과) 유사합니다.

OSGi Services는 다음을 통해 디버깅에 도움이 됩니다.

+ 제공 OSGi 번들 및 이를 사용하는 모든 OSGi 번들과 함께 AEM의 모든 OSGi 서비스 나열

### Sling 작업

슬링 작업은 모든 슬링 작업 대기열을 나열합니다. 이 기능은 `/system/console/slingevent`의 [AEM SDK 로컬 빠른 시작의 작업](http://localhost:4502/system/console/slingevent)과(와) 유사합니다.

Sling 작업 도움말 디버깅 방법:

+ Sling 작업 큐 및 구성 목록
+ 활성, 큐에 있음 및 처리된 Sling 작업의 수에 대한 통찰력을 제공합니다. 이는 AEM의 Sling 작업에서 수행되는 워크플로우, 임시 워크플로우 및 기타 작업과 관련된 문제를 디버깅하는 데 유용합니다.

## Java 패키지

Java 패키지를 사용하면 AEM as a Cloud Service에서 Java 패키지 및 버전을 사용할 수 있는지 확인할 수 있습니다. 이 기능은 `/system/console/depfinder`의 [AEM SDK의 로컬 빠른 시작의 종속성 파인더](http://localhost:4502/system/console/depfinder)와(과) 동일합니다.

![Developer Console - Java 패키지](./assets/developer-console/java-packages.png)

Java 패키지는 해결되지 않은 가져오기 또는 스크립트(HTL, JSP 등)의 해결되지 않은 클래스로 인해 번들이 시작되지 않는 문제를 해결하는 데 사용됩니다. Java Packages에서 어떤 번들도 Java 패키지를 내보내지 않는 경우(또는 버전이 OSGi 번들에 의해 가져온 버전과 일치하지 않는 경우):

+ 프로젝트의 AEM API Maven 종속성 버전이 환경의 AEM 릴리스 버전과 일치하는지 확인합니다(가능한 경우 모든 항목을 최신 버전으로 업데이트).
+ Maven 프로젝트에 추가 Maven 종속성이 사용되는 경우
   + AEM SDK API 종속성에서 제공하는 대체 API를 대신 사용할 수 있는지 확인합니다.
   + 추가 종속성이 필요한 경우 일반 Jar가 아닌 OSGi 번들로 제공되는지, 그리고 핵심 OSGi 번들이 `ui.apps` 패키지에 포함된 방법과 유사하게 프로젝트의 코드 패키지(`ui.apps`)에 포함되어 있는지 확인하십시오.

## 서블릿

서블릿은 AEM이 궁극적으로 요청을 처리하는 Java 서블릿 또는 스크립트(HTL, JSP)에 대한 URL을 확인하는 방법에 대한 통찰력을 제공하는 데 사용됩니다. 이 기능은 `/system/console/servletresolver`의 [AEM SDK의 로컬 빠른 시작의 Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver)과(와) 동일합니다.

![Developer Console - 서블릿](./assets/developer-console/servlets.png)

서블릿은 디버깅 결정에 도움이 됩니다.

+ URL이 대응 가능한 부분(리소스, 선택기, 확장)으로 분해되는 방법입니다.
+ URL이 확인하는 서블릿 또는 스크립트는 잘못된 형식의 URL이나 잘못 등록된 서블릿/스크립트를 식별하는 데 도움이 됩니다.

## 쿼리

쿼리는 AEM에서 검색 쿼리가 실행되는 내용과 방법에 대한 통찰력을 제공하는 데 도움이 됩니다. 이 기능은 [AEM SDK의 로컬 빠른 시작의 도구 > 쿼리 성능](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 콘솔과 동일합니다.

쿼리는 특정 pod를 선택한 경우에만 작동합니다. 해당 pod의 쿼리 성능 웹 콘솔이 열리면 개발자가 AEM 서비스에 로그인할 수 있는 액세스 권한을 보유해야 합니다.

![Developer Console - 쿼리 - 쿼리 설명](./assets/developer-console/queries__explain-query.png)

쿼리는 다음을 통해 디버깅에 도움이 됩니다.

+ Oak에서 쿼리를 해석, 분석 및 실행하는 방법에 대해 설명합니다. 이는 쿼리가 느린 이유를 추적하고 쿼리의 속도를 높이는 방법을 이해할 때 매우 중요합니다.
+ 설명 기능과 함께 AEM에서 실행되는 가장 인기 있는 쿼리를 나열합니다.
+ 설명 기능과 함께 AEM에서 실행 중인 가장 느린 쿼리를 나열합니다.
