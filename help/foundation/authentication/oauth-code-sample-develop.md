---
title: AEM에서 OAuth 범위 개발
description: Adobe Experience Manager의 확장 가능한 OAuth 범위를 사용하면 최종 사용자가 승인한 클라이언트 애플리케이션에서 리소스에 대한 액세스 제어를 수행할 수 있습니다. 아래 다이어그램은 AEM 컨텍스트에서 요청 흐름을 보여 줍니다.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# OAuth 범위 개발

Adobe Experience Manager의 확장 가능한 OAuth 범위를 사용하면 최종 사용자가 승인한 클라이언트 애플리케이션에서 리소스에 대한 액세스 제어를 수행할 수 있습니다. 아래 다이어그램은 AEM 컨텍스트에서 요청 흐름을 보여 줍니다.

![Oauth 범위 흐름](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM은 세 가지 범위를 제공합니다.

* 프로필
* 오프라인 액세스
* 복제

AEM의 확장 가능한 OAuth 범위를 사용하여 다른 사용자 지정 범위를 정의할 수 있습니다. 예를 들어 OAuth를 통해 승인된 모바일 앱이 에셋을 쓰지 않고 읽기로 제한되도록 하는 사용자 지정 범위를 개발 및 AEM에 배포할 수 있습니다.

OAuth는 AEM 사용자의 자격 증명을 해당 애플리케이션에 제공해야 하는 대신 액세스 토큰을 사용하므로 클라이언트 애플리케이션을 인증하는 기본 방법입니다.

* [코드 보기](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
