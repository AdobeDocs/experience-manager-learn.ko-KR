---
title: AEM과 CORS(Cross-Origin Resource Sharing) 이해
description: Adobe Experience Manager의 CORS(Cross-Origin Resource Sharing)를 사용하면 AEM이 아닌 웹 속성을 통해 인증된 AEM 및 인증되지 않은로 클라이언트측 호출을 수행하여 컨텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.
version: 6.3, 6,4, 6.5
sub-product: foundation, content services, sites
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# 상호 출처 리소스 공유 이해([!DNL CORS])

Adobe Experience Manager의 교차 도메인 리소스 공유([!DNL CORS])는 비 AEM 웹 속성을 활용하여 인증된 AEM과 인증되지 않은을 클라이언트측 호출함으로써 컨텐츠를 가져오거나 AEM과 직접 상호 작용할 수 있습니다.

## Adobe OSGi 구성

CORS 구성은 AEM의 OSGi 구성 팩터리로 관리되며 각 정책은 팩터리의 한 인스턴스에 해당합니다.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe OSGi 구성](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 정책 선택

정책은

* `Allowed Origin` 를 `Origin` 요청 헤더로
* 및 요청 경로 `Allowed Paths` 를 사용합니다.

이러한 값과 일치하는 첫 번째 정책이 사용됩니다. 아무 것도 찾을 수 없으면 모든 [!DNL CORS] 요청이 거부됩니다.

정책을 전혀 구성하지 않으면, 핸들러가 비활성화되어, 따라서 서버의 다른 모듈이 응답하지 않으면 요청이 효과적으로 거부되므로 [!DNL CORS] 요청도 응답하지 않습니다 [!DNL CORS].

### 정책 속성

#### [!UICONTROL 허용된 원본]

* `"alloworigin" <origin> | *`
* 리소스에 액세스할 수 있는 URI를 지정하는 매개 변수 `origin` 목록입니다. 자격 증명이 없는 요청의 경우 서버는 *를 와일드카드로 지정하여 모든 원본 사용자가 리소스에 액세스할 수 있도록 허용합니다. *CORS가 없는 웹 사이트에서 모든 외부(즉, 공격자) 웹 사이트에서 CORS가 브라우저에 의해 엄격히 금지된다는 요청을 하도록 허용하므로 제작 시 사용하지 않는 것이 좋습니다.`Allow-Origin: *`*

#### [!UICONTROL 허용된 원본(등록)]

* `"alloworiginregexp" <regexp>`
* 리소스에 액세스할 수 있는 URI를 지정하는 일반 표현식 `regexp` 목록입니다. *정규식은 주의깊게 빌드하지 않을 경우 의도하지 않은 일치로 이어질 수 있으므로 침입자가 정책과 일치하는 사용자 정의 도메인 이름을 사용할 수 있습니다.* 일반적으로 다른 정책 속성을 반복 구성하는 경우에도 각 특정 원본 호스트 이름 `alloworigin`에 대해 별도의 정책을 사용하는 것이 좋습니다. 서로 다른 기원은 다른 라이프 사이클과 요구 사항을 가지기 때문에 명확한 분리에서 이익을 얻는 경향이 있다.

#### [!UICONTROL 허용되는 경로]

* `"allowedpaths" <regexp>`
* 정책이 적용되는 리소스 경로를 지정하는 일반 표현식 `regexp` 목록입니다.

#### [!UICONTROL 노출된 머리글]

* `"exposedheaders" <header>`
* 브라우저에 액세스할 수 있는 요청 헤더를 나타내는 헤더 매개 변수 목록입니다.

#### [!UICONTROL 최대 연령]

* `"maxage" <seconds>`
* 플라이트 전 요청의 결과를 캐싱할 수 있는 기간을 나타내는 `seconds` 매개 변수입니다.

#### [!UICONTROL 지원되는 헤더]

* `"supportedheaders" <header>`
* 실제 요청 시 사용할 수 있는 HTTP 헤더를 나타내는 매개 변수 `header` 목록입니다.

#### [!UICONTROL 허용되는 메서드]

* `"supportedmethods"`
* 실제 요청 시 사용할 수 있는 HTTP 메서드를 나타내는 메서드 매개 변수 목록입니다.

#### [!UICONTROL 자격 증명 지원]

* `"supportscredentials" <boolean>`
* 요청에 대한 응답을 브라우저에 표시할 수 있는지 여부를 `boolean` 나타내는 것입니다. 플라이트 전 요청에 대한 응답의 일부로 사용되는 경우 자격 증명을 사용하여 실제 요청을 수행할 수 있는지 여부를 나타냅니다.

### 구성 예

사이트 1은 요청을 통해 컨텐츠를 사용하는 기본, 익명으로 액세스 가능한 읽기 전용 [!DNL GET] 시나리오입니다.

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

사이트 2는 보다 복잡하며 권한 부여 및 안전하지 않은 요청이 필요합니다.

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

## 디스패처 캐싱 문제 및 구성 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1+ 응답 헤더부터 캐싱할 수 있습니다. 요청이 익명이면 요청이 [!DNL CORS] 요청된 리소스를 따라 헤더를 캐싱할 수 [!DNL CORS]있습니다.

일반적으로 디스패처의 컨텐츠 캐싱에 대한 것과 동일한 고려 사항을 디스패처의 CORS 응답 헤더를 캐싱하는 데 적용할 수 있습니다. 다음 표에서는 헤더(및 따라서 요청)를 캐싱할 수 있는 시기를 [!DNL CORS] [!DNL CORS] 정의합니다.

| 액세스 가능 | 환경 | 인증 상태 | 설명 |
|-----------|-------------|-----------------------|-------------|
| 아니오 | AEM 게시 | 인증됨 | AEM 작성자의 발송자 캐싱은 작성되지 않은 정적 자산으로 제한됩니다. 따라서 HTTP 응답 헤더를 비롯한 대부분의 리소스를 AEM 작성자에게 캐시하는 것이 어렵고 비실용적입니다. |
| 아니오 | AEM 게시 | 인증됨 | 인증된 요청에 CORS 헤더를 캐시하지 마십시오. 이는 요청된 사용자의 인증/인증 상태가 전달되는 리소스에 어떤 영향을 미치는지 결정하기 어려우므로 인증된 요청을 캐시하지 않는 일반적인 지침에 따릅니다. |
| 예 | AEM 게시 | 익명 | 발송자에서 캐시로 사용할 수 있는 익명 요청은 또한 응답 헤더를 캐시하여 향후 CORS 요청이 캐시된 컨텐츠에 액세스할 수 있도록 할 수 있습니다. AEM 게시에 대한 모든 CORS 구성 변경 **에 영향을 받는 캐시된 리소스의 무효화가 뒤따라야 합니다** . 어떤 캐시된 콘텐츠가 영향을 받을 수 있는지 결정하기 어려우므로, 우수 사례는 디스패처 캐시가 삭제되는 코드 또는 구성 배포에 영향을 줍니다. |

CORS 헤더의 캐싱을 허용하려면 다음 구성을 모든 지원 AEM 게시 디스패처.any 파일에 추가합니다.

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

파일을 변경한 후 웹 서버 응용 프로그램 **을** `dispatcher.any` 다시 시작해야 합니다.

구성 업데이트 후 헤더가 다음 요청에 적절하게 캐시되도록 하기 위해 캐시를 완전히 지우는 것이 `/headers` 좋습니다.

## CORS 문제 해결

로깅은 다음 아래에서 사용할 수 있습니다 `com.adobe.granite.cors`.

* 요청이 거부된 이유에 대한 세부 사항 `DEBUG` 을 볼 수 있도록 [!DNL CORS] 설정
* CORS 핸들러를 통해 진행되는 모든 요청에 대한 세부 사항을 볼 수 있도록 설정 `TRACE`

### 팁:

* 말림(curl)을 사용하여 XHR 요청을 수동으로 다시 만들 수 있지만 각각의 요청에 차이가 있을 수 있으므로 모든 헤더와 세부 정보를 복사해야 합니다.일부 브라우저 콘솔에서는 curl 명령을 복사할 수 있습니다.
* 인증, CSRF 토큰 필터, 디스패처 필터 또는 기타 보안 레이어가 아닌 CORS 핸들러에 의해 요청이 거부되었는지 확인
   * CORS 처리기가 200으로 응답하지만 `Access-Control-Allow-Origin` [!DNL DEBUG] 헤더가 응답에 없는 경우 로그인 중인 거부에 대한 거부 로그를 검토합니다. `com.adobe.granite.cors`
* 요청의 발송자 캐싱이 [!DNL CORS] 활성화된 경우
   * 구성을 `/headers` 적용하고 웹 서버를 다시 `dispatcher.any` 시작했는지 확인합니다.
   * OSGi 또는 dispatcher.구성이 변경된 후 캐시가 올바르게 지워졌는지 확인합니다.
* 필요한 경우 요청에 인증 자격 증명이 있는지 확인합니다.

## 지원 자료

* [AEM OSGi Configuration factory for Cross-Origin Resource Sharing Policies](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [W3C(Cross-Origin Resource Sharing)](https://www.w3.org/TR/cors/)
* [HTTP 액세스 제어(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
