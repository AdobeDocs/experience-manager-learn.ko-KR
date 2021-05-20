---
title: AEM과의 CORS(원본 간 리소스 공유) 이해
description: Adobe Experience Manager의 CORS(Cross-Origin Resource Sharing)를 사용하면 AEM이 아닌 웹 속성을 사용하여 인증되고 인증되지 않은 AEM에 클라이언트측 호출을 수행하여 컨텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.
version: 6.3, 6,4, 6.5
sub-product: foundation, content services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: 보안
role: Developer
level: Intermediate
source-git-commit: 1c99c319fba5048904177fc82c43554b0cf0fc15
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 1%

---


# 원본 간 리소스 공유 이해([!DNL CORS])

Adobe Experience Manager의 원본 간 리소스 공유([!DNL CORS])를 사용하면 AEM이 아닌 웹 속성을 사용하여 인증되고 인증되지 않은 AEM에 클라이언트측 호출을 수행하여 콘텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.

## Adobe Granite 교차 도메인 리소스 공유 정책 OSGi 구성

CORS 구성은 AEM에서 OSGi 구성 팩토리로 관리되며 각 정책은 팩토리의 한 인스턴스로 표시됩니다.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 교차 도메인 리소스 공유 정책 OSGi 구성](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 정책 선택

정책을 비교하여 선택합니다

* `Allowed Origin` 및  `Origin` 요청 헤더 사용
* 및 요청 경로가 있는 `Allowed Paths`.

이러한 값과 일치하는 첫 번째 정책이 사용됩니다. 찾을 수 없으면 [!DNL CORS] 요청이 거부됩니다.

정책이 전혀 구성되지 않으면 처리기가 비활성화되므로 서버의 다른 모듈이 [!DNL CORS]에 응답하지 않는 한 [!DNL CORS] 요청도 응답되지 않습니다.

### 정책 속성

#### [!UICONTROL 허용된 원본]

* `"alloworigin" <origin> | *`
* 리소스에 액세스할 수 있는 URI를 지정하는 `origin` 매개 변수 목록 자격 증명이 없는 요청의 경우 서버는 *를 와일드카드로 지정하여 모든 원본에서 리소스에 액세스할 수 있도록 합니다. *모든 외국인(즉, 공격자) 웹 사이트 `Allow-Origin: *` 에서 CORS가 없는 경우 브라우저가 이를 엄격하게 금지하도록 허용하므로 프로덕션 환경에서는 를 사용하지 않는 것이 좋습니다.*

#### [!UICONTROL 허용된 원본(Regexp)]

* `"alloworiginregexp" <regexp>`
* 리소스에 액세스할 수 있는 URI를 지정하는 `regexp` 정규 표현식 목록입니다. *정규 표현식을 주의 깊게 빌드하지 않으면 의도하지 않은 일치 항목이 발생할 수 있으므로 공격자가 정책과 일치하는 사용자 지정 도메인 이름을 사용할 수 있습니다.* 일반적으로 다른 정책 속성을 반복적으로 구성한다는  `alloworigin`의미라도 를 사용하여 특정 원본 호스트 이름에 대해 별도의 정책을 사용하는 것이 좋습니다. 서로 다른 기원은 라이프 사이클과 요구 사항이 다르기 때문에 명확한 분리로부터 이익을 얻는 경향이 있다.

#### [!UICONTROL 허용되는 경로]

* `"allowedpaths" <regexp>`
* 정책이 적용되는 리소스 경로를 지정하는 `regexp` 정규 표현식 목록입니다.

#### [!UICONTROL 노출된 헤더]

* `"exposedheaders" <header>`
* 브라우저에 액세스할 수 있는 요청 헤더를 나타내는 헤더 매개 변수 목록입니다.

#### [!UICONTROL 최대 연령]

* `"maxage" <seconds>`
* 플라이트 전 요청의 결과를 캐시할 수 있는 시간을 나타내는 `seconds` 매개 변수입니다.

#### [!UICONTROL 지원되는 헤더]

* `"supportedheaders" <header>`
* 실제 요청을 만들 때 사용할 수 있는 HTTP 헤더를 나타내는 `header` 매개 변수 목록입니다.

#### [!UICONTROL 허용된 메서드]

* `"supportedmethods"`
* 실제 요청을 만들 때 사용할 수 있는 HTTP 메서드를 나타내는 메서드 매개 변수 목록입니다.

#### [!UICONTROL 자격 증명 지원]

* `"supportscredentials" <boolean>`
* 요청에 대한 응답을 브라우저에 표시할 수 있는지 여부를 나타내는 `boolean`. 플라이트 전 요청에 대한 응답의 일부로 사용되는 경우, 자격 증명을 사용하여 실제 요청을 수행할 수 있는지 여부를 나타냅니다.

### 구성 예

사이트 1은 [!DNL GET] 요청을 통해 컨텐츠를 사용하는 기본적이고 익명으로 액세스할 수 있는 읽기 전용 시나리오입니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

사이트 2가 더 복잡하며 승인되고 안전하지 않은 요청이 필요합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Dispatcher 캐싱 문제 및 구성 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1+ 응답 헤더부터 캐시할 수 있습니다. 따라서 요청이 익명 요청인 경우 [!DNL CORS] 요청된 리소스와 함께 [!DNL CORS] 헤더를 캐싱할 수 있습니다.

일반적으로 Dispatcher에서 콘텐츠를 캐싱하는 것과 동일한 고려 사항을 Dispatcher에서 CORS 응답 헤더를 캐싱하는 데 적용할 수 있습니다. 다음 표는 [!DNL CORS] 헤더(및 따라서 [!DNL CORS] 요청)를 캐시할 수 있는 시기를 정의합니다.

| 캐시 가능 | 환경 | 인증 상태 | 설명 |
|-----------|-------------|-----------------------|-------------|
| 아니오 | AEM 게시 | 인증됨 | AEM 작성자에 대한 Dispatcher 캐싱은 정적 비작성 자산으로 제한됩니다. 따라서 HTTP 응답 헤더를 포함하여 대부분의 리소스를 AEM 작성자에게 캐시하는 것이 어렵고 비현실적입니다. |
| 아니오 | AEM 게시 | 인증됨 | 인증된 요청 시 CORS 헤더를 캐싱하지 마십시오. 이는 요청 사용자의 인증/승인 상태가 전달된 리소스에 어떤 영향을 주는지 결정하기가 어렵기 때문에 인증된 요청을 캐싱하지 않는다는 일반적인 지침을 따릅니다. |
| 예 | AEM 게시 | 익명 | dispatcher에서 캐시 가능한 익명 요청은 응답 헤더도 캐시할 수 있으므로 향후 CORS 요청이 캐시된 컨텐츠에 액세스할 수 있도록 합니다. AEM 게시 **에 대한 모든 CORS 구성 변경 사항에는 영향을 받는 캐시된 리소스의 무효화가 뒤따라야 합니다.** 우수 사례에 따라 디스패처 캐시가 삭제되는 코드 또는 구성 배포에 대해 결정됩니다. 캐시된 콘텐츠가 영향을 받을 수 있는지 확인하는 것은 어렵습니다. |

CORS 헤더의 캐싱을 허용하려면 다음 구성을 모든 지원 AEM Publish dispatcher.any 파일에 추가합니다.

```
/cache { 
  ...
  /headers {
      "Origin",
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

`dispatcher.any` 파일을 변경한 후 **웹 서버 응용 프로그램**&#x200B;을 다시 시작해야 합니다.

`/cache/headers` 구성 업데이트 후 다음 요청에서 헤더가 적절하게 캐시되도록 하려면 캐시를 완전히 지워야 할 가능성이 높습니다.

## CORS 문제 해결

로깅은 `com.adobe.granite.cors`에서 사용할 수 있습니다.

* [!DNL CORS] 요청이 거부된 이유에 대한 세부 정보를 보려면 `DEBUG` 사용
* `TRACE` 을 활성화하여 CORS 핸들러를 통과하는 모든 요청에 대한 세부 정보를 확인합니다.

### 팁:

* curl을 사용하여 XHR 요청을 수동으로 다시 만들지만, 각 요청에 차이가 있을 수 있으므로 모든 헤더와 세부 정보를 복사해야 합니다.일부 브라우저 콘솔에서는 curl 명령을 복사할 수 있습니다
* CORS 처리기에서 요청이 거부되었는지, 인증, CSRF 토큰 필터, 디스패처 필터 또는 기타 보안 레이어가 아닌지 확인합니다
   * CORS 처리기가 200에 응답하지만 `Access-Control-Allow-Origin` 헤더가 응답에 없는 경우 `com.adobe.granite.cors`의 [!DNL DEBUG] 아래에 있는 거부 로그를 검토하십시오
* [!DNL CORS] 요청의 디스패처 캐싱이 활성화된 경우
   * `/cache/headers` 구성이 `dispatcher.any`에 적용되어 웹 서버가 성공적으로 다시 시작되었는지 확인합니다
   * OSGi 또는 dispatcher.any 구성을 변경한 후 캐시가 제대로 지워졌는지 확인합니다.
* 필요한 경우 요청에 인증 자격 증명이 있는지 확인합니다.

## 지원 자료

* [교차 도메인 리소스 공유 정책을 위한 AEM OSGi Configuration factory](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [교차 도메인 리소스 공유(W3C)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
