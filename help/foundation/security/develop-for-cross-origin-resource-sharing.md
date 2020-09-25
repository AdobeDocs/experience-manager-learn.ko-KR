---
title: AEM에서 CORS(Cross-Origin Resource Sharing) 개발
description: 클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 컨텐츠에 액세스하기 위해 CORS를 활용하는 간단한 예입니다.
version: 6.3, 6,4, 6.5
sub-product: foundation, content services, sites
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# CORS(Cross-Origin Resource Sharing) 개발

클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 컨텐츠 [!DNL CORS] 에 액세스하는 방법을 소개하는 짧은 예입니다.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

이 비디오에서:

* **www.example.com을** 통해 localhost에 매핑 `/etc/hosts`
* **aem-publish.local** maps to localhost via `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) ( [[!DNL Python]&#39;s SimpleHTTPServer의 래퍼](https://docs.python.org/2/library/simplehttpserver.html))는 포트 8000을 통해 HTML 페이지를 제공하고 있습니다.
* [!DNL AEM Dispatcher] 가 [!DNL Apache HTTP Web Server] 2.4에서 실행되고 요청이 다음으로 역방향 프록시 처리 `aem-publish.local` 를 `localhost:4503`실행합니다.

자세한 내용은 AEM [에서 CORS(Cross-Origin Resource Sharing) 이해를 참조하십시오](./understand-cross-origin-resource-sharing.md).

## www.example.com 및 JavaScript 사용

이 웹 페이지에는

1. 단추 클릭
1. 다음에 [!DNL AJAX GET] 요청 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. JSON 응답의 `jcr:title` 양식을 검색합니다.
1. DOM `jcr:title` 에

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGi 공장 구성

에 대한 OSGi 구성 팩터리는 다음을 통해 [!DNL Cross-Origin Resource Sharing] 사용할 수 있습니다.

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher configuration {#dispatcher-configuration}

캐시된 컨텐츠에서 헤더의 캐싱 및 [!DNL CORS] 제공을 허용하려면 다음 구성을 모든 지원 AEM 게시 `dispatcher.any` 파일에 추가합니다.

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**파일을 변경한 후 웹 서버 응용** 프로그램을 `dispatcher.any` 다시 시작합니다.

구성 업데이트 후 헤더가 다음 요청에 적절하게 캐시되도록 하기 위해 캐시를 완전히 지우는 것이 `/headers` 좋습니다.

## 지원 자료 {#supporting-materials}

* [AEM OSGi Configuration factory for Cross-Origin Resource Sharing Policies](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [macOS용 SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (Windows/macOS/Linux 호환)

* [AEM의 CORS(Cross-Origin Resource Sharing) 이해](./understand-cross-origin-resource-sharing.md)
* [W3C(Cross-Origin Resource Sharing)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

