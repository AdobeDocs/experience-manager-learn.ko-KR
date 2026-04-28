---
title: 기본 Next.js 앱
description: A basic Next.js app that displays a list of WKND adventures and their details
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
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 5%

---

# 기본 Next.js 앱

This [Next.js](https://nextjs.org/) app demonstrates how to query content using AEM&#39;s GraphQL APIs using persisted queries. 이 애플리케이션은 필터링 가능한 WKND 모험을 렌더링하며, 모험을 선택하면 모험의 전체 세부 정보를 표시합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다
+ WKND의 지속 쿼리 `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`을(를) 사용합니다.

>[!IMPORTANT]
>
> Codesandbox.io does not support editting of Next.js application in the embedded IDE. To edit this code sample, [open the Next.js app directly on codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
