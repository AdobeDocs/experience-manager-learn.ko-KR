---
title: AEM과의 CORS(원본 간 리소스 공유) 이해
description: Adobe Experience Manager의 CORS(원본 간 리소스 공유)는 비 AEM 웹 속성을 사용하여 인증되거나 인증되지 않은 AEM에 대한 클라이언트측 호출을 수행하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있도록 합니다.
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
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 1%

---

# 원본 간 리소스 공유 이해([!DNL CORS])

Adobe Experience Manager의 원본 간 리소스 공유([!DNL CORS])를 사용하면 인증되거나 인증되지 않은 클라이언트측 AEM을 호출하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있는 AEM 이외 웹 속성을 사용할 수 있습니다.

이 문서에 설명된 OSGI 구성은 다음 작업에 충분합니다.

1. AEM 게시의 단일 원본 리소스 공유
2. AEM 작성자에 대한 CORS 액세스

AEM 게시에서 다중 원본 CORS 액세스가 필요한 경우 [이 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ko#dispatcher-configuration)를 참조하세요.

## Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성

CORS 구성은 AEM에서 OSGi 구성 팩토리로 관리되며 각 정책은 팩토리의 하나의 인스턴스로 표시됩니다.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy]&#x200B;(`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 정책 선택

정책을 선택하려면

* `Origin` 요청 헤더가 있는 `Allowed Origin`
* 및 `Allowed Paths`이(가) 요청 경로에 있습니다.

이 값과 일치하는 첫 번째 정책이 사용됩니다. 찾을 수 없으면 [!DNL CORS] 요청이 거부됩니다.

정책이 전혀 구성되어 있지 않으면 처리기가 비활성화되어 있으므로 서버의 다른 모듈이 [!DNL CORS]에 응답하지 않는 한 [!DNL CORS] 요청도 응답하지 않습니다.

### 정책 속성

#### [!UICONTROL 허용된 원본]

* `"alloworigin" <origin> | *`
* 리소스에 액세스할 수 있는 URI를 지정하는 `origin` 매개 변수 목록입니다. 자격 증명이 없는 요청의 경우 서버에서 &#42;을(를) 와일드카드로 지정할 수 있으므로 모든 원본이 리소스에 액세스할 수 있습니다. *모든 외부(예: 공격자) 웹 사이트에서 CORS가 없는 웹 사이트는 브라우저에서 엄격히 금지되어 있다는 요청을 할 수 있으므로 프로덕션에서 `Allow-Origin: *`을(를) 사용하지 않는 것이 좋습니다.*

#### [!UICONTROL 허용된 원본(Regexp)]

* `"alloworiginregexp" <regexp>`
* 리소스에 액세스할 수 있는 URI를 지정하는 `regexp` 정규식 목록입니다. *정규 표현식을 신중하게 작성하지 않으면 의도하지 않은 일치 항목이 발생할 수 있으므로 공격자가 정책과 일치하는 사용자 지정 도메인 이름을 사용할 수 있습니다.* 일반적으로 `alloworigin`을(를) 사용하여 각 특정 원본 호스트 이름에 대해 별도의 정책을 사용하는 것이 좋습니다. 다른 정책 속성이 반복적으로 구성되더라도 마찬가지입니다. 서로 다른 기원은 서로 다른 수명 주기와 요구 사항을 갖는 경향이 있으므로 명확한 분리로부터 이익을 얻을 수 있다.

#### [!UICONTROL 허용된 경로]

* `"allowedpaths" <regexp>`
* 정책이 적용되는 리소스 경로를 지정하는 `regexp` 정규식 목록입니다.

#### [!UICONTROL 노출된 머리글]

* `"exposedheaders" <header>`
* 브라우저가 액세스할 수 있는 응답 헤더를 나타내는 헤더 매개 변수 목록입니다. CORS 요청(프리플라이트 아님)의 경우 비워 두지 않으면 이러한 값이 `Access-Control-Expose-Headers` 응답 헤더에 복사됩니다. 그러면 목록의 값(헤더 이름)이 브라우저에 액세스할 수 있게 됩니다. 이 값이 없으면 브라우저에서 해당 헤더를 읽을 수 없습니다.

#### [!UICONTROL 최대 나이]

* `"maxage" <seconds>`
* 사전 요청 결과를 캐시할 수 있는 시간을 나타내는 `seconds` 매개 변수입니다.

#### [!UICONTROL 지원되는 헤더]

* `"supportedheaders" <header>`
* 실제 요청을 수행할 때 사용할 수 있는 HTTP 요청 헤더를 나타내는 `header` 매개 변수 목록입니다.

#### [!UICONTROL 허용된 메서드]

* `"supportedmethods"`
* 실제 요청을 수행할 때 사용할 수 있는 HTTP 메서드를 나타내는 메서드 매개 변수 목록입니다.

#### [!UICONTROL 자격 증명 지원]

