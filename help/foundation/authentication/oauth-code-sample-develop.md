---
title: AEM에서 OAuth 범위 개발
description: Adobe Experience Manager의 확장 가능한 OAuth 범위를 사용하면 최종 사용자가 승인한 클라이언트 응용 프로그램에서 리소스에 대한 액세스 제어를 수행할 수 있습니다. 아래 다이어그램은 AEM 컨텍스트에서 요청 흐름을 보여줍니다.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# OAuth 범위 개발

Adobe Experience Manager의 확장 가능한 OAuth 범위를 사용하면 최종 사용자가 승인한 클라이언트 응용 프로그램의 리소스에 대한 액세스 제어를 수행할 수 있습니다. 아래 다이어그램은 AEM 컨텍스트에서 요청 흐름을 보여줍니다.

![Oauth 범위 흐름](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM에서는 세 가지 범위를 제공합니다.

* 프로필
* 오프라인 액세스
* 복제

AEM 확장 가능한 OAuth 범위를 사용하면 다른 사용자 지정 범위를 정의할 수 있습니다. 예를 들어, 사용자 지정 범위를 개발 및 AEM에 배포하여 OAuth를 통해 인증된 모바일 앱을 읽기 전용으로 제한하지만 자산을 쓰지는 않습니다.

OAuth는 AEM 사용자의 자격 증명을 해당 애플리케이션에 제공할 필요 없이 액세스 토큰을 사용하므로 클라이언트 응용 프로그램을 승인하는 기본 방법입니다.

* [코드 보기](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
