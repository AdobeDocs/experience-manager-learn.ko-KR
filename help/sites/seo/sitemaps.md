---
title: 사이트맵
description: AEM Sites용 사이트 맵을 만들어 SEO를 향상시키는 방법을 알아봅니다.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 957
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 3%

---

# 사이트맵

AEM Sites용 사이트 맵을 만들어 SEO를 향상시키는 방법을 알아봅니다.

>[!WARNING]
>
>이 비디오에서는 사이트 맵에서 상대 URL의 사용을 보여 줍니다. 사이트맵 [절대 URL을 사용해야 함](https://sitemaps.org/protocol.html). 다음을 참조하십시오 [구성](#absolute-sitemap-urls) 절대 URL을 활성화하는 방법에 대한 자세한 내용은 아래 비디오에서 다루지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## 구성

### 절대 사이트 맵 URL{#absolute-sitemap-urls}

AEM 사이트 맵은 다음을 사용하여 절대 URL을 지원합니다. [Sling 매핑](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). 이 작업은 사이트 맵(일반적으로 AEM Publish 서비스)을 생성하는 AEM 서비스에 매핑 노드를 생성하여 수행됩니다.

에 대한 Sling 매핑 노드 정의의 예 `https://wknd.com` 다음을 정의할 수 있습니다. `/etc/map/https` 다음과 같이:

| 경로 | 속성 이름 | 속성 유형 | 속성 값 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 문자열 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 문자열 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 문자열 | `wknd.com/$1` |

아래 스크린샷은 유사한 구성을 보여 주지만 `http://wknd.local` (에서 실행 중인 로컬 호스트 이름 매핑 `http`).

![사이트 맵 절대 URL 구성](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### 사이트 맵 스케줄러 OSGi 구성

다음을 정의합니다 [OSGi 공장 구성](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 빈도용(사용) [cron 표현식](http://www.cronmaker.com/)) 사이트 맵은 AEM에서 다시 생성/생성되고 캐시됩니다.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher 허용 필터 규칙

사이트 맵 인덱스 및 사이트 맵 파일에 대한 HTTP 요청을 허용합니다.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache 웹 서버 재작성 규칙

확인 `.xml` 사이트 맵 HTTP 요청은 올바른 기본 AEM 페이지로 라우팅됩니다. URL 단축법을 사용하지 않거나 Sling 매핑을 사용하여 URL 단축법을 사용하는 경우 이 구성은 필요하지 않습니다.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## 리소스

+ [AEM 사이트 맵 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Apache Sling 사이트 맵 설명서](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org 사이트 맵 설명서](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org 사이트 맵 인덱스 파일 설명서](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)