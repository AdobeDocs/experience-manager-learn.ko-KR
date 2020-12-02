---
title: AEM 헤드리스 자습서
description: 헤드리스 CMS로 Adobe Experience Manager을 사용하는 방법에 대한 자습서 모음입니다.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# AEM 헤드리스 자습서

Adobe Experience Manager에는 헤드리스 끝점을 정의하고 콘텐츠를 JSON으로 전달하기 위한 다양한 옵션이 있습니다. 실습 위주의 튜토리얼을 통해 다양한 옵션을 사용하여 여러분에게 딱 맞는 옵션을 선택하는 방법을 살펴볼 수 있습니다.

## AEM GraphQL API 자습서

>[!CAUTION]
>
> 컨텐츠 조각 전달용 AEM GraphQL API는 2021년 초에 릴리스됩니다.
> 관련 설명서는 미리 볼 수 있습니다.

컨텐츠 조각용 AEM GraphQL API
에서는 외부 클라이언트 애플리케이션이 AEM에서 관리되는 컨텐츠를 사용하여 경험을 렌더링하는 헤드리스 CMS 시나리오를 지원합니다.

최신 컨텐츠 전달 API는 Javascript 기반의 프런트 엔드 애플리케이션의 효율성과 성능을 높여줍니다. REST API를 사용하는 경우 다음과 같은 문제가 발생합니다.

* 한 번에 하나의 개체를 가져오는 요청이 많습니다.
* 종종 &quot;과잉 제공&quot; 컨텐츠로, 이는 애플리케이션이 필요 이상으로 수신함을 의미합니다

이러한 문제를 해결하기 위해 GraphQL은 클라이언트가 AEM에 필요한 컨텐츠만 쿼리하고 단일 API 호출을 사용하여 수신하도록 허용하는 쿼리 기반 API를 제공합니다.

* AEM GraphQL API를 사용하는 방법에 대해 알아보려면 [AEM GraphQL API 시작하기 자습서](./graphql/overview.md)

## AEM 컨텐츠 서비스 자습서

AEM Content Services는 기존 AEM 페이지를 활용하여 헤드리스 REST API 끝점을 구성하고 AEM 구성 요소는 이러한 끝점에 표시할 컨텐츠를 정의하거나 참조합니다.

AEM Content Services에서는 AEM Sites에서 웹 페이지를 작성하는 데 사용되는 동일한 컨텐츠 추상적인 기능을 사용하여 이러한 HTTP API의 컨텐츠와 스키마를 정의할 수 있습니다. AEM 페이지 및 AEM 구성 요소를 사용하면 마케터는 모든 애플리케이션에 영향을 줄 수 있는 유연한 JSON API를 신속하게 구성하고 업데이트할 수 있습니다.

* AEM Content Services를 사용하는 방법에 대해 학습하려면 [AEM Content Services 시작하기 자습서](./content-services/overview.md)

## AEM GraphQL과 AEM 컨텐츠 서비스 비교

|  | AEM GraphQL API | AEM 컨텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 컨텐트 | 컨텐츠 조각 | AEM 구성 요소 |
| 컨텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 배달 형식 | GraphQL JSON | AEM ComponentExporter JSON |

## 기타 유용한 자습서

헤드리스 개념과 관련된 기타 AEM 자습서는 다음과 같습니다.

* [AEM SPA 편집기 및 Angular 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor 및 React 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)