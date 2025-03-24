---
title: CDN 캐싱을 활성화하는 방법
description: AEM as a Cloud Service CDN에서 HTTP 응답 캐싱을 활성화하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# CDN 캐싱을 활성화하는 방법

AEM as a Cloud Service CDN에서 HTTP 응답 캐싱을 활성화하는 방법을 알아봅니다. 응답 캐싱은 `Cache-Control`, `Surrogate-Control` 또는 `Expires` HTTP 응답 캐시 헤더에 의해 제어됩니다.

이러한 캐시 헤더는 일반적으로 `mod_headers`을(를) 사용하여 AEM Dispatcher vhost 구성에서 설정되지만 AEM 게시 자체에서 실행되는 사용자 지정 Java™ 코드에서도 설정할 수 있습니다.

## 기본 캐싱 동작

사용자 지정 구성이 없으면 기본값이 사용됩니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 기반 `mynewsite` AEM 프로젝트가 배포되면 다음 스크린샷에서 AEM Publish 및 Author의 기본 캐싱 동작을 확인할 수 있습니다.

![기본 캐싱 동작](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

자세한 내용은 [AEM 게시 - 기본 캐시 수명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) 및 [AEM 작성자 - 기본 캐시 수명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life)을 검토하십시오.

요약하면, AEM as a Cloud Service은 AEM Publish에서 대부분의 콘텐츠 유형(HTML, JSON, JS, CSS 및 Assets)과 AEM Author에서 몇 가지 콘텐츠 유형(JS, CSS)을 캐시합니다.

## 캐싱 활성화

기본 캐싱 동작을 변경하려면 두 가지 방법으로 캐시 헤더를 업데이트할 수 있습니다.

1. **Dispatcher vhost 구성:** AEM 게시에만 사용할 수 있습니다.
1. **사용자 지정 Java™ 코드:**&#x200B;은(는) AEM 게시와 작성자 모두에서 사용할 수 있습니다.

이러한 각 옵션을 검토해 보겠습니다.

### Dispatcher vhost 구성

이 옵션은 캐싱을 활성화하는 데 권장되는 방법입니다. 하지만 AEM 게시에만 사용할 수 있습니다. 캐시 헤더를 업데이트하려면 Apache HTTP Server의 vhost 파일에서 `mod_headers` 모듈 및 `<LocationMatch>` 지시문을 사용합니다. 일반 구문은 다음과 같습니다.

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

다음은 각 **header**&#x200B;의 목적과 헤더에 적용할 수 있는 **attributes**&#x200B;을 요약합니다.

|                     | 웹 브라우저 | CDN | 설명 |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | 이 헤더는 웹 브라우저 및 CDN 캐시 수명을 제어합니다. |
| Surrogate-Control | ✘ | ✔ | 이 헤더는 CDN 캐시 수명을 제어합니다. |
| 만료 | ✔ | ✔ | 이 헤더는 웹 브라우저 및 CDN 캐시 수명을 제어합니다. |


- **max-age**: 이 특성은 응답 콘텐츠의 TTL 또는 &quot;Time to Live&quot;를 초 단위로 제어합니다.
- **stale-while-revalidate**: 이 특성은 수신된 요청이 지정된 기간(초) 내에 있을 때 CDN 계층에서 응답 콘텐츠의 _stale 상태_ 처리를 제어합니다. _부실 상태_&#x200B;은(는) TTL이 만료된 후 응답을 다시 확인하기 전 기간입니다.
- **stale-if-error**: 원본 서버를 사용할 수 없고 수신된 요청이 지정된 기간(초) 내에 있는 경우 이 특성은 CDN 계층에서 응답 콘텐츠의 _stale 상태_ 처리를 제어합니다.

자세한 내용은 [안정성 및 유효성 재검사](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) 세부 정보를 검토하십시오.

#### 예

부실 상태 처리 없이 **HTML 콘텐츠 형식**&#x200B;의 웹 브라우저 및 CDN 캐시 수명을 _10분_(으)로 늘리려면 다음 단계를 수행합니다.

1. AEM 프로젝트에서 `dispatcher/src/conf.d/available_vhosts` 디렉터리에서 원하는 가상 파일을 찾습니다.
1. vhost(예: `wknd.vhost`) 파일을 다음과 같이 업데이트합니다.

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   `dispatcher/src/conf.d/enabled_vhosts` 디렉터리의 vhost 파일이 `dispatcher/src/conf.d/available_vhosts` 디렉터리의 파일에 대해 **symlinks**&#x200B;이므로 없는 경우 symlink를 만드십시오.
1. [Cloud Manager - 웹 계층 구성 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) 또는 [RDE 명령](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration)을 사용하여 원하는 AEM as a Cloud Service 환경에 vhost 변경 사항을 배포합니다.

그러나 웹 브라우저 및 CDN 캐시 수명에 대해 다른 값을 갖기 위해 위의 예에서 `Surrogate-Control` 헤더를 사용할 수 있습니다. 마찬가지로 특정 날짜 및 시간에 캐시를 만료하려면 `Expires` 헤더를 사용할 수 있습니다. 또한 `stale-while-revalidate` 및 `stale-if-error` 특성을 사용하여 응답 콘텐츠의 부실 상태 처리를 제어할 수 있습니다. AEM WKND 프로젝트에 [참조 부실 상태 처리](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN 캐시 구성이 있습니다.

마찬가지로 다른 콘텐츠 유형(JSON, JS, CSS 및 Assets)의 캐시 헤더도 업데이트할 수 있습니다.

### 사용자 지정 Java™ 코드

이 옵션은 AEM 게시와 작성자 모두에서 사용할 수 있습니다. 그러나 AEM Author에서 캐싱을 활성화하고 기본 캐싱 동작을 유지하는 것은 권장되지 않습니다.

캐시 헤더를 업데이트하려면 사용자 지정 Java™ 코드의 `HttpServletResponse` 개체(Sling 서블릿, Sling 서블릿 필터)를 사용합니다. 일반 구문은 다음과 같습니다.

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
