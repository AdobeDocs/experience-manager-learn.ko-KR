---
title: AEM용 Sling Dynamic Include 설정
description: Apache HTTP Web Server에서 실행되는 AEM Dispatcher와 함께 Apache Sling Dynamic Include를 설치하고 사용하는 비디오 개요입니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 5%

---

# 설정 [!DNL Sling Dynamic Include]

설치 및 사용에 대한 비디오 안내 [!DNL Apache Sling Dynamic Include] with [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR) 실행 [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> AEM Dispatcher 최신 버전이 로컬로 설치되었는지 확인합니다.

1. 를 다운로드하여 설치합니다. [[!DNL Sling Dynamic Include] 번들](https://sling.apache.org/downloads.cgi).
1. 구성 [!DNL Sling Dynamic Include] 사용 [!DNL OSGi Configuration Factory] at **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   또는 AEM 코드 베이스에 추가하려면 적절한 **sling:OsgiConfig** 노드 위치:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (선택 사항) 마지막 단계를 반복하여 [편집 가능한 템플릿의 잠금(초기) 콘텐츠](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 을 통해 제공 [!DNL SDI] 또한. 추가 구성의 이유는 편집 가능한 템플릿의 잠긴 콘텐츠가 `/conf` 대신 `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. 업데이트 [!DNL Apache HTTPD Web server]s `httpd.conf` 파일을 사용하여 [!DNL Include] 모듈.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 업데이트 [!DNL vhost] include 지시문을 준수하기 위한 파일입니다.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. dispatcher.any 구성 파일을 업데이트하여 (1) 지원 `nocache` 선택기와 (2) 는 TTL 지원을 활성화합니다.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > 후행 `*` 빗속에서 `*.nocache.html*` 위의 규칙은 결과를 초래할 수 있습니다. [하위 리소스에 대한 요청의 문제](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 항상 다시 시작 [!DNL Apache HTTP Web Server] 구성 파일을 변경한 후 또는 `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>사용 중인 경우 [!DNL Sling Dynamic Includes] esi(Edge Side Includes)의 경우 관련 정보를 캐시하십시오 [dispatcher 캐시의 응답 헤더](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). 가능한 헤더에는 다음이 포함됩니다.
>
>* &quot;Cache-Control&quot;
>* &quot;컨텐츠 처리&quot;
>* &quot;Content-Type&quot;
>* &quot;만료&quot;
>* &quot;마지막 수정 날짜&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;마지막 수정 날짜&quot;
>


## 지원 자료

* [Sling Dynamic Include 번들 다운로드](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include 설명서](https://github.com/Cognifide/Sling-Dynamic-Include)
