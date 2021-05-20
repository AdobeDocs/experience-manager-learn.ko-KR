---
title: AEM Headless 자습서
description: Adobe Experience Manager을 헤드리스 CMS로 사용하는 방법에 대한 자습서 모음입니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# AEM Headless 자습서

Adobe Experience Manager에는 헤드리스 엔드포인트를 정의하고 콘텐츠를 JSON으로 전달하는 여러 가지 옵션이 있습니다. 실습 위주의 자습서를 통해 다양한 옵션을 사용하는 방법을 탐색하고 사용자에게 적합한 옵션을 선택할 수 있습니다.

## AEM GraphQL API 자습서

컨텐츠 조각용 AEM GraphQL API
에서는 외부 클라이언트 애플리케이션이 AEM에서 관리되는 콘텐츠를 사용하여 경험을 렌더링하는 헤드리스 CMS 시나리오를 지원합니다.

최신 콘텐츠 전달 API는 Javascript 기반 프런트 엔드 애플리케이션의 효율성 및 성능을 위한 키입니다. REST API 를 사용하면 다음과 같은 문제가 발생합니다.

* 한 번에 하나의 개체를 가져오기 위한 요청 수가 많습니다.
* 종종 &quot;초과 게재&quot; 콘텐츠로, 애플리케이션이 필요한 것보다 더 많이 수신함을 의미합니다

이러한 문제를 해결하기 위해 GraphQL은 클라이언트가 AEM에 필요한 컨텐츠에만 쿼리하고 단일 API 호출을 사용하여 수신할 수 있는 쿼리 기반 API를 제공합니다.

* [AEM GraphQL API 시작하기 자습서](./graphql/overview.md)에서 AEM GraphQL API를 사용하는 방법을 알아봅니다

## 토큰 기반 인증 자습서

AEM은 GraphQL, AEM Content Services에서 Assets HTTP API에 헤드리스 방식으로 상호 작용할 수 있는 다양한 HTTP 엔드포인트를 제공합니다. 종종 이러한 헤드리스 소비자는 보호된 컨텐츠 또는 작업에 액세스하려면 AEM을 인증해야 할 수 있습니다. 이를 위해 AEM에서는 외부 애플리케이션, 서비스 또는 시스템에서 HTTP 요청에 대한 토큰 기반 인증을 지원합니다.

* [외부 애플리케이션 자습서에서 Cloud Service으로 AEM에 인증](./authentication/overview.md)에서 액세스 토큰을 사용하여 HTTP를 통해 AEM을 인증하는 방법을 알아봅니다

## AEM 컨텐츠 서비스 자습서

AEM Content Services는 기존 AEM 페이지를 활용하여 헤드리스 REST API 엔드포인트를 작성하고 AEM 구성 요소가 이러한 엔드포인트에 표시할 컨텐츠를 정의하거나 참조합니다.

AEM Content Services를 사용하면 AEM Sites에서 웹 페이지를 작성하는 데 사용되는 것과 동일한 컨텐츠 추상을 사용하여 이러한 HTTP API의 컨텐츠 및 스키마를 정의할 수 있습니다. AEM 페이지 및 AEM 구성 요소 를 사용하면 마케터가 모든 애플리케이션을 구동할 수 있는 유연한 JSON API를 빠르게 구성하고 업데이트할 수 있습니다.

* [AEM 컨텐츠 서비스 시작하기 자습서](./content-services/overview.md)에서 AEM 컨텐츠 서비스를 사용하는 방법을 알아봅니다.

## AEM GraphQL과 AEM 컨텐츠 서비스 비교

|  | AEM GraphQL APIs | AEM 컨텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 컨텐트 | 콘텐츠 조각 | AEM 구성 요소 |
| 컨텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 배달 형식 | GraphQL JSON | AEM ComponentExporter JSON |

## 기타 유용한 자습서

헤드리스 개념과 관련된 기타 AEM 자습서는 다음과 같습니다.

* [AEM SPA 편집기 및 Angular 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor 및 React 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)