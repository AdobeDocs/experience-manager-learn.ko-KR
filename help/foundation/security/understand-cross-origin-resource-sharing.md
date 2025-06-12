---
title: AEM을 사용한 원본 간 리소스 공유(CORS) 이해
description: Adobe Experience Manager의 원본 간 리소스 공유(CORS)를 사용하면 AEM이 아닌 웹 속성에서도 인증 여부와 관계없이 클라이언트측에서 AEM에 호출하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# 원본 간 리소스 공유([!DNL CORS]) 이해

Adobe Experience Manager의 원본 간 리소스 공유([!DNL CORS])를 사용하면 AEM이 아닌 웹 속성에서도 인증 여부와 관계없이 클라이언트측에서 AEM에 호출하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.

이 문서에 설명된 OSGI 구성은 다음에 적합합니다.

1. AEM 게시 인스턴스에서의 단일 출처 리소스 공유
2. AEM 작성자 인스턴스에 대한 CORS 액세스

AEM 게시 인스턴스에서 다중 출처 CORS 액세스가 필요한 경우 [이 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ko#dispatcher-configuration)를 참조하십시오.

## Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성

CORS 구성은 AEM의 OSGi 구성 팩토리로 관리되며, 각 정책은 팩토리의 한 인스턴스로 표현됩니다.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy]&#x200B;(`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 정책 선택

정책은 다음을 비교하여 선택됩니다.

* `Origin` 요청 헤더와 `Allowed Origin`
* 요청 경로와 `Allowed Paths`

이들 값과 일치하는 첫 번째 정책이 사용됩니다. 일치하는 정책이 없으면 모든 [!DNL CORS] 요청이 거부됩니다.

정책이 전혀 구성되지 않은 경우에는 핸들러가 비활성화되어 있어 [!DNL CORS] 요청에 응답하지 않으므로, 서버의 다른 모듈이 [!DNL CORS]에 응답하지 않는 한 모든 CORS 요청이 사실상 거부됩니다.

### 정책 속성

#### [!UICONTROL 허용된 출처]

* `"alloworigin" <origin> | *`
* 리소스에 액세스할 수 있는 URI를 지정하는 `origin` 매개변수 목록입니다. 자격 증명이 없는 요청의 경우 서버는 &#42;를 와일드카드로 지정하여 모든 출처가 리소스에 액세스하도록 허용할 수 있습니다. *그러나 `Allow-Origin: *`는 브라우저가 CORS가 없으면 기본적으로 엄격히 금지하는 요청을 모든 외부(즉, 공격자) 웹 사이트가 수행할 수 있게 하므로, 운영 환경에서는 절대 권장되지 않습니다.*

#### [!UICONTROL 허용된 출처(정규 표현식)]

* `"alloworiginregexp" <regexp>`
* 리소스에 액세스할 수 있는 URI를 지정하는 `regexp` 정규 표현식 목록입니다. *정규 표현식을 신중하게 작성하지 않으면 의도치 않은 일치가 발생할 수 있으며, 이에 따라 공격자가 정책과 일치하는 사용자 정의 도메인 이름을 사용할 수 있습니다.* 다른 정책 속성을 반복적으로 구성해야 하더라도, 일반적으로 `alloworigin`을 사용하여 각각의 특정 출처 호스트 이름에 대해 별도의 정책을 설정하는 것이 좋습니다. 출처마다 수명 주기와 요구 사항이 다르기 때문에 명확하게 구분된 정책을 적용하는 것이 바람직합니다.

#### [!UICONTROL 허용되는 경로]

* `"allowedpaths" <regexp>`
* 정책이 적용되는 리소스 경로를 지정하는 `regexp` 정규 표현식 목록입니다.

#### [!UICONTROL 노출된 헤더]

* `"exposedheaders" <header>`
* 브라우저가 액세스할 수 있는 응답 헤더를 나타내는 헤더 매개변수 목록입니다. CORS 요청(프리플라이트 제외)의 경우 비어 있지 않으면 이들 값이 `Access-Control-Expose-Headers` 응답 헤더에 복사됩니다. 그러면 목록에 포함된 값(헤더 이름)은 브라우저에서 액세스할 수 있게 됩니다. 이 설정이 없으면 해당 헤더는 브라우저에서 읽을 수 없습니다.

#### [!UICONTROL 최대 기간]

* `"maxage" <seconds>`
* 프리플라이트 결과를 캐시할 수 있는 기간을 나타내는 `seconds` 매개변수입니다.

#### [!UICONTROL 지원되는 헤더]

* `"supportedheaders" <header>`
* 실제 요청을 수행할 때 사용할 수 있는 HTTP 요청 헤더를 나타내는 `header` 매개변수 목록입니다.

#### [!UICONTROL 허용된 메서드]

* `"supportedmethods"`
* 실제 요청을 수행할 때 사용할 수 있는 HTTP 메서드를 나타내는 메서드 매개변수 목록입니다.

