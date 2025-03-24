---
title: AEM용 Sling Dynamic Include 설정
description: Apache HTTP 웹 서버에서 실행되는 AEM Dispatcher과 함께 Apache Sling Dynamic Include를 설치하고 사용하는 방법에 대한 비디오 설명입니다.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# [!DNL Sling Dynamic Include] 설정

[!DNL Apache HTTP Web Server]에서 실행 중인 [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR)와 함께 [!DNL Apache Sling Dynamic Include]을(를) 설치하고 사용하는 방법에 대한 비디오 연습입니다.

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 최신 버전의 AEM Dispatcher이 로컬에 설치되어 있는지 확인합니다.

1. [[!DNL Sling Dynamic Include] 번들](https://sling.apache.org/downloads.cgi)을(를) 다운로드하여 설치하십시오.
1. **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;에서 [!DNL OSGi Configuration Factory]을(를) 통해 [!DNL Sling Dynamic Include]을(를) 구성합니다.

   또는 AEM 코드 기반에 추가하려면 다음 위치에 적절한 **sling:OsgiConfig** 노드를 만드십시오.

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

1. (선택 사항) 마지막 단계를 반복하여 [편집 가능한 템플릿](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html)의 잠긴(초기) 콘텐츠에 있는 구성 요소를 [!DNL SDI]을(를) 통해서도 제공할 수 있습니다. 추가 구성의 이유는 편집 가능한 템플릿의 잠긴 콘텐츠가 `/content` 대신 `/conf`에서 제공되기 때문입니다.

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

1. [!DNL Apache HTTPD Web server]의 `httpd.conf` 파일을 업데이트하여 [!DNL Include] 모듈을 사용하도록 설정합니다.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. include 지시문을 준수하도록 [!DNL vhost] 파일을 업데이트하십시오.

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

1. (1) `nocache` 선택기를 지원하고 (2) TTL 지원을 사용하도록 Dispatcher.any 구성 파일을 업데이트합니다.

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
   > 위의 glob `*.nocache.html*` 규칙에서 후행 `*`을(를) 해제하면 [하위 리소스에 대한 요청에 문제가 발생](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)할 수 있습니다.

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 구성 파일 또는 `dispatcher.any`을(를) 변경한 후 항상 [!DNL Apache HTTP Web Server]을(를) 다시 시작하십시오.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>ESI(Edge-side includes) 제공에 [!DNL Sling Dynamic Includes]을(를) 사용하는 경우 Dispatcher 캐시에 관련 [응답 헤더를 캐시해야 합니다](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). 가능한 헤더에는 다음이 포함됩니다.
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;만료&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>

## 지원 자료

* [Sling Dynamic Include 번들 다운로드](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include 설명서](https://github.com/Cognifide/Sling-Dynamic-Include)
