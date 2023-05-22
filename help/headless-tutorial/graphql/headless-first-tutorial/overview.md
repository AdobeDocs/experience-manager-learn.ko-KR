---
title: AEM Headless 첫 번째 자습서
description: AEM Headless 첫 번째 애플리케이션이 되는 방법을 알아봅니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# AEM Headless 첫 번째 자습서

{{aem-headless-trials-promo}}

AEM Headless API 및 GraphQL에서 완벽하게 작동하는 React를 사용하여 웹 경험을 구축하는 방법에 대한 자습서를 시작합니다. 이 튜토리얼에서는 React, Adobe Experience Manager(AEM) Headless API 및 GraphQL의 기능을 결합하여 동적 및 대화형 웹 애플리케이션을 만드는 프로세스를 안내합니다.

React는 간소화, 재사용성 및 구성 요소 기반 아키텍처로 알려진 사용자 인터페이스 구축을 위해 널리 사용되는 JavaScript 라이브러리입니다. AEM은 강력한 컨텐츠 관리 기능을 제공하며 개발자가 다양한 채널 및 애플리케이션을 통해 AEM에 저장된 컨텐츠 및 데이터에 액세스할 수 있도록 해주는 Headless API를 제공합니다.

AEM Headless API를 활용하면 AEM 인스턴스에서 콘텐츠, 에셋 및 데이터를 검색하고 이를 사용하여 React 애플리케이션을 실행할 수 있습니다. API에 대한 유연한 쿼리 언어인 GraphQL은 AEM 인스턴스에서 특정 데이터를 요청하는 효율적이고 정확한 방법을 제공하여 React와 AEM 간의 원활한 통합을 지원합니다.

![AEM Headless 첫 번째 자습서](./assets/overview/overview.png)

이 자습서에서는 GraphQL과 함께 React 및 AEM Headless API를 사용하여 웹 경험을 구축하는 단계별 프로세스를 안내합니다. 개발 환경을 설정하고, React와 AEM을 연결하고, GraphQL 쿼리를 사용하여 콘텐츠를 검색하고, 웹 애플리케이션에서 동적으로 렌더링하는 방법에 대해 알아봅니다.

React 프로젝트 구성, AEM으로 인증 설정, GraphQL을 사용하여 AEM에서 콘텐츠 쿼리, React 구성 요소의 데이터 처리, 캐싱 및 페이지 매김 기능을 활용하여 성능 최적화 등의 주제를 다룹니다.

이 자습서를 마칠 때까지 React, AEM Headless API 및 GraphQL을 활용하여 강력하고 매력적인 웹 경험을 구축하는 방법에 대해 확실히 이해할 수 있습니다. 이제 자세히 살펴보고 다음 웹 애플리케이션을 빌드해 보겠습니다!

## 사전 요구 사항

### 기술

+ React 숙련도
+ GraphQL에서의 숙련도
+ AEM에 대한 기본 지식 as a Cloud Service

### AEM as a Cloud Service

이 자습서에서는 관리자가 AEM as a Cloud Service 환경에 액세스해야 합니다.

### 소프트웨어

+ [Node.js v16+](https://nodejs.org/en/)
   + 를 실행하여 노드 버전을 확인합니다. `node -v` 명령줄에서
+ [npm 6+](https://www.npmjs.com/)
   + 를 실행하여 npm 버전 확인 `npm -v` 명령줄에서
+ [Git](https://git-scm.com/)
   + 를 실행하여 git 버전 확인 `git -v` 명령줄에서

사용 [노드 버전 관리자(nvm)](https://github.com/nvm-sh/nvm) 동일한 컴퓨터에 여러 버전의 node.js가 있는 문제를 해결했습니다.

컴퓨터에 소프트웨어를 전체적으로 설치할 수 있는 권한이 있는지 확인하십시오.

## 다음 단계

이제 환경이 설정되었으므로 다음 단계로 이동하겠습니다. [AEM as a Cloud Service으로 콘텐츠 설정 및 작성](./1-content-modeling.md)
