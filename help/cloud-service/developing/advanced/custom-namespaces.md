---
title: 사용자 정의 네임스페이스
description: 사용자 정의 네임스페이스를 정의하고 AEM에 as a Cloud Service으로 배포하는 방법에 대해 알아봅니다.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 520
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# 사용자 정의 네임스페이스

사용자 정의 정의 및 배포 방법 알아보기 [네임스페이스](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM as a Cloud Service으로.

사용자 정의 네임스페이스는 JCR 속성 중 `:`. AEM에서는 다음과 같은 여러 네임스페이스를 사용합니다.

+ `jcr` JCR 시스템 속성용
+ `cq` AEM(이전의 Adobe CQ) 속성용
+ `dam` DAM 에셋과 관련된 AEM 속성의 경우
+ `dc` 더블린 코어 속성용

... 그리고 많은 다른 사람.

네임스페이스를 사용하여 속성의 범위와 의도를 표시할 수 있습니다. 사용자 지정 네임스페이스(종종 회사 이름)를 만들면 AEM 구현과 관련된 노드 또는 속성을 명확하게 식별하고 비즈니스 관련 데이터를 포함할 수 있습니다.

사용자 정의 네임스페이스는에서 관리됩니다 [Sling 저장소 초기화(repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) AEM 스크립트 및 를 OSGi 구성으로서 as a Cloud Service으로 배포하고 [AEM 프로젝트](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) `ui.config` 프로젝트.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## 리소스

+ [Sling Repository Initialization (repoinit) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 코드

다음 코드를 사용하여 `wknd` 네임스페이스입니다.

### RepositoryInitializer OSGI 구성

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

이렇게 하면 다음을 사용하여 사용자 지정 속성을 사용할 수 있습니다. `wknd` 네임스페이스, 이름 뒤에 있는 첫 번째 매개 변수로 표시 `register namespace` AEM에서 사용할 지침 고급 스크립트 정의를 보려면 [Sling Repository Initialization (repoinit) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
