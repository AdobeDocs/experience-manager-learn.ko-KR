---
title: AEM에서 OAuth 범위 개발
description: Adobe Experience Manager의 확장 가능한 OAuth 범위를 통해 최종 사용자가 인증한 클라이언트 응용 프로그램의 리소스에 대한 액세스 제어를 가능하게 합니다. 아래 다이어그램은 AEM 컨텍스트의 요청 흐름을 보여줍니다.
version: 6.3, 6.4, 6.5
feature: 사용자 및 그룹
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 3%

---


# OAuth 범위 개발

Adobe Experience Manager의 확장 가능한 OAuth 범위를 통해 최종 사용자가 인증한 클라이언트 응용 프로그램의 리소스에 대한 액세스 제어를 수행할 수 있습니다. 아래 다이어그램은 AEM 컨텍스트의 요청 흐름을 보여줍니다.

![Oauth 범위 흐름](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM은 3가지 범위를 제공합니다.

* 프로필
* 오프라인 액세스
* 복제

AEM 확장 가능한 OAuth 범위를 통해 다른 사용자 정의 범위를 정의할 수 있습니다. 예를 들어 OAuth를 통해 인증한 모바일 앱을 읽기 전용으로 제한할 수 있지만 에셋 쓰기는 할 수 있는 사용자 정의 범위를 AEM에 개발 및 배포할 수 있습니다.

OAuth는 AEM 사용자의 자격 증명을 해당 애플리케이션에 제공할 필요 없이 액세스 토큰을 사용하기 때문에 클라이언트 응용 프로그램을 인증하는 데 권장되는 방법입니다.

* [코드 보기](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
