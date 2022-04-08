---
title: Sitemap
description: AEM Sites용 사이트 맵을 만들어 SEO를 높이는 방법을 알아봅니다.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Sitemap

AEM Sites용 사이트 맵을 만들어 SEO를 높이는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 리소스

+ [AEM Sitemap 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap 설명서](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap 설명서](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap 인덱스 파일 설명서](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 구성

### Sitemap 스케줄러 OSGi 구성

을(를) 정의합니다 [OSGi 공장 구성](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 빈도(사용 [cron 표현식](http://www.cronmaker.com)) 사이트 맵이 다시 생성되고 AEM에서 캐시됩니다.

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

확인 `.xml` sitemap HTTP 요청은 올바른 기본 AEM 페이지로 라우팅됩니다. URL 단축을 사용하지 않거나 Sling 매핑을 사용하여 URL 단축을 달성하는 경우 이 구성이 필요하지 않습니다.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
