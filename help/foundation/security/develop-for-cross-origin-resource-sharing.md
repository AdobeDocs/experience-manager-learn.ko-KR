---
title: AEM과의 CORS(원본 간 리소스 공유)를 위한 개발
description: CORS를 활용하여 클라이언트측 JavaScript을 통해 외부 웹 애플리케이션에서 AEM 콘텐츠에 액세스하는 간단한 예입니다.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# CORS(원본 간 리소스 공유)를 위한 개발

클라이언트측 JavaScript을 통해 외부 웹 애플리케이션에서 AEM 콘텐츠에 액세스하는 데 [!DNL CORS]을(를) 활용하는 간단한 예입니다. 이 예에서는 CORS OSGi 구성을 사용하여 AEM에서 CORS 액세스를 활성화합니다. OSGi 구성 접근 방식은 다음과 같은 경우에 실행 가능합니다.

* 단일 원본이 AEM 게시 콘텐츠에 액세스하고 있습니다.
* AEM 작성자에게는 CORS 액세스 권한이 필요합니다.

AEM Publish에 대한 다중 원본 액세스가 필요한 경우 [이 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration)를 참조하세요.

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

이 비디오에서:

* **www.example.com**&#x200B;이(가) `/etc/hosts`을(를) 통해 localhost에 매핑됨
* **aem-publish.local** `/etc/hosts`을(를) 통해 localhost에 매핑
* SimpleHTTPServer([[!DNL Python]의 SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)에 대한 래퍼)가 포트 8000을 통해 HTML 페이지를 제공하고 있습니다.
   * _더 이상 Mac App Store에서 사용할 수 없습니다. [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._&#x200B;과(와) 같은 유사한 기능 사용
* [!DNL AEM Dispatcher]이(가) [!DNL Apache HTTP Web Server] 2.4에서 실행 중이고 `aem-publish.local`에 대한 요청을 `localhost:4503`에 역 프록싱합니다.

자세한 내용은 [AEM의 CORS(원본 간 리소스 공유) 이해](./understand-cross-origin-resource-sharing.md)를 검토하세요.

## www.example.com HTML 및 JavaScript

이 웹 페이지에는 다음과 같은 논리가 있습니다

1. 버튼을 클릭하면
1. `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`에 [!DNL AJAX GET] 요청
1. JSON 응답에서 `jcr:title`을(를) 검색합니다.
1. DOM에 `jcr:title` 넣기

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

## Dispatcher 구성 {#dispatcher-configuration}

### CORS 요청 헤더 허용

필요한 [HTTP 요청 헤더가 처리를 위해 AEM으로 통과하도록 허용](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)하려면 Dispatcher의 `/clientheaders` 구성에서 허용되어야 합니다.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS 응답 헤더 캐싱

캐시된 콘텐츠에서 CORS 헤더를 캐시하고 제공할 수 있도록 하려면 다음 [/cache /headers 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#caching-http-response-headers)을(를) AEM 게시 `dispatcher.any` 파일에 추가하십시오.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

`dispatcher.any` 파일을 변경한 후 **웹 서버 응용 프로그램을 다시 시작**&#x200B;합니다.

`/cache /headers` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 수 있습니다.

## 지원 자료 {#supporting-materials}

* macOS에 대한 [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html)&#x200B;(Windows/macOS/Linux 호환)

* [AEM의 CORS(원본 간 리소스 공유) 이해](./understand-cross-origin-resource-sharing.md)
* [원본 간 리소스 공유(W3C)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
