---
title: 쿼리 필터링
description: 특정 컨텐츠 조각을 선택한 다음 해당 세부 사항을 표시할 수 있도록 하는 JavaScript 구현 .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 쿼리 필터링

이 JavaScript 및 Handlebars 예에서는 GraphQL 결과를 필터링하고 선택한 결과를 표시하는 방법을 보여 줍니다.

이 코드:

+ 연결 대상 [wknd.site](https://wknd.site)의 AEM 게시 서비스 이며, 인증이 필요하지 않습니다
+ 지속된 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`
