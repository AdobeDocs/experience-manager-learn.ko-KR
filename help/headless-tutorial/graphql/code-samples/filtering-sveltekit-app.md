---
title: 간단한 SvelteKit 앱
description: 컨텐츠 조각을 사용하여 모델링된 WKND 모험을 표시하는 간단한 SvelteKit 앱입니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# SvelteKit 앱 필터링

AEM Headless GraphQL API를 사용하여 데이터를 나열하는 기능을 살펴보십시오. [SvelteKit](https://kit.svelte.dev/) 앱. 이 SvelteKit 앱은 모험의 세부 사항을 표시하기 위해 선택할 수 있는 WKND 어드벤처 목록을 만듭니다.

이 코드는 Adobe의 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) SvelteKit에서 지속되는 GraphQL 쿼리를 호출하려면 이 앱에서는 `wknd-shared/adventures-all` 지속형 쿼리로 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 어드벤처 세부 사항은 `wknd-shared/adventures-by-slug` 지속형 쿼리입니다.

이 코드:

+ AEM 게시 서비스에 연결되며, 인증이 필요하지 않습니다
+ WKND의 지속적인 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`