* `"supportscredentials" <boolean>`
* 요청에 대한 응답을 브라우저에 노출할 수 있는지 여부를 나타내는 `boolean`. 프리플라이트 요청에 대한 응답의 일부로 사용되는 경우 자격 증명을 사용하여 실제 요청을 수행할 수 있는지 여부를 나타냅니다.

### 예제 구성

사이트 1은 [!DNL GET]개의 요청을 통해 콘텐츠를 사용하는 익명으로 액세스할 수 있는 기본 읽기 전용 시나리오입니다.

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

사이트 2는 보다 복잡하며 승인 및 변경(POST, PUT, DELETE) 요청이 필요합니다.

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

## Dispatcher 캐싱 관련 사항 및 구성 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1 이상 응답 헤더를 캐시할 수 있습니다. 이렇게 하면 요청이 익명인 한 [!DNL CORS]이(가) 요청한 리소스와 함께 [!DNL CORS] 헤더를 캐시할 수 있습니다.

일반적으로 Dispatcher에서 콘텐츠를 캐시하기 위한 동일한 고려 사항을 Dispatcher에서 CORS 응답 헤더를 캐시하는 데 적용할 수 있습니다. 다음 표는 [!DNL CORS]개의 헤더(및 [!DNL CORS]개의 요청)를 캐시할 수 있는 시기를 정의합니다.

| 캐시 가능 | 환경 | 인증 상태 | 설명 |
|-----------|-------------|-----------------------|-------------|
| 아니요 | AEM 게시 | 인증됨 | AEM 작성자의 Dispatcher 캐싱은 작성되지 않은 정적 자산으로 제한됩니다. 따라서 HTTP 응답 헤더를 포함하여 AEM Author에 대부분의 리소스를 캐시하는 것이 어렵고 비현실적입니다. |
| 아니요 | AEM 게시 | 인증됨 | 인증된 요청에 대해 CORS 헤더를 캐시하지 마십시오. 이는 요청하는 사용자의 인증/권한 부여 상태가 전달된 리소스에 미치는 영향을 판단하기 어렵기 때문에 인증된 요청을 캐시하지 않는다는 일반적인 지침을 따릅니다. |
| 예 | AEM 게시 | 익명 | Dispatcher에서 캐시 가능한 익명 요청은 응답 헤더도 캐시할 수 있으므로 향후 CORS 요청이 캐시된 콘텐츠에 액세스할 수 있습니다. AEM 게시 **must**&#x200B;에서 CORS 구성을 변경한 후에는 영향을 받는 캐시된 리소스가 무효화되어야 합니다. 우수 사례에서는 영향을 줄 수 있는 캐시된 콘텐츠를 확인하기 어렵기 때문에 Dispatcher 캐시가 삭제되는 코드 또는 구성 배포에 대해 설명합니다. |

### CORS 요청 헤더 허용

필요한 [HTTP 요청 헤더가 처리를 위해 AEM으로 통과하도록 허용](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#specifying-the-http-headers-to-pass-through-clientheaders)하려면 Dispatcher의 `/clientheaders` 구성에서 허용되어야 합니다.

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

`dispatcher.any` 파일을 변경한 후 **웹 서버 응용 프로그램을 다시 시작**&#x200B;해야 합니다.

`/cache/headers` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 수 있습니다.

## CORS 문제 해결

로깅을 `com.adobe.granite.cors`에서 사용할 수 있습니다.

* [!DNL CORS] 요청이 거부된 이유에 대한 세부 정보를 보려면 `DEBUG`을(를) 사용하도록 설정하십시오.
* `TRACE`을(를) 사용하면 CORS 처리기를 통해 전송되는 모든 요청에 대한 세부 정보를 볼 수 있습니다.

### 팁:

* curl을 사용하여 XHR 요청을 수동으로 다시 생성하지만, 각 헤더 및 세부 정보가 다를 수 있으므로 모든 헤더와 세부 정보를 복사해야 합니다. 일부 브라우저 콘솔에서는 curl 명령을 복사할 수 있습니다.
* 인증, CSRF 토큰 필터, Dispatcher 필터 또는 기타 보안 레이어가 아닌 CORS 핸들러에서 요청이 거부되었는지 확인합니다.
   * CORS 처리기가 200으로 응답하지만 응답에 `Access-Control-Allow-Origin` 헤더가 없는 경우 `com.adobe.granite.cors`의 [!DNL DEBUG]에서 거부 로그를 검토하십시오.
* [!DNL CORS]개 요청의 디스패처 캐싱이 활성화된 경우
   * `/cache/headers` 구성이 `dispatcher.any`에 적용되었고 웹 서버가 성공적으로 다시 시작되었는지 확인하십시오.
   * OSGi 또는 dispatcher.any 구성을 변경한 후 캐시가 제대로 지워졌는지 확인하십시오.
* 필요한 경우 요청에 대한 인증 자격 증명이 있는지 확인합니다.

## 지원 자료

* [원본 간 리소스 공유 정책을 위한 AEM OSGi 구성 팩토리](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [원본 간 리소스 공유(W3C)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
