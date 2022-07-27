---
title: AEM Headless 모바일 배포
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEM Headless 모바일 배포

AEM 헤드리스 모바일 배포는 iOS, Android 등을 위한 기본 모바일 앱입니다. 헤드리스 방식으로 AEM에서 컨텐츠를 사용하고 상호 작용할 수 있습니다.

AEM Headless API에 대한 HTTP 연결이 브라우저 컨텍스트에서 시작되지 않으므로 모바일 배포에서는 최소 구성이 필요합니다.

## 배포 구성

| 모바일 앱 연결 대상 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [CORS 구성](./cors.md) | ✘ | ✘ | ✘ |
| 이미지 URL 호스트 이름 | ✔ | ✔ | ✔ |

## 예제 모바일 앱

Adobe은 iOS 및 Android 모바일 앱의 예를 제공합니다.


