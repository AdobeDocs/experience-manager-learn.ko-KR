---
title: CDN 캐싱을 비활성화하는 방법
description: AEM의 as a Cloud Service CDN에서 HTTP 응답 캐싱을 비활성화하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# CDN 캐싱을 비활성화하는 방법

AEM의 as a Cloud Service CDN에서 HTTP 응답 캐싱을 비활성화하는 방법에 대해 알아봅니다. 응답 캐싱은 다음 방법으로 제어됩니다. `Cache-Control`, `Surrogate-Control`, 또는 `Expires` HTTP 응답 캐시 헤더입니다.

이러한 캐시 헤더는 일반적으로 다음을 사용하여 AEM Dispatcher vhost 구성에서 설정됩니다. `mod_headers`: 그러나 AEM Publish 자체에서 실행되는 사용자 지정 Java™ 코드에서도 설정할 수 있습니다.

## 기본 캐싱 동작

다음과 같은 경우 AEM Publish 및 Author에 대한 기본 캐싱 동작을 검토하십시오. [AEM Project Archetype](./enable-caching.md#default-caching-behavior) 기반 AEM 프로젝트가 배포됩니다.

## 캐싱 비활성화

캐싱을 해제하면 AEM as a Cloud Service 인스턴스의 성능에 부정적인 영향을 줄 수 있으므로 기본 캐싱 동작을 해제할 때 주의하십시오.

그러나 다음과 같이 캐싱을 비활성화할 수 있는 시나리오가 있습니다.

- 새 기능 개발 및 변경 사항을 즉시 확인하고자 합니다.
- 콘텐츠는 보안(인증된 사용자만 해당) 또는 동적(장바구니, 주문 세부 사항)이며 캐시되어서는 안 됩니다.

캐싱을 비활성화하려면 두 가지 방법으로 캐시 헤더를 업데이트할 수 있습니다.

1. **Dispatcher vhost 구성:** AEM 게시에만 사용할 수 있습니다.
1. **사용자 지정 Java™ 코드:** AEM Publish와 Author 모두에서 사용할 수 있습니다.

이러한 각 옵션을 검토해 보겠습니다.

### Dispatcher vhost 구성

이 옵션은 캐싱을 비활성화하는 데 권장되는 접근 방식이지만 AEM Publish에만 사용할 수 있습니다. 캐시 헤더를 업데이트하려면 `mod_headers` 모듈 및 `<LocationMatch>` Apache HTTP Server의 vhost 파일에 있는 지시문입니다. 일반 구문은 다음과 같습니다.

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### 예

의 CDN 캐싱을 비활성화하려면 **CSS 콘텐츠 유형** 일부 문제 해결 목적의 경우 다음 단계를 따르십시오.

기존 CSS 캐시를 무시하려면 CSS 파일에 대한 새 캐시 키를 생성하려면 CSS 파일을 변경해야 합니다.

1. AEM 프로젝트에서 원하는 프로젝트 파일을 찾습니다. `dispatcher/src/conf.d/available_vhosts` 디렉토리.
1. vhost 업데이트(예: `wknd.vhost`) 파일은 다음과 같습니다.

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   의 vhost 파일 `dispatcher/src/conf.d/enabled_vhosts` 디렉터리는 다음과 같습니다 **심볼릭 링크** 에 있는 파일로 `dispatcher/src/conf.d/available_vhosts` 디렉터리이므로 없는 경우 심볼릭 링크를 만들어야 합니다.
1. 를 사용하여 원하는 AEM as a Cloud Service 환경에 vhost 변경 사항을 배포합니다. [Cloud Manager - 웹 계층 구성 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) 또는 [RDE 명령](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### 사용자 지정 Java™ 코드

이 옵션은 AEM Publish와 Author 모두에서 사용할 수 있습니다. 캐시 헤더를 업데이트하려면 `SlingHttpServletResponse` 사용자 지정 Java™ 코드의 개체(Sling 서블릿, Sling 서블릿 필터). 일반 구문은 다음과 같습니다.

```java
response.setHeader("Cache-Control", "private");
```
