---
title: AEM GraphQL에 대한 디스패처 필터
description: AEM GraphQL에서 사용할 AEM Publish Dispatcher 필터를 구성하는 방법을 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# 디스패처 필터

Adobe Experience Manager as a Cloud Service은 AEM 게시 디스패처 필터를 사용하여 AEM에 도달해야 하는 요청만 AEM에 도달하도록 합니다. 기본적으로 모든 요청이 거부되며 허용된 URL에 대한 패턴을 명시적으로 추가해야 합니다.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher 필터 구성 필요 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하도록 합니다.

## Dispatcher 필터 구성

AEM 게시 디스패처 필터 구성은 AEM에 도달하기 위해 허용되는 URL 패턴을 정의하고 AEM 지속적인 쿼리 끝점에 대한 URL 접두사를 포함해야 합니다.

| 클라이언트 연결 대상 | AEM Author | AEM 게시 | AEM 미리 보기 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher 필터 구성 필요 | ✘ | ✔ | ✔ |

추가 `allow` url 패턴을 사용하는 규칙 `/graphql/execute.json/*`, 및 파일 ID(예: `/0600`는 예제 팜 파일에서 고유합니다.)
이렇게 하면 다음과 같이 지속된 쿼리 종단점에 HTTP GET 요청을 수행할 수 있습니다 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` 를 통해 AEM Publish에 연결할 수 있습니다.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### 필터 구성 예

+ [Dispatcher 필터의 예는 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)