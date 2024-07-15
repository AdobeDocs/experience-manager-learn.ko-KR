---
title: 기본 React 앱
description: WKND 모험 및 세부 사항 목록을 표시하는 기본 React 앱
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 17
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# 기본 React 앱

이 [React](https://reactjs.org/) 앱은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 이 애플리케이션은 필터링 가능한 WKND 모험을 렌더링하며, 모험을 선택하면 모험의 전체 세부 정보를 표시합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리 `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`을(를) 사용합니다.

이 Next.js 앱의 빌드 방법에 대한 자세한 내용을 보려면 [예제 React 앱 설명서](../example-apps/react-app.md)를 검토하십시오.
