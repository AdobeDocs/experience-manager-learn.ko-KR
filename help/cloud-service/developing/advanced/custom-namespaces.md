---
title: 사용자 지정 네임스페이스
description: 사용자 지정 네임스페이스를 정의하고 AEM as a Cloud Service에 배포하는 방법을 알아봅니다.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# 사용자 지정 네임스페이스

사용자 정의 정의 및 배포 방법 알아보기 [네임스페이스](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM as a Cloud Service 으로 이동합니다.

사용자 지정 네임스페이스는 `:`. AEM에서는 다음과 같은 여러 네임스페이스를 사용합니다.

+ `jcr` JCR 시스템 속성의 경우
+ `cq` AEM(이전 Adobe CQ) 속성의 경우
+ `dam` DAM 자산과 관련된 AEM 속성의 경우
+ `dc` 더블린 코어 속성의 경우

... 그리고 많은 다른 사람들.

네임스페이스는 속성의 범위와 의도를 나타내는 데 사용할 수 있습니다. 사용자 지정 네임스페이스(일반적으로 회사 이름)를 만들면 AEM 구현에 관련된 노드나 속성을 명확히 식별하고 비즈니스 관련 데이터를 포함하는 데 도움이 됩니다.

사용자 지정 네임스페이스는 [Sling Repository 초기화(포인터)](https://sling.apache.org/documentation/bundles/repository-initialization.html) 스크립트를 작성하고 AEM as a Cloud Service에 OSGi 구성으로 배포하고 다음 구성에 추가됩니다. [AEM 프로젝트](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) `ui.config` 프로젝트.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## 리소스

+ [Sling Repository Initialization(포인터) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 코드

다음 코드는 `wknd` 네임스페이스.

### RepositoryInitializer OSGi 구성

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

이를 통해 `wknd` 네임스페이스(다음 항목 뒤에 첫 번째 매개 변수로 표시됨) `register namespace` AEM에서 사용할 명령. 고급 스크립트 정의를 보려면 [Sling Repository Initialization(포인터) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
