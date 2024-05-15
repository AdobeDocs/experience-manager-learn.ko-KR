---
title: AEM 작성자 서비스 캐싱
description: AEM as a Cloud Service 작성자 서비스 캐싱에 대한 일반 개요.
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---

# AEM Author

AEM Author는 제공하는 콘텐츠의 매우 동적이고 권한에 민감한 특성으로 인해 캐싱이 제한되었습니다. 일반적으로 AEM Author에 대한 캐싱을 사용자 지정하지 않고 대신 Adobe이 제공하는 캐시 구성을 사용하여 성능 향상을 보장하는 것이 좋습니다.

![AEM 작성자 캐싱 개요 다이어그램](./assets/author/author-all.png){align="center"}

AEM Author에서 캐싱을 사용자 지정하는 것은 권장되지 않지만 AEM Author에는 Adobe 관리 CDN이 있지만 AEM Dispatcher는 없다는 것이 좋습니다. 모든 AEM Dispatcher 구성은 AEM Dispatcher가 없으므로 AEM Author에서 무시됩니다.

## CDN

AEM Author 서비스는 CDN을 사용하지만 그 목적은 제품 리소스의 전달을 향상시키는 것이므로 광범위하게 구성하지 말고 그대로 작동하도록 하는 것입니다.

![AEM 게시 캐싱 개요 다이어그램](./assets/author/author-cdn.png){align="center"}

AEM Author CDN은 최종 사용자(일반적으로 마케터 또는 콘텐츠 작성자)와 AEM Author 사이에 있습니다. AEM 작성 환경을 향상하고 작성된 컨텐츠가 아닌 정적 에셋과 같은 변경할 수 없는 파일을 캐시합니다.

AEM 작성자의 CDN은 를 포함하여 관심 있을 수 있는 여러 유형의 리소스를 캐시합니다. [지속 쿼리에 대한 사용자 지정 TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances), 및 [사용자 지정 클라이언트 라이브러리에 대한 긴 TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### 기본 캐시 수명

다음 고객 대면 리소스는 AEM Author CDN에 의해 캐시되며 다음의 기본 캐시 수명을 갖습니다.

| 컨텐츠 유형 | 기본 CDN 캐시 수명 |
|:------------ |:---------- |
| [지속 쿼리(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1분 |
| [클라이언트 라이브러리(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30일 |
| [기타](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | 캐시되지 않음 |


## AEM Dispatcher

AEM Author 서비스는 AEM Dispatcher를 포함하지 않고 만 사용합니다. [CDN](#cdn) 캐싱용입니다.
