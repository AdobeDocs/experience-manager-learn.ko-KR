---
title: AEM GraphQL에 대한 CORS 구성
description: AEM GraphQL에서 사용할 CORS(원본 간 리소스 공유)를 구성하는 방법을 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 1%

---


# CORS(원본 간 리소스 공유)

Adobe Experience Manager as a Cloud Service의 CORS(Cross-Origin Resource Sharing)를 사용하면 AEM이 아닌 웹 속성을 사용하여 AEM GraphQL API를 브라우저 기반의 클라이언트측 호출로 수행할 수 있습니다.

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하도록 합니다.

## CORS 요구 사항

AEM에 연결하는 클라이언트가 AEM과 동일한 원본(호스트 또는 도메인이라고도 함)에서 지원되지 않는 경우 AEM GraphQL API에 대한 브라우저 기반 연결에 CORS가 필요합니다.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 구성 필요 | ✔ | ✔ | ✘ | ✘ |

## OSGi 구성

AEM CORS OSGi 구성 팩토리는 CORS HTTP 요청을 수락하기 위한 허용 기준을 정의합니다.

| 클라이언트 연결 대상 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 구성 필요 | ✔ | ✔ | ✔ |


아래 예제는 AEM 게시(`../config.publish/..`)에 사용할 수 있지만, [지원되는 실행 모드 폴더](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

주요 구성 속성은 다음과 같습니다.

+ `alloworigin` 및/또는 `alloworiginregexp` AEM 웹에 연결된 클라이언트가 실행되는 원본을 지정합니다.
+ `allowedpaths` 지정된 원본에서 허용되는 URL 경로 패턴을 지정합니다. AEM GraphQL 지속적인 쿼리를 지원하려면 다음 패턴을 사용합니다. `"/graphql/execute.json.*"`
+ `supportedmethods` cors 요청에 대해 허용되는 HTTP 메서드를 지정합니다. 추가 `GET`를 사용하여 AEM GraphQL 지속적인 쿼리를 지원할 수 있습니다.

[CORS OSGi 구성에 대해 자세히 알아보십시오 .](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


이 예제 구성은 AEM GraphQL 지속적인 쿼리 사용을 지원합니다. 클라이언트 정의 GraphQL 쿼리를 사용하려면 GraphQL 엔드포인트 URL을 `allowedpaths` 및 `POST` to `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 인증된 AEM GraphQL API 요청

권한이 필요한 AEM GraphQL API에 액세스할 때(일반적으로 AEM Publish에서 AEM 작성자 또는 보호된 컨텐츠) CORS OSGi 구성에 추가 값이 있는지 확인합니다.

+ `supportedheaders` 또한 목록 `"Authorization"`
+ `supportscredentials` 가 로 설정되어 있습니다. `true`

CORS 구성이 필요한 AEM GraphQL API에 대한 인증된 요청은 일반적으로 다음의 컨텍스트에서 발생하므로 드문 경우입니다 [서버 간 앱](../server-to-server.md) 따라서 CORS 구성이 필요하지 않습니다. 다음과 같이 CORS 구성이 필요한 브라우저 기반 앱 [단일 페이지 앱](../spa.md) 또는 [웹 구성 요소](../web-component.md)인 경우 일반적으로 자격 증명 의 보안을 유지하는 것이 어려우므로 권한 부여를 사용합니다.

예를 들어, 이 두 설정은 `CORSPolicyImpl` OSGi 공장 구성:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### OSGi 구성 예

+ [OSGi 구성의 예는 WKND 프로젝트에서 확인할 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 구성

AEM 게시(및 미리 보기) 서비스의 Dispatcher는 CORS를 지원하도록 구성해야 합니다.

| 클라이언트 연결 대상 | AEM 작성자 | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher CORS 구성 필요 | ✘ | ✔ | ✔ |

### HTTP 요청에 CORS 헤더 허용

업데이트 `clientheaders.any` HTTP 요청 헤더를 허용하는 파일 `Origin`,  `Access-Control-Request-Method`, 및 `Access-Control-Request-Headers` AEM으로 전달하여 HTTP 요청을 [AEM CORS 구성](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher 구성 예

+ [Dispatcher 예 _클라이언트 헤더_ 구성은 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### CORS HTTP 응답 헤더 전달

캐싱하도록 Dispatcher 팜 구성 **CORS HTTP 응답 헤더** 를 추가하여 AEM GraphQL 지속적인 쿼리가 Dispatcher 캐시에서 제공될 때 이러한 쿼리가 포함되어 있는지 확인합니다. `Access-Control-...` 헤더 목록에 헤더를 추가합니다.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

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

#### Dispatcher 구성 예

+ [Dispatcher 예 _CORS HTTP 응답 헤더_ 구성은 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
