---
title: AEM Headless 첫 번째 튜토리얼
description: AEM Headless의 첫 번째 애플리케이션이 되는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# AEM Headless 첫 번째 튜토리얼

React, AEM Headless API 및 GraphQL을 사용하여 웹 경험을 빌드하는 방법에 대한 튜토리얼을 시작합니다. 이 튜토리얼에서는 React, Adobe Experience Manager(AEM) Headless API 및 GraphQL의 강력한 기능을 결합하여 동적이고 인터랙티브한 웹 애플리케이션을 만드는 과정을 안내합니다.

React는 단순함과 재사용성, 구성 요소 기반 아키텍처로 잘 알려진 인기 있는 JavaScript UI 라이브러리입니다. AEM은 강력한 콘텐츠 관리 기능을 제공하며, 개발자가 다양한 채널과 애플리케이션을 통해 AEM에 저장된 콘텐츠와 데이터에 액세스할 수 있도록 하는 Headless API를 제공합니다.

AEM Headless API를 활용하면 AEM 인스턴스에서 콘텐츠, 자산 및 데이터를 가져와 React 애플리케이션에 적용할 수 있습니다. API를 위한 유연한 쿼리 언어인 GraphQL은 AEM 인스턴스에서 특정 데이터를 요청하는 효율적이고 정확한 방법을 제공하여 React와 AEM 간의 원활한 통합을 가능하게 합니다.

![AEM Headless 첫 번째 튜토리얼](./assets/overview/overview.png)

이 튜토리얼에서는 GraphQL과 함께 React 및 AEM Headless API를 사용하여 웹 경험을 빌드하는 단계별 프로세스를 설명합니다. 개발 환경을 설정하는 방법, React와 AEM 간의 연결을 설정하는 방법, GraphQL 쿼리를 사용하여 콘텐츠를 검색하는 방법 및 웹 애플리케이션에서 콘텐츠를 동적으로 렌더링하는 방법을 알아봅니다.

React 프로젝트 구성, AEM을 통해 인증 설정, GraphQL을 사용하여 AEM에서 콘텐츠 쿼리, React 구성 요소에서 데이터 처리, 캐싱 및 페이지 매김을 활용한 성능 최적화 등의 주제를 다루게 됩니다.

이 튜토리얼을 완료하면 React, AEM Headless API 및 GraphQL을 활용하여 강력하고 매력적인 웹 경험을 빌드하는 방법을 확실히 이해할 수 있습니다. 그럼, 다음 웹 애플리케이션 빌드를 시작해 보겠습니다.

## 사전 요구 사항

### 기술

+ React에서의 숙련도
+ GraphQL에서의 숙련도
+ AEM as a Cloud Service에 대한 기본 지식

### AEM as a Cloud Service

이 튜토리얼을 사용하려면 AEM as a Cloud Service 환경에 대한 관리자 액세스 권한이 필요합니다.

### 소프트웨어

+ [Node.js v16 이상](https://nodejs.org/en/)
   + 명령줄에서 `node -v`를 실행하여 노드 버전 확인
+ [npm 6 이상](https://www.npmjs.com/)
   + 명령줄에서 `npm -v`를 실행하여 npm 버전 확인
+ [Git](https://git-scm.com/)
   + 명령줄에서 `git -v`를 실행하여 git 버전 확인

여러 버전의 Node.js를 동일한 컴퓨터에서 관리해야 하는 경우, [노드 버전 관리자(NVM)](https://github.com/nvm-sh/nvm)를 사용하십시오.

또한 컴퓨터에 소프트웨어를 전역적으로 설치할 수 있는 권한이 있는지 확인해야 합니다.

## 다음 단계

환경 설정이 완료되었으므로, 다음 단계인 [AEM as a Cloud Service에서 콘텐츠 설정 및 작성](./1-content-modeling.md)으로 넘어가겠습니다.
