---
title: Filtering Express app
description: A simple Express app that filters WKND adventures modeled using Content Fragments.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Filtering Express app

Explore AEM Headless GraphQL APIs ability to filter data using a [Express](https://expressjs.com/) and [Pug](https://pugjs.org/) app. This Express app creates a list of WKND adventures filterable by Activity Type.

This code demonstrates using Adobe&#39;s [AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) to invoke persisted GraphQL queries using Node.js-based JavaScript. 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속 쿼리에 전달되고 지정된 활동 유형의 모험에 대한 모험 세부 정보만 검색합니다. Adventure details are retrieved from AEM via the `wknd-shared/adventures-by-slug` persisted query.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다
+ Uses the WKND&#39;s persisted queries: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, and `wknd-shared/adventures-by-slug`
