---
title: jQuery 및 Handlebars를 사용한 필터링
description: 표시할 WKND 모험 을 필터링하는 jQuery 및 Handlebars를 사용하는 JavaScript 구현입니다. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# jQuery 및 Handlebars를 사용한 필터링

를 사용하는 JavaScript 앱을 사용하여 데이터를 필터링하는 AEM Headless GraphQL API 기능 살펴보기 [jQuery](https://jquery.com/) 및 [핸들바](https://handlebarsjs.com/). 이 앱은 활동 유형별로 필터링 가능한 WKND 모험 목록을 만듭니다.

이 코드는 Adobe의 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 지속 GraphQL 쿼리를 호출합니다. 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속 쿼리를 통해 지정된 활동 유형의 모험에 대해서만 어드벤처 세부 정보를 검색합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-activity`
