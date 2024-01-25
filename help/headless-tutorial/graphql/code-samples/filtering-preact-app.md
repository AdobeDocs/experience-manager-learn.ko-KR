---
title: 사전 설정 앱 필터링
description: 콘텐츠 조각을 사용하여 모델링된 WKND 모험을 필터링하는 간단한 사전 설정 앱입니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 사전 설정 앱 필터링

다음을 사용하여 데이터를 필터링하는 AEM Headless GraphQL API 기능 살펴보기 [전치사](https://preactjs.com/) 앱. 이 사전 설정 앱은 활동 유형별로 필터링 가능한 WKND 모험 목록을 생성합니다.

이 코드는 Adobe의 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) React에서 지속된 GraphQL 쿼리를 호출합니다. 이 앱은 `wknd-shared/adventures-all` 지속 쿼리를 사용하여 모든 모험을 수집하고 사용 가능한 활동 유형 목록을 파생합니다. 사용자가 활동 유형을 선택하면 선택한 유형이 `wknd-shared/adventures-by-activity` 지속 쿼리를 통해 지정된 활동 유형의 모험에 대해서만 어드벤처 세부 정보를 검색합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-activity`
