---
title: 기본 Next.js 앱
description: WKND 모험 목록 및 세부 사항을 표시하는 기본 Next.js 앱입니다
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 6%

---

# 기본 Next.js 앱

이 [Next.js](https://nextjs.org/) 앱은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 이 애플리케이션은 필터링 가능한 WKND 모험을 렌더링하며, 모험을 선택하면 모험의 전체 세부 정보를 표시합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다
+ WKND의 지속 쿼리 `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`을(를) 사용합니다.

>[!IMPORTANT]
>
> Codesandbox.io는 포함된 IDE에서 Next.js 응용 프로그램의 편집을 지원하지 않습니다. 이 코드 샘플을 편집하려면 [codesandbox.io에서 바로 Next.js 앱을 여십시오](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
