---
title: AEM Headless 실습 튜토리얼 시작하기 - GraphQL
description: AEM GraphQL API를 사용하여 콘텐츠를 작성하고 노출하는 방법을 소개하는 전체 튜토리얼입니다.
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
duration: 54
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 88%

---

# AEM Headless 시작하기 - GraphQL

Headless CMS 시나리오에서 AEM의 GraphQL API를 사용하여 콘텐츠를 작성 및 공개하고 외부 앱에서 사용하는 방법을 소개하는 전체 튜토리얼입니다.

이 튜토리얼에서는 AEM의 GraphQL API 및 Headless 기능을 사용하여 외부 앱에 표시되는 경험을 강화할 수 있는 방법을 알아봅니다.

이 튜토리얼에서는 다음 주제를 다룹니다.

* 프로젝트 구성 만들기
* 데이터 모델링을 위한 콘텐츠 조각 모델 만들기
* 이전에 만든 모델을 기반으로 콘텐츠 조각 만들기.
* 통합된 GraphiQL 개발 도구를 사용하여 AEM의 콘텐츠 조각을 쿼리하는 방법 살펴보기.
* GraphQL 쿼리를 AEM에 저장 또는 유지
* 샘플 React 앱에서 지속 GraphQL 쿼리 사용

## 사전 요구 사항 {#prerequisites}

이 튜토리얼을 따르려면 다음 사항이 필요합니다.

* 기본 HTML 및 JavaScript 기술
* 다음 도구가 로컬에 설치되어 있어야 합니다.
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE(예: [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM 환경

이 튜토리얼을 완료하려면 AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스 권한이 있는 것이 좋습니다.

## 시작하기

[콘텐츠 조각 모델 정의](content-fragment-models.md)(으)로 자습서를 시작하십시오.

## GitHub 프로젝트

소스 코드 및 컨텐츠 패키지는 `basic-tutorial`AEM Guides - WKND GraphQL GitHub 프로젝트[의 ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial) 폴더에서 사용할 수 있습니다.


튜토리얼이나 코드에 문제가 있는 경우 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-graphql/issues)를 남겨주십시오.
