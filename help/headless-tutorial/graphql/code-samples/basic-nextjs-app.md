---
title: 기본 Next.js 앱
description: WKND 모험 목록 및 세부 사항을 표시하는 기본 Next.js 앱입니다
version: Cloud Service
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
duration: 25
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# 기본 Next.js 앱

이 [Next.js](https://nextjs.org/) 앱은 지속 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. 이 애플리케이션은 필터링 가능한 WKND 모험을 렌더링하며, 모험을 선택하면 모험의 전체 세부 정보를 표시합니다.

이 코드:

+ AEM Publish 서비스에 연결하며 인증이 필요하지 않습니다.
+ WKND의 지속 쿼리를 사용합니다. `wknd-shared/adventures-all` 및 `wknd-shared/adventures-by-slug`

이 Next.js 앱이 빌드되는 방식에 대한 자세한 내용을 보려면 [예제 Next.js 앱 설명서](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io는 포함된 IDE에서 Next.js 응용 프로그램의 편집을 지원하지 않습니다. 이 코드 샘플을 편집하려면 [codesandbox.io에서 Next.js 앱을 바로 엽니다.](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
