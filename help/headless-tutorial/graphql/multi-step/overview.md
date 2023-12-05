---
title: AEM Headless 시작하기 실습형 튜토리얼 - GraphQL
description: AEM GraphQL API를 사용하여 콘텐츠를 작성하고 노출하는 방법을 소개하는 종단간 튜토리얼입니다.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 81
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 2%

---

# AEM Headless 시작하기 - GraphQL

{{aem-headless-trials-promo}}

Headless CMS 시나리오에서, AEM GraphQL API를 사용하여 콘텐츠를 작성하고 노출하고 외부 앱에서 사용하는 방법을 보여 주는 종단간 튜토리얼입니다.

이 튜토리얼에서는 AEM GraphQL API 및 Headless 기능을 사용하여 외부 앱에 표시되는 경험을 제공하는 방법을 알아봅니다.

이 튜토리얼에서는 다음 주제를 다룹니다.

* 프로젝트 구성 만들기
* 콘텐츠 조각 모델을 만들어 데이터 모델링
* 이전에 만든 모델을 기반으로 콘텐츠 조각을 만듭니다.
* 통합 GraphiQL 개발 도구를 사용하여 AEM의 콘텐츠 조각을 쿼리하는 방법을 살펴봅니다.
* AEM에 GraphQL 쿼리를 저장하거나 지속하려면
* 샘플 React 앱에서 지속 GraphQL 쿼리 사용

## 사전 요구 사항 {#prerequisites}

이 자습서를 수행하려면 다음이 필요합니다.

* 기본 HTML 및 JavaScript 기술
* 다음 도구를 로컬에 설치해야 합니다.
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE(예: [Microsoft® Visual Studio 코드](https://code.visualstudio.com/))

### AEM 환경

이 자습서를 완료하려면 AEM AEM 관리자가 as a Cloud Service 환경에 액세스하는 것이 좋습니다. AEM as a Cloud Service 환경에 액세스할 수 없는 경우 [로컬 AEM as a Cloud Service 빠른 시작 SDK](/help/cloud-service/local-development-environment/aem-runtime.md). 하지만 콘텐츠 조각 탐색과 같은 일부 제품 UI 화면은 다르다는 점에 유의해야 합니다.

## 시작해 보겠습니다!

튜토리얼 시작 [콘텐츠 조각 모델 정의](content-fragment-models.md).

## GitHub 프로젝트

소스 코드 및 컨텐츠 패키지는 [AEM 안내서 - WKND GraphQL GitHub 프로젝트](https://github.com/adobe/aem-guides-wknd-graphql).

자습서나 코드에서 문제를 발견하면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-graphql/issues).
