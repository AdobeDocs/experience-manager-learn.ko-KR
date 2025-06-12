---
title: AEM을 사용하여 원본 간 리소스 공유(CORS) 개발
description: 클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 콘텐츠에 액세스하기 위해 CORS를 활용하는 간단한 예시입니다.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '318'
ht-degree: 100%

---

# 원본 간 리소스 공유(CORS) 개발

클라이언트측 JavaScript를 통해 외부 웹 애플리케이션에서 AEM 콘텐츠에 액세스하기 위해 [!DNL CORS]를 활용하는 간단한 예시입니다. 이 예시에서는 CORS OSGi 구성을 사용하여 AEM에서 CORS 액세스를 활성화합니다. 다음과 같은 경우 OSGi 구성 방식이 적합합니다.

* 단일 출처에서 AEM 게시 콘텐츠에 액세스하는 경우
* AEM 작성자 인스턴스에서 CORS 액세스가 필요한 경우

AEM 게시 인스턴스에 대한 다중 출처 액세스가 필요한 경우에는 [이 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ko#dispatcher-configuration)를 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/33348?quality=12&learn=on&captions=kor)

이 비디오에서는 다음 내용을 다룹니다.

* **www.example.com**&#x200B;은 `/etc/hosts`를 통해 localhost로 매핑되어 있습니다.
* **aem-publish.local**&#x200B;은 `/etc/hosts`를 통해 localhost로 매핑되어 있습니다.
* SimpleHTTPServer([[!DNL Python] SimpleHTTPServer의 래퍼](https://docs.python.org/2/library/simplehttpserver.html))는 포트 8000을 통해 HTML 페이지를 제공합니다.
   * _Mac App Store에서는 더 이상 제공되지 않습니다. [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)와 같은 유사한 도구를 사용하십시오._
* [!DNL AEM Dispatcher]는 [!DNL Apache HTTP Web Server] 2.4에서 실행 중이며, `aem-publish.local`로 오는 요청을 `localhost:4503`으로 리버스 프록시 처리합니다.

자세한 내용은 [AEM의 원본 간 리소스 공유(CORS) 이해](./understand-cross-origin-resource-sharing.md)를 검토하십시오.

## www.example.com HTML 및 JavaScript

이 웹 페이지에는 다음과 같은 논리가 포함되어 있습니다.

1. 버튼 클릭 시
1. `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`으로 [!DNL AJAX GET] 요청 전송
1. JSON 응답에서 `jcr:title` 추출
1. DOM에 `jcr:title` 주입

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

[!DNL Cross-Origin Resource Sharing]을 위한 OSGi 구성 팩토리는 다음 경로에서 확인할 수 있습니다.

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

필요한 [HTTP 요청 헤더가 AEM으로 전달되어 처리될 수 있도록 하려면](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#specifying-the-http-headers-to-pass-through-clientheaders), Disaptcher의 `/clientheaders` 구성에서 해당 헤더를 허용해야 합니다.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS 응답 헤더 캐싱

캐시된 콘텐츠에 CORS 헤더의 캐싱 및 제공을 허용하려면 다음 [/cache /headers 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#caching-http-response-headers)을 AEM 게시 `dispatcher.any` 파일에 추가합니다.

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

`dispatcher.any` 파일을 변경한 후 **웹 서버 애플리케이션을 다시 시작**&#x200B;합니다.

`/cache /headers` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 수도 있습니다.

## 지원 자료 {#supporting-materials}

* [macOS용 Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html)&#x200B;(Windows/macOS/Linux 호환)

* [AEM의 원본 간 리소스 공유(CORS) 이해](./understand-cross-origin-resource-sharing.md)
* [W3C(원본 간 리소스 공유)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
