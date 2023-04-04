---
title: AEM을 통한 CORS(원본 간 리소스 공유) 개발
description: 클라이언트 측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 컨텐츠에 액세스하는 CORS를 활용하는 짧은 예입니다.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# CORS(원본 간 리소스 공유)를 위한 개발

활용 사례 [!DNL CORS] 클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 컨텐츠에 액세스할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

이 비디오에서는:

* **www.example.com** 를 통해 localhost에 매핑 `/etc/hosts`
* **aem-publish.local** 를 통해 localhost에 매핑 `/etc/hosts`
* SimpleHTTPServer(용 래퍼) [[!DNL Python]의 SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))이 포트 8000을 통해 HTML 페이지를 제공하고 있습니다.
   * _Mac App Store에서 더 이상 사용할 수 없습니다. 다음과 유사한 사용 [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 실행 중입니다. [!DNL Apache HTTP Web Server] 2.4 및 역방향 프록싱 요청 `aem-publish.local` to `localhost:4503`.

자세한 내용은 [AEM의 CORS(원본 간 리소스 공유) 이해](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML 및 JavaScript

이 웹 페이지에는

1. 버튼을 클릭하면
1. 만들기 [!DNL AJAX GET] 요청 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. 를 검색합니다. `jcr:title` JSON 응답 양식
1. 를 삽입합니다. `jcr:title` DOM으로

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

에 대한 OSGi 구성 팩토리 [!DNL Cross-Origin Resource Sharing] 는 다음을 통해 사용할 수 있습니다.

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

## Dispatcher 구성 {#dispatcher-configuration}

캐시된 컨텐츠에서 CORS 헤더의 캐싱 및 서비스를 허용하려면 다음을 추가합니다 [/clientheaders 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) 모든 지원 AEM Publish `dispatcher.any` 파일.

```
/myfarm { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**웹 서버 응용 프로그램을 다시 시작합니다** 변경한 후 `dispatcher.any` 파일.

따라서 다음 요청 이후에 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지우는 것이 필요할 수 있습니다. `/clientheaders` 구성 업데이트.

## 지원 자료 {#supporting-materials}

* [macOS의 제베](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (Windows/macOS/Linux 호환 가능)

* [AEM의 CORS(원본 간 리소스 공유) 이해](./understand-cross-origin-resource-sharing.md)
* [교차 도메인 리소스 공유(W3C)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
