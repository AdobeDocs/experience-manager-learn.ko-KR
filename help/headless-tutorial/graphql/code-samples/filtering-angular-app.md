---
title: Angular 앱 필터링
description: A simple Angular app that filters WKND adventures modeled using Content Fragments.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 5%

---

# Angular 앱 필터링

Explore AEM Headless GraphQL APIs ability to filter data using a [Angular](https://angular.io/) app. This Angular app creates a list of WKND adventures filterable by Activity Type.

This code demonstrates using Adobe&#39;s [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) to invoke persisted GraphQL queries from Angular. 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속 쿼리에 전달되고 지정된 활동 유형의 모험에 대한 모험 세부 정보만 검색합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다
+ WKND의 지속 쿼리 `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-activity`을(를) 사용합니다.
