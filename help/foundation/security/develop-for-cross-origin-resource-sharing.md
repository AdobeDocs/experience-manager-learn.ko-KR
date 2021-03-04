---
title: AEM을 통한 CORS(Cross-Origin Resource Sharing) 개발
description: 클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 컨텐츠에 액세스하기 위해 CORS를 활용하는 간단한 예.
version: 6.3, 6,4, 6.5
sub-product: 기반, 컨텐츠 서비스, 사이트
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
topic: 보안
role: 개발자
level: 초급
feature: null
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---


# CORS(Cross-Origin Resource Sharing) 개발

클라이언트측 JavaScript를 통해 외부 웹 응용 프로그램에서 AEM 내용에 액세스하기 위해 [!DNL CORS]을 활용하는 간단한 예입니다.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

이 비디오에서:

* **www.example.** commaps를  `/etc/hosts`
* **aem-publish.** localmaps를  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (SimpleHTTPServer의 래퍼 [[!DNL Python])는 포트 8000을 통해 HTML 페이지를](https://docs.python.org/2/library/simplehttpserver.html) 제공하고 있습니다.
* [!DNL AEM Dispatcher] 가  [!DNL Apache HTTP Web Server] 2.4에서 실행되고 요청이 다음으로 역방향 프록시 처리  `aem-publish.local` 요청을  `localhost:4503`실행합니다.

자세한 내용은 AEM](./understand-cross-origin-resource-sharing.md)에서 [CORS(교차 도메인 리소스 공유) 이해를 참조하십시오.

## www.example.com 및 JavaScript 사용

이 웹 페이지에는

1. 단추를 클릭하면
1. `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`에 [!DNL AJAX GET] 요청을 만듭니다.
1. JSON 응답의 `jcr:title`을 검색합니다.
1. `jcr:title`을(를) DOM에 삽입합니다.

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

## OSGi 팩토리 구성

[!DNL Cross-Origin Resource Sharing]에 대한 OSGi 구성 팩토리는 다음을 통해 사용할 수 있습니다.

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

## 발송자 구성 {#dispatcher-configuration}

캐시된 컨텐츠에서 CORS 헤더의 캐싱 및 제공을 허용하려면 다음 [/clientheaders 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)을 모든 지원 AEM 게시 `dispatcher.any` 파일에 추가합니다.

```
/cache { 
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

**파일을 변경한** 후 웹 서버 응용 프로그램을  `dispatcher.any` 다시 시작합니다.

`/clientheaders` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지우는 것이 필요할 수 있습니다.

## 지원 자료 {#supporting-materials}

* [크로스 원본 리소스 공유 정책을 위한 AEM OSGi 구성 팩토리](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [macOS용 SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (Windows/macOS/Linux 호환)

* [AEM의 CORS(교차 도메인 리소스 공유) 이해](./understand-cross-origin-resource-sharing.md)
* [W3C(Cross-Origin Resource Sharing)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

