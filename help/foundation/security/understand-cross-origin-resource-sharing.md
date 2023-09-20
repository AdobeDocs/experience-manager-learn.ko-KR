---
title: AEM과의 CORS(원본 간 리소스 공유) 이해
description: Adobe Experience Manager의 CORS(원본 간 리소스 공유)를 사용하면 비 AEM 웹 속성을 사용하여 인증되거나 인증되지 않은 클라이언트측에서 AEM을 호출하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: f47beff14782bb3f570d32818b000fc279394f19
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 2%

---

# 원본 간 리소스 공유 이해([!DNL CORS])

Adobe Experience Manager의 원본 간 리소스 공유([!DNL CORS])는 비 AEM 웹 속성을 사용하여 클라이언트측을 호출하여 인증되거나 인증되지 않은 AEM을 모두 호출하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있도록 합니다.

이 문서에 설명된 OSGI 구성은 다음 작업에 충분합니다.

1. AEM 게시의 단일 원본 리소스 공유
2. AEM 작성자에 대한 CORS 액세스

AEM 게시에서 다중 출처 CORS 액세스가 필요한 경우 다음을 참조하십시오. [이 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

## Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성

CORS 구성은 AEM에서 OSGi 구성 팩토리로 관리되며 각 정책은 팩토리의 하나의 인스턴스로 표시됩니다.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 원본 간 리소스 공유 정책 OSGi 구성](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 정책 선택

정책을 선택하려면

* `Allowed Origin` (으)로 `Origin` 요청 헤더
* 및 `Allowed Paths` 요청 경로 포함.

이 값과 일치하는 첫 번째 정책이 사용됩니다. 찾을 수 없는 경우 [!DNL CORS] 요청이 거부되었습니다.

구성된 정책이 없으면 [!DNL CORS] 서버의 다른 모듈이 응답하지 않는 한 핸들러가 비활성화되어 효과적으로 거부되므로 요청에 대한 응답도 이루어지지 않습니다. [!DNL CORS].

### 정책 속성

#### [!UICONTROL 허용된 원본]

* `"alloworigin" <origin> | *`
* 목록 `origin` 리소스에 액세스할 수 있는 URI를 지정하는 매개 변수입니다. 자격 증명이 없는 요청의 경우 서버에서 다음을 지정할 수 있습니다. &#42; 를 와일드카드로 만들기 때문에 모든 원본이 리소스에 액세스할 수 있습니다. *를 사용하지 않는 것이 좋습니다 `Allow-Origin: *` 프로덕션 환경에서는 모든 외국(즉, 공격자) 웹 사이트에서 CORS가 없는 것은 브라우저에서 엄격히 금지된다는 요청을 할 수 있기 때문입니다.*

#### [!UICONTROL 허용된 원본 (Regexp)]

* `"alloworiginregexp" <regexp>`
* 목록 `regexp` 리소스에 액세스할 수 있는 URI를 지정하는 정규 표현식. *정규 표현식은 신중하게 작성되지 않으면 의도하지 않은 일치로 이어질 수 있으므로 공격자가 정책과 일치하는 사용자 정의 도메인 이름을 사용할 수 있습니다.* 일반적으로 를 사용하여 각 특정 원본 호스트 이름에 대해 별도의 정책을 사용하는 것이 좋습니다. `alloworigin`를 조정할 경우 다른 정책 속성이 반복적으로 구성된 경우에도 마찬가지입니다. 서로 다른 기원은 서로 다른 수명 주기와 요구 사항을 갖는 경향이 있으므로 명확한 분리로부터 이익을 얻을 수 있다.

#### [!UICONTROL 허용되는 경로]

* `"allowedpaths" <regexp>`
* 목록 `regexp` 정책이 적용되는 리소스 경로를 지정하는 정규 표현식.

#### [!UICONTROL 노출된 헤더]

* `"exposedheaders" <header>`
* 브라우저가 액세스할 수 있는 응답 헤더를 나타내는 헤더 매개 변수 목록입니다. CORS 요청(프리플라이트 아님)의 경우 비어 있지 않으면 이러한 값이 `Access-Control-Expose-Headers` 응답 헤더. 그러면 목록의 값(헤더 이름)이 브라우저에 액세스할 수 있게 됩니다. 이 값이 없으면 브라우저에서 해당 헤더를 읽을 수 없습니다.

#### [!UICONTROL 최대 나이]

* `"maxage" <seconds>`
* A `seconds` 프리플라이트 요청의 결과가 캐시될 수 있는 기간을 나타내는 매개 변수.

#### [!UICONTROL 지원되는 헤더]

* `"supportedheaders" <header>`
* 목록 `header` 실제 요청을 만들 때 사용할 수 있는 HTTP 요청 헤더를 나타내는 매개 변수입니다.

#### [!UICONTROL 허용된 메서드]

* `"supportedmethods"`
* 실제 요청을 수행할 때 사용할 수 있는 HTTP 메서드를 나타내는 메서드 매개 변수 목록입니다.

#### [!UICONTROL 자격 증명 지원]

* `"supportscredentials" <boolean>`
* A `boolean` 요청에 대한 응답을 브라우저에 노출할 수 있는지 여부를 나타냅니다. 프리플라이트 요청에 대한 응답의 일부로 사용되는 경우 자격 증명을 사용하여 실제 요청을 수행할 수 있는지 여부를 나타냅니다.

### 예제 구성

사이트 1은 익명으로 액세스할 수 있는 기본 읽기 전용 시나리오로, 콘텐츠를 [!DNL GET] 요청:

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
    "Access-Control-Request-Headers",
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

Dispatcher 4.1.1 이상 응답 헤더를 캐시할 수 있습니다. 이를 통해 캐싱할 수 있습니다 [!DNL CORS] 를 따라 헤더 표시 [!DNL CORS]- 요청이 익명으로 되어 있는 한 요청된 리소스입니다.

일반적으로 Dispatcher에서 콘텐츠를 캐시하기 위한 동일한 고려 사항은 Dispatcher에서 CORS 응답 헤더를 캐시하는 데 적용할 수 있습니다. 다음 표는 다음과 같은 시점을 정의합니다 [!DNL CORS] 헤더(따라서 [!DNL CORS] 요청)을 캐시할 수 있습니다.

| 캐시 가능 | 환경 | 인증 상태 | 설명 |
|-----------|-------------|-----------------------|-------------|
| 아니요 | AEM 게시 | 인증됨 | AEM 작성자의 Dispatcher 캐싱은 작성되지 않은 정적 자산으로 제한됩니다. 따라서 HTTP 응답 헤더를 포함하여 AEM Author에 대부분의 리소스를 캐시하는 것이 어렵고 비현실적입니다. |
| 아니요 | AEM 게시 | 인증됨 | 인증된 요청에 대해 CORS 헤더를 캐시하지 마십시오. 이는 요청하는 사용자의 인증/권한 부여 상태가 전달된 리소스에 미치는 영향을 판단하기 어렵기 때문에 인증된 요청을 캐시하지 않는다는 일반적인 지침을 따릅니다. |
| 예 | AEM 게시 | 익명 | Dispatcher에서 캐시 가능한 익명 요청은 응답 헤더도 캐시할 수 있으므로 향후 CORS 요청이 캐시된 콘텐츠에 액세스할 수 있습니다. AEM 게시의 모든 CORS 구성 변경 **필수** 영향을 받는 캐시된 리소스의 무효화가 뒤따릅니다. 우수 사례에서는 영향을 줄 수 있는 캐시된 콘텐츠를 확인하기 어렵기 때문에 Dispatcher 캐시가 삭제되는 코드 또는 구성 배포에 대해 설명합니다. |

### CORS 요청 헤더 허용

필요한 항목을 허용하려면 [처리를 위해 AEM으로 전달할 HTTP 요청 헤더](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), Dispatcher에 허용되어야 합니다. `/clientheaders` 구성.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS 응답 헤더 캐싱

캐시된 콘텐츠에서 CORS 헤더를 캐시하고 제공할 수 있도록 하려면 다음을 추가하십시오 [/cache /headers 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#caching-http-response-headers) AEM 게시로 `dispatcher.any` 파일.

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

다음을 잊지 마십시오. **웹 서버 응용 프로그램을 다시 시작합니다.** 을(를) 변경한 후 `dispatcher.any` 파일.

다음 요청 시 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 수 있습니다. `/cache/headers` 구성 업데이트.

## CORS 문제 해결

로깅은 다음에서 사용할 수 있습니다. `com.adobe.granite.cors`:

* 활성화 `DEBUG` 이(가) 왜 [!DNL CORS] 요청이 거부되었습니다.
* 활성화 `TRACE` CORS 핸들러를 거치는 모든 요청에 대한 세부 정보 보기

### 팁:

* curl을 사용하여 XHR 요청을 수동으로 다시 생성하지만, 각 헤더 및 세부 정보가 다를 수 있으므로 모든 헤더와 세부 정보를 복사해야 합니다. 일부 브라우저 콘솔에서는 curl 명령을 복사할 수 있습니다.
* 인증, CSRF 토큰 필터, Dispatcher 필터 또는 기타 보안 레이어가 아닌 CORS 핸들러에서 요청이 거부되었는지 확인합니다.
   * CORS 처리기가 200으로 응답하면 `Access-Control-Allow-Origin` 헤더에 응답이 없습니다. 로그에서 거부 여부를 검토하십시오. [!DNL DEBUG] 위치: `com.adobe.granite.cors`
* 의 Dispatcher 캐싱인 경우 [!DNL CORS] 요청이 활성화됨
   * 다음을 확인합니다. `/cache/headers` 구성이에 적용됩니다. `dispatcher.any` 웹 서버가 다시 시작되었습니다.
   * OSGi 또는 dispatcher.any 구성을 변경한 후 캐시가 제대로 지워졌는지 확인하십시오.
* 필요한 경우 요청에 대한 인증 자격 증명이 있는지 확인합니다.

## 지원 자료

* [원본 간 리소스 공유 정책을 위한 AEM OSGi 구성 팩토리](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [원본 간 리소스 공유(W3C)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
