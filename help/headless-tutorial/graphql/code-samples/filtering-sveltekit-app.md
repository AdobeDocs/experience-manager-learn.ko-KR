---
title: Simple SvelteKit 앱
description: 콘텐츠 조각을 사용하여 모델링된 WKND 모험을 표시하는 간단한 SvelteKit 앱입니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 37
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# SvelteKit 앱 필터링

다음을 사용하여 데이터를 나열하는 AEM Headless GraphQL API 기능 살펴보기 [SvelteKit](https://kit.svelte.dev/) 앱. 이 SvelteKit 앱은 WKND 모험 목록을 만들며, 이 목록을 선택하여 모험 세부 사항을 표시할 수 있습니다.

이 코드는 Adobe의 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) SvelteKit에서 지속 GraphQL 쿼리를 호출합니다. 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 어드벤처 세부 정보는 다음을 통해 요청됨: `wknd-shared/adventures-by-slug` 지속 쿼리.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`
