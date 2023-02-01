---
title: SvelteKit 앱 필터링
description: 컨텐츠 조각을 사용하여 모델링된 WKND 모험을 필터링하는 간단한 SvelteKit 앱입니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# SvelteKit 앱 필터링

AEM Headless GraphQL API를 사용하여 데이터를 필터링하는 기능 살펴보기 [SvelteKit](https://kit.svelte.dev/) 앱. 이 SvelteKit 앱은 활동 유형별로 필터링 가능한 WKND 어드벤처 목록을 만듭니다.

이 코드는 Adobe의 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) SvelteKit에서 지속되는 GraphQL 쿼리를 호출하려면 이 앱에서는 `wknd-shared/adventures-all` 지속형 쿼리로 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속형 쿼리와 지정된 활동 유형의 모험에 대한 모험 세부 사항을 검색합니다.

이 코드:

+ AEM 게시 서비스에 연결되며, 인증이 필요하지 않습니다
+ WKND의 지속적인 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-activity`
