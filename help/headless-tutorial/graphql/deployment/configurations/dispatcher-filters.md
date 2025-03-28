---
title: AEM GraphQL용 Dispatcher 필터
description: AEM GraphQL에서 사용할 AEM Publish Dispatcher 필터를 구성하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Dispatcher 필터

Adobe Experience Manager as a Cloud Service은 AEM 게시 Dispatcher 필터를 사용하여 AEM에 연결해야 하는 요청만 AEM에 전달되도록 합니다. 기본적으로 모든 요청이 거부되며 허용된 URL에 대한 패턴을 명시적으로 추가해야 합니다.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher 필터 구성 필요 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하십시오.

## Dispatcher 필터 구성

AEM Publish Dispatcher 필터 구성은 AEM에 연결할 수 있는 URL 패턴을 정의하며 AEM 지속 쿼리 끝점의 URL 접두사를 포함해야 합니다.

| 클라이언트가에 연결 | AEM Author | AEM 게시 | AEM 미리보기 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher 필터 구성 필요 | ✘ | ✔ | ✔ |

URL 패턴이 `/graphql/execute.json/*`인 `allow` 규칙을 추가하고 파일 ID(예: `/0600`, 예제 팜 파일에서 고유함)를 확인하십시오.
이렇게 하면 HTTP GET 요청이 지속 쿼리 끝점(예: `HTTP GET /graphql/execute.json/wknd-shared/adventures-all`부터 AEM 게시까지)에 도달할 수 있습니다.

AEM Headless 경험에서 경험 조각을 사용하는 경우 이러한 경로에 대해 동일한 작업을 수행합니다.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### 필터 구성 예

+ [Dispatcher 필터의 예는 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
