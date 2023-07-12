---
title: AEM GraphQL에 대한 CORS 구성
description: AEM GraphQL에서 사용할 CORS(원본 간 리소스 공유)를 구성하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: 36b4217a899b462296d4389bc96a644da97d5da4
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# CORS(원본 간 리소스 공유)

Adobe Experience Manager as a Cloud Service의 CORS(원본 간 리소스 공유)를 사용하면 AEM이 아닌 웹 속성을 사용하여 AEM GraphQL API에 대한 브라우저 기반 클라이언트측 호출을 할 수 있습니다.

다음 문서에서는 구성 방법을 설명합니다 _단일 원본_ cors를 통해 특정 AEM Headless 엔드포인트 세트에 액세스 단일 원본은 AEM이 아닌 단일 도메인만 AEM에 액세스함을 의미합니다(예: https://www.example.com에 연결하는 https://app.example.com). 다중 원본 액세스는 캐싱 문제로 인해 이 접근 방식을 사용하여 작동하지 않을 수 있습니다.

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하십시오.

## CORS 요구 사항

AEM에 연결하는 클라이언트가 AEM과 동일한 원본(호스트 또는 도메인이라고도 함)에서 제공되지 않을 경우 AEM GraphQL API에 대한 브라우저 기반 연결에 CORS가 필요합니다.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 구성 필요 | ✔ | ✔ | ✘ | ✘ |

## OSGi 구성

AEM CORS OSGi 구성 팩토리는 CORS HTTP 요청을 수락하는 허용 기준을 정의합니다.

| 클라이언트가에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 구성 필요 | ✔ | ✔ | ✔ |


아래 예제는 AEM 게시용 OSGi 구성(`../config.publish/..`)에 추가할 수 있습니다 [지원되는 모든 실행 모드 폴더](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

주요 구성 속성은 다음과 같습니다.

+ `alloworigin` 및/또는 `alloworiginregexp` AEM 웹에 연결하는 클라이언트가 실행되는 원본을 지정합니다.
+ `allowedpaths` 지정된 원본에서 허용되는 URL 경로 패턴을 지정합니다.
   + AEM GraphQL 지속 쿼리를 지원하려면 다음 패턴을 추가합니다. `/graphql/execute.json.*`
   + 경험 조각을 지원하려면 다음 패턴을 추가합니다. `/content/experience-fragments/.*`
+ `supportedmethods` cors 요청에 허용된 HTTP 메서드를 지정합니다. 추가 `GET`AEM GraphQL 지속 쿼리(및 경험 조각)를 지원합니다.

[CORS OSGi 구성에 대해 자세히 알아보십시오.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

이 예제 구성은 AEM GraphQL 지속 쿼리 사용을 지원합니다. 클라이언트 정의 GraphQL 쿼리를 사용하려면에 GraphQL 끝점 URL을 추가하십시오. `allowedpaths` 및 `POST` 끝 `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
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
    "HEAD",
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 승인된 AEM GraphQL API 요청

승인이 필요한 AEM GraphQL API(일반적으로 AEM Publish의 작성자 또는 보호된 콘텐츠)에 액세스할 때 CORS OSGi 구성에 추가 값이 있는지 확인합니다.

+ `supportedheaders` 목록 `"Authorization"`
+ `supportscredentials` 이(가) (으)로 설정됨 `true`

CORS 구성이 필요한 AEM GraphQL API에 대한 승인된 요청은 일반적으로 의 컨텍스트에서 발생하므로 이례적입니다. [서버 간 앱](../server-to-server.md) 따라서 CORS 구성이 필요하지 않습니다. 다음과 같은 CORS 구성이 필요한 브라우저 기반 앱 [단일 페이지 앱](../spa.md) 또는 [웹 구성 요소](../web-component.md)는 자격 증명 보안을 유지하기 어려우므로 일반적으로 권한 부여를 사용합니다.

예를 들어 이 두 설정은 `CORSPolicyImpl` OSGi 공장 구성:

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

+ [OSGi 구성의 예는 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 구성

AEM 게시(및 미리보기) 서비스의 Dispatcher가 CORS를 지원하도록 구성되어 있어야 합니다.

| 클라이언트가에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher CORS 구성 필요 | ✘ | ✔ | ✔ |

### HTTP 요청에 CORS 헤더 허용

업데이트 `clientheaders.any` HTTP 요청 헤더를 허용하는 파일 `Origin`,  `Access-Control-Request-Method`, 및 `Access-Control-Request-Headers` 을 AEM에 전달하여 HTTP 요청을 [AEM CORS 구성](#osgi-configuration).

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

+ [Dispatcher 의 예 _클라이언트 헤더_ 구성은 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### CORS HTTP 응답 헤더 전달

캐시할 Dispatcher 팜 구성 **CORS HTTP 응답 헤더** 를 추가하여 Dispatcher 캐시에서 AEM GraphQL 지속 쿼리가 제공될 때 이를 포함시킵니다. `Access-Control-...` 헤더를 캐시 헤더 목록에 추가합니다.

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

+ [Dispatcher 의 예 _CORS HTTP 응답 헤더_ 구성은 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
