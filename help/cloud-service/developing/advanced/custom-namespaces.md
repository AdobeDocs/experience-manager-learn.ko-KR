---
title: 사용자 정의 네임스페이스
description: 사용자 지정 네임스페이스를 정의하고 AEM as a Cloud Service에 배포하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# 사용자 정의 네임스페이스

사용자 지정 [네임스페이스](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html)를 정의하고 AEM as a Cloud Service에 배포하는 방법에 대해 알아봅니다.

사용자 지정 네임스페이스는 `:` 앞에 있는 JCR 속성의 선택적 부분입니다. AEM에서는 다음과 같은 여러 네임스페이스를 사용합니다.

+ JCR 시스템 속성에 대한 `jcr`
+ AEM(이전의 Adobe CQ) 속성에 대한 `cq`
+ DAM 자산과 관련된 AEM 속성의 `dam`
+ 더블린 코어 속성에 대한 `dc`

... 그리고 많은 다른 사람.

네임스페이스를 사용하여 속성의 범위와 의도를 표시할 수 있습니다. 사용자 지정 네임스페이스(종종 회사 이름)를 만들면 AEM 구현과 관련된 노드 또는 속성을 명확하게 식별하고 비즈니스 관련 데이터를 포함할 수 있습니다.

사용자 지정 네임스페이스는 [Sling Repository Initialization(repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) 스크립트에서 관리되고 OSGi 구성으로 AEM as a Cloud Service에 배포되며 [AEM 프로젝트의](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko) `ui.config` 프로젝트에 추가됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416848?quality=12&learn=on&captions=kor)

## 리소스

+ [Sling 저장소 초기화(repoinit) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 코드

다음 코드는 `wknd` 네임스페이스를 구성하는 데 사용됩니다.

### RepositoryInitializer OSGI 구성

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

이렇게 하면 `register namespace` 명령 뒤에 첫 번째 매개 변수로 표시된 `wknd` 네임스페이스를 사용하는 사용자 지정 속성을 AEM에서 사용할 수 있습니다. 고급 스크립트 정의를 보려면 [Sling 저장소 초기화(repoinit) 설명서](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)의 예제를 검토하십시오.
