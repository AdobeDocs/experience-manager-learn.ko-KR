---
title: 기본 React 앱
description: WKND 모험과 그 세부 사항 목록을 표시하는 기본 React 앱입니다
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---


# 기본 React 앱

이 [React](https://reactjs.org/) 앱에서는 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 이 응용 프로그램은 WKND Adventures의 필터링 가능 항목을 렌더링하고, 탐색을 선택하면 모험의 세부 사항이 표시됩니다.

이 코드:

+ AEM 게시 서비스에 연결되며, 인증이 필요하지 않습니다
+ WKND의 지속적인 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`

이 Next.js 앱이 빌드되는 방식에 대한 심층적인 검토를 위해 [React 앱 설명서 예](../example-apps/react-app.md).