#### [!UICONTROL 자격 증명 지원]

* `"supportscredentials" <boolean>`
* 요청에 대한 응답을 브라우저에 노출할 수 있는지 여부를 나타내는 `boolean`입니다. 프리플라이트 요청에 대한 응답의 일부로 사용되는 경우, 실제 요청이 인증 정보를 포함하여 수행될 수 있는지를 나타냅니다.

### 예제 구성

사이트 1은 익명으로 액세스 가능한 읽기 전용 시나리오로, [!DNL GET] 요청을 통해 콘텐츠를 사용합니다.

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ]
}
```

사이트 2는 보다 복잡하며, 인증된 요청과 변경(POST, PUT, DELETE) 요청이 필요합니다.

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher 캐싱 문제 및 구성 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1 이상 버전부터는 응답 헤더도 캐시할 수 있습니다. 이를 통해 요청이 익명일 경우, [!DNL CORS] 요청 리소스와 함께 [!DNL CORS] 헤더도 캐시할 수 있게 됩니다.

일반적으로 Dispatcher에서 콘텐츠를 캐싱하는 것과 동일한 고려 사항이 CORS 응답 헤더를 Dispatcher에서 캐싱하는 데에도 적용됩니다. 다음 표는 [!DNL CORS] 헤더(즉, [!DNL CORS] 요청)를 캐시할 수 있는 시기를 정의합니다.

| 캐시 가능 여부 | 환경 | 인증 상태 | 설명 |
|-----------|-------------|-----------------------|-------------|
| 아니요 | AEM 게시 인스턴스 | 인증됨 | AEM 작성자 인스턴스의 Dispatcher 캐싱은 정적이고 작성되지 않은 자산으로 제한됩니다. 이로 인해 HTTP 응답 헤더를 포함하여 AEM 작성자 인스턴스에서는 대부분의 리소스를 캐시하기가 어렵고 비효율적입니다. |
| 아니요 | AEM 게시 인스턴스 | 인증됨 | 인증된 요청에 대해 CORS 헤더를 캐시하지 않아야 합니다. 이는 인증된 요청을 캐시하지 말라는 일반적인 지침과 일치하며, 요청자의 인증/권한 상태에 따라 제공되는 리소스가 달라질 수 있기 때문에 이를 판단하기 어렵습니다. |
| 예 | AEM 게시 인스턴스 | 익명 | Dispatcher에서 캐시 가능한 익명 요청은 응답 헤더도 함께 캐시할 수 있어, 이후 CORS 요청이 캐시된 콘텐츠에 액세스할 수 있습니다. AEM 게시 인스턴스에서 CORS 구성이 변경되면 해당 캐시된 리소스를 **무효화해야 합니다**. 모범 사례에서는 코드 또는 구성 배포 시 Dispatcher 캐시를 삭제하도록 규정하고 있는데, 어떤 캐시된 콘텐츠가 영향을 받는지 판단하기 어렵기 때문입니다. |

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

`dispatcher.any` 파일을 변경한 후에는 **웹 서버 애플리케이션을 다시 시작**&#x200B;해야 합니다.

`/cache/headers` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 수도 있습니다.

## CORS 문제 해결

로깅은 `com.adobe.granite.cors` 아래에서 설정할 수 있습니다.

* `DEBUG`를 활성화하면 [!DNL CORS] 요청이 거부된 이유에 대한 상세 정보를 확인할 수 있습니다.
* `TRACE`를 활성화하면 CORS 핸들러를 통과하는 모든 요청에 대한 상세 정보를 확인할 수 있습니다.

### 팁:

* curl을 사용하여 XHR 요청을 수동으로 재현하되, 각 헤더와 세부 정보를 모두 복사해야 합니다. 각 요소가 결과에 영향을 줄 수 있으므로, 일부 브라우저 콘솔에서는 curl 명령을 복사하는 기능을 제공합니다.
* 요청이 인증, CSRF 토큰 필터, Dispatcher 필터 또는 다른 보안 계층이 아니라 CORS 핸들러에 의해 거부되었는지 확인하십시오.
   * CORS 핸들러가 200 응답을 반환했음에도 불구하고 응답에 `Access-Control-Allow-Origin` 헤더가 없다면 `com.adobe.granite.cors`의 [!DNL DEBUG]에서 거부 사유를 검토하십시오.
* Dispatcher에서 [!DNL CORS] 요청의 캐싱이 활성화된 경우
   * `/cache/headers` 구성이 `dispatcher.any`에 적용되었고 웹 서버가 성공적으로 다시 시작되었는지 확인하십시오.
   * OSGi 또는 dispatcher.any 구성이 변경된 후에는 캐시가 제대로 지워졌는지 확인하십시오.
* 필요한 경우 요청에 인증 자격 증명이 있는지 확인하십시오.

## 지원 자료

* [원본 간 리소스 공유 정책 정책을 위한 AEM OSGi 구성 팩토리](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [W3C(원본 간 리소스 공유)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
