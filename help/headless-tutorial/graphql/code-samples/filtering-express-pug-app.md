---
title: 빠른 앱 필터링
description: 콘텐츠 조각을 사용하여 모델링된 WKND 모험을 필터링하는 간단한 Express 앱입니다.
version: Cloud Service
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
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 빠른 앱 필터링

다음을 사용하여 데이터를 필터링하는 AEM Headless GraphQL API 기능 살펴보기 [고속](https://expressjs.com/) 및 [Pug](https://pugjs.org/) 앱. 이 Express 앱에서는 활동 유형별로 필터링 가능한 WKND 모험 목록을 만듭니다.

이 코드는 Adobe의 [NodeJS용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) Node.js 기반 JavaScript를 사용하여 지속 GraphQL 쿼리를 호출하려면 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속 쿼리를 통해 지정된 활동 유형의 모험에 대해서만 어드벤처 세부 정보를 검색합니다. 어드벤처 세부 사항은 를 통해 AEM에서 검색됩니다. `wknd-shared/adventures-by-slug` 지속 쿼리.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리를 사용합니다. `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, 및 `wknd-shared/adventures-by-slug`
