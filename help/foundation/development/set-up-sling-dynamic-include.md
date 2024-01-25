---
title: AEM용 Sling Dynamic Include 설정
description: Apache HTTP 웹 서버에서 실행되는 AEM Dispatcher와 함께 Apache Sling Dynamic Include를 설치하고 사용하는 비디오 설명입니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 874
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 설정 [!DNL Sling Dynamic Include]

설치 및 사용에 대한 비디오 둘러보기 [!DNL Apache Sling Dynamic Include] 포함 [AEM 디스패처](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) 실행 중 [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 최신 버전의 AEM Dispatcher가 로컬에 설치되어 있는지 확인합니다.

1. 다운로드 및 설치 [[!DNL Sling Dynamic Include] 번들](https://sling.apache.org/downloads.cgi).
1. 구성 [!DNL Sling Dynamic Include] 를 통해 [!DNL OSGi Configuration Factory] 위치: **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   또는 AEM 코드 기반에 추가하려면 적절한 를 만듭니다 **sling:OsgiConfig** 노드 위치:

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

1. (선택 사항) 마지막 단계를 반복하여 의 구성 요소를 허용합니다. [편집 가능한 템플릿의 잠긴(초기) 콘텐츠](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 다음을 통해 제공: [!DNL SDI] 또한. 추가 구성의 이유는 편집 가능한 템플릿의 잠긴 컨텐츠가에서 제공되기 때문입니다. `/conf` 대신 `/content`.

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

1. 업데이트 [!DNL Apache HTTPD Web server]의 `httpd.conf` 파일을 활성화하여 [!DNL Include] 모듈.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 업데이트 [!DNL vhost] 적용할 파일에는 지시문이 포함됩니다.

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

1. (1)을 지원하도록 dispatcher.any 구성 파일 업데이트 `nocache` 선택기 및 (2) TTL 지원을 활성화합니다.

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
   > 후행 남기기 `*` 오프인 더 glob `*.nocache.html*` 위의 규칙으로 인해 다음이 발생할 수 있습니다. [하위 리소스 요청 문제](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 항상 다시 시작 [!DNL Apache HTTP Web Server] 구성 파일 또는 을 변경한 후 `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>을 사용하는 경우 [!DNL Sling Dynamic Includes] ESI(Edge-side includes) 서비스를 제공하는 경우 적절한 캐시를 사용해야 합니다. [dispatcher 캐시의 응답 헤더](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). 가능한 헤더에는 다음이 포함됩니다.
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
