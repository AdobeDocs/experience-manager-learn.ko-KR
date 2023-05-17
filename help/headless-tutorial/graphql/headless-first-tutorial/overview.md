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
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# AEM Headless 첫 번째 자습서

![AEM Headless 첫 번째 자습서](./assets/overview/overview.png)

AEM Headless API 및 GraphQL에서 완전히 제공되는 React를 사용하여 웹 경험을 구축하는 방법에 대한 자습서를 시작합니다. 이 자습서에서는 React, Adobe Experience Manager(AEM) Headless API 및 GraphQL의 강력한 기능을 결합하여 동적 및 대화형 웹 애플리케이션을 만드는 과정을 안내합니다.

React는 단순성, 재유용성 및 구성 요소 기반 아키텍처로 알려진 사용자 인터페이스를 빌드하기 위해 널리 사용되는 JavaScript 라이브러리입니다. AEM은 강력한 컨텐츠 관리 기능을 제공하며 개발자가 다양한 채널 및 애플리케이션을 통해 AEM에 저장된 컨텐츠 및 데이터에 액세스할 수 있도록 해주는 헤드리스 API를 표시합니다.

AEM 헤드리스 API를 활용하면 AEM 인스턴스에서 컨텐츠, 자산 및 데이터를 검색하고 이 API를 사용하여 React 애플리케이션을 실행할 수 있습니다. API를 위한 유연한 쿼리 언어인 GraphQL을 사용하면 AEM 인스턴스에서 특정 데이터를 빠르고 정확하게 요청할 수 있으므로 React와 AEM 간에 원활하게 통합할 수 있습니다.

이 자습서에서는 GraphQL과 React 및 AEM Headless API를 사용하여 웹 경험을 구축하는 단계별 프로세스를 살펴봅니다. 개발 환경을 설정하고, React와 AEM 간의 연결을 설정하고, GraphQL 쿼리를 사용하여 콘텐츠를 검색하고, 웹 애플리케이션에서 동적으로 렌더링하는 방법을 알아봅니다.

React 프로젝트 구성, AEM으로 인증 설정, GraphQL을 사용하여 AEM에서 컨텐츠 쿼리, React 구성 요소에서 데이터 처리, 캐싱 및 페이지 매기기를 활용하여 성능 최적화와 같은 주제를 다룹니다.

이 자습서를 마치면 React, AEM Headless API 및 GraphQL을 활용하여 강력하고 매력적인 웹 경험을 구축하는 방법에 대해 명확하게 이해할 수 있습니다. 자, 이제 새로운 웹 애플리케이션 구축을 시작해 보겠습니다!

## 사전 요구 사항

### 기술

+ React 숙련도
+ GraphQL의 숙련도
+ AEM as a Cloud Service 기본 지식

### AEM as a Cloud Service

이 자습서에서는 AEM as a Cloud Service 환경에 대한 관리자 액세스 권한이 필요합니다.

### 소프트웨어

+ [Node.js v16+](https://nodejs.org/en/)
   + 를 실행하여 노드 버전을 확인합니다 `node -v` 명령줄에서
+ [npm 6+](https://www.npmjs.com/)
   + 를 실행하여 npm 버전 확인 `npm -v` 명령줄에서
+ [Git](https://git-scm.com/)
   + 실행 `git -v` 명령줄에서

사용 [노드 버전 관리자(nvm)](https://github.com/nvm-sh/nvm) 동일한 컴퓨터에 node.js의 여러 버전이 있는 주소를 확인하려면 다음을 수행하십시오.

컴퓨터에 소프트웨어를 전체적으로 설치할 수 있는 권한이 있는지 확인합니다.

## 다음 단계

환경이 설정되었으므로 다음 단계로 넘어갑시다. [AEM as a Cloud Service에서 컨텐츠 설정 및 작성](./1-content-modeling.md)
