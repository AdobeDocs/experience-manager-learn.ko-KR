---
title: AEM용 Sling Dynamic Include 설정
description: Apache HTTP Web Server에서 실행되는 AEM Dispatcher와 함께 Apache Sling Dynamic Include를 설치하고 사용하는 비디오 단계입니다.
version: 6.3, 6.4, 6.5
sub-product: foundation, sites
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# [!DNL Sling Dynamic Include] 설정

](https://docs.adobe.com/content/help/ko-KR/experience-manager-dispatcher/using/dispatcher.html)에서 실행 중인 [!DNL Apache Sling Dynamic Include]AEM Dispatcher[을(를) 설치하고 사용하는 비디오 스루입니다.[!DNL Apache HTTP Web Server]

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 최신 버전의 AEM Dispatcher가 로컬에 설치되어 있는지 확인합니다.

1. [[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi)을 다운로드하여 설치합니다.
1. [!DNL Sling Dynamic Include]http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration **의 [!DNL OSGi Configuration Factory]을(를) 통해 구성합니다.**

   또는 AEM 코드 베이스에 추가하려면 다음 위치에 적절한 **sling:OsgiConfig** 노드를 만드십시오.

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

1. (선택 사항) 편집 가능한 템플릿](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html)의 [잠긴(초기) 컨텐츠에 있는 구성 요소가 [!DNL SDI]를 통해 제공될 수 있도록 하려면 마지막 단계를 반복합니다. 편집 가능한 템플릿의 잠긴 콘텐츠가 `/content` 대신 `/conf`에서 제공되기 때문에 추가 구성이 가능합니다.

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

1. [!DNL Apache HTTPD Web server]의 `httpd.conf` 파일을 업데이트하여 [!DNL Include] 모듈을 활성화합니다.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. [!DNL vhost] 파일을 업데이트하여 include 지시어를 준수합니다.

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

1. dispatcher.any 구성 파일을 업데이트하여 (1) `nocache` 선택기 및 (2) TTL 지원을 활성화합니다.

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
   > 위의 글로벌 `*.nocache.html*` 규칙에서 후행 `*`을 종료하면 하위 리소스](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)에 대한 요청에 [문제가 발생할 수 있습니다.

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 구성 파일 또는 `dispatcher.any`을 변경한 후에는 항상 [!DNL Apache HTTP Web Server]을 다시 시작하십시오.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Edge-side includes(ESI)를 제공하는 데 [!DNL Sling Dynamic Includes]을(를) 사용하는 경우 디스패처 캐시](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)에 관련 [응답 헤더를 캐시해야 합니다. 가능한 헤더에는 다음이 포함됩니다.
>
>* &quot;Cache-Control&quot;
>* &quot;컨텐트-처리&quot;
>* &quot;Content-Type&quot;
>* &quot;만료&quot;
>* &quot;최종 수정일&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;최종 수정일&quot;

>



## 지원 자료

* [Sling Dynamic Include 번들 다운로드](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include 설명서](https://github.com/Cognifide/Sling-Dynamic-Include)
