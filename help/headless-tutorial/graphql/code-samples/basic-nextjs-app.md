---
title: 기본 Next.js 앱
description: WKND 모험 및 세부 사항 목록을 표시하는 기본 Next.js 앱
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: e3fb145e7a9f33dd010f6c40e42573d41e54b302
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# 기본 Next.js 앱

이 [Next.js](https://nextjs.org/) 앱에서는 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 이 응용 프로그램은 WKND Adventures의 필터링 가능 항목을 렌더링하고, 탐색을 선택하면 모험의 세부 사항이 표시됩니다.

이 코드:

+ AEM 게시 서비스에 연결되며, 인증이 필요하지 않습니다
+ WKND의 지속적인 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`

이 Next.js 앱이 빌드되는 방식에 대한 심층적인 검토를 위해 [예제 Next.js 앱 설명서](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io는 포함된 IDE에서 Next.js 애플리케이션 편집을 지원하지 않습니다. 이 코드 샘플을 편집하려면 [codesandbox.io에서 직접 Next.js 앱 열기](https://codesandbox.io/s/wknd-next-js-app-3n6zdv).
