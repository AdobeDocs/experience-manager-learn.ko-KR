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
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: f8fd13d3f315aa0bd9f268b9fe81b9d9c17b243c
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 2%

---

# CORS(원본 간 리소스 공유)

Adobe Experience Manager as a Cloud Service의 CORS(원본 간 리소스 공유)는 비 AEM 웹 속성을 사용하여 AEM GraphQL API 및 기타 AEM Headless 리소스에 대한 브라우저 기반 클라이언트측 호출을 수행할 수 있도록 합니다.

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하십시오.

## CORS 요구 사항

AEM에 연결하는 클라이언트가 AEM과 동일한 원본(호스트 또는 도메인이라고도 함)에서 제공되지 않을 경우 AEM GraphQL API에 대한 브라우저 기반 연결에 CORS가 필요합니다.

| 클라이언트 유형 | [단일 페이지 앱(SPA)](../spa.md) | [웹 구성 요소/JS](../web-component.md) | [모바일](../mobile.md) | [서버 간](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 구성 필요 | ✔ | ✔ | ✘ | ✘ |

## AEM Author

AEM Author 서비스에서 CORS를 활성화하는 것은 AEM Publish 및 AEM 미리보기 서비스와 다릅니다. AEM Author 서비스는 AEM Author 서비스의 실행 모드 폴더에 OSGi 구성을 추가해야 하며 Dispatcher 구성을 사용하지 않습니다.

### OSGi 구성

AEM CORS OSGi 구성 팩토리는 CORS HTTP 요청을 수락하는 허용 기준을 정의합니다.

| 클라이언트가에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 구성 필요 | ✔ | ✘ | ✘ |


아래 예제는 AEM 작성자용 OSGi 구성(`../config.author/..`) AEM Author 서비스에서만 활성화됩니다.

주요 구성 속성은 다음과 같습니다.

+ `alloworigin` 및/또는 `alloworiginregexp` AEM 웹에 연결하는 클라이언트가 실행되는 원본을 지정합니다.
+ `allowedpaths` 지정된 원본에서 허용되는 URL 경로 패턴을 지정합니다.
   + AEM GraphQL 지속 쿼리를 지원하려면 다음 패턴을 추가합니다. `/graphql/execute.json.*`
   + 경험 조각을 지원하려면 다음 패턴을 추가합니다. `/content/experience-fragments/.*`
+ `supportedmethods` cors 요청에 허용된 HTTP 메서드를 지정합니다. AEM GraphQL 지속 쿼리(및 경험 조각)를 지원하려면 다음을 추가하십시오. `GET` .
+ `supportedheaders` 포함 `"Authorization"` as AEM 작성자에 대한 요청은 승인되어야 합니다.
+ `supportscredentials` 이(가) (으)로 설정됨 `true` as request to AEM Author 는 승인되어야 합니다.

[CORS OSGi 구성에 대해 자세히 알아보십시오.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

다음 예에서는 AEM 작성자에서 AEM GraphQL 지속 쿼리 사용을 지원합니다. 클라이언트 정의 GraphQL 쿼리를 사용하려면에 GraphQL 끝점 URL을 추가하십시오. `allowedpaths` 및 `POST` 끝 `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### OSGi 구성 예

+ [OSGi 구성의 예는 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM 게시

AEM 게시(및 미리보기) 서비스에서 CORS를 활성화하는 것은 AEM 작성자 서비스와 다릅니다. AEM Publish 서비스를 사용하려면 AEM Dispatcher 구성을 AEM Publish의 Dispatcher 구성에 추가해야 합니다. AEM 게시는 [OSGi 구성](#osgi-configuration).

AEM 게시에서 CORS를 구성할 때 다음을 확인하십시오.

+ 다음 `Origin` 다음을 제거하여 HTTP 요청 헤더를 AEM Publish 서비스로 보낼 수 없습니다. `Origin` AEM Dispatcher 프로젝트의 헤더(이전에 추가된 경우) `clientheaders.any` 파일. 임의 `Access-Control-` 헤더는에서 제거해야 합니다. `clientheaders.any` 파일 및 Dispatcher가 AEM 게시 서비스가 아닌 파일을 관리합니다.
+ 가지고 계신 것이 있으면 [CORS OSGi 구성](#osgi-configuration) aem Publish 서비스에서 이 프로그램을 제거하고 해당 구성을 [Dispatcher vhost 구성](#set-cors-headers-in-vhost) 아래에 요약되어 있습니다.

### Dispatcher 구성

AEM 게시(및 미리보기) 서비스의 Dispatcher가 CORS를 지원하도록 구성되어 있어야 합니다.

| 클라이언트가에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher CORS 구성 필요 | ✘ | ✔ | ✔ |

#### Dispatcher 환경 변수 설정

1. AEM Dispatcher 구성을 위한 global.vars 파일을 엽니다(일반적으로 위치). `dispatcher/src/conf.d/variables/global.vars`.
2. 다음 내용을 파일에 추가합니다.

   ```
   # Enable CORS handling in the dispatcher
   #
   # By default, CORS is handled by the AEM publish server.
   # If you uncomment and define the ENABLE_CORS variable, then CORS will be handled in the dispatcher.
   # See the default.vhost file for a suggested dispatcher configuration. Note that:
   #   a. You will need to adapt the regex from default.vhost to match your CORS domains
   #   b. Remove the "Origin" header (if it exists) from the clientheaders.any file
   #   c. If you have any CORS domains configured in your AEM publish server origin, you have to move those to the dispatcher
   #       (i.e. accordingly update regex in default.vhost to match those domains)
   #
   Define ENABLE_CORS
   ```

#### vhost에서 CORS 헤더 설정

1. Dispatcher 구성 프로젝트에서 AEM Publish 서비스에 대한 vhost 구성 파일을 엽니다(일반적으로 `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. 의 내용을 복사합니다. `<IfDefine ENABLE_CORS>...</IfDefine>` 을(를) 활성화한 vhost 구성 파일로 차단합니다.

   ```{ highlight="19"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
       ...
       <IfDefine ENABLE_CORS>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch '%{REQUEST_SCHEME}://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish
         RequestHeader unset Origin
   
         ################## End of CORS configuration ##################
       </IfDefine>
       ...
     </IfModule>
     ...
   </VirtualHost>
   ```

3. 아래 줄에서 정규 표현식을 업데이트하여 AEM Publish 서비스에 액세스하는 원하는 원본을 일치시킵니다. 여러 출처가 필요한 경우 이 라인을 복제하고 각 출처/출처 패턴에 대해 업데이트합니다.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + 예를 들어 출처에서 CORS 액세스를 활성화하려면 다음을 수행합니다.

      + 의 모든 하위 도메인 `https://example.com`
      + 의 모든 포트 `http://localhost`

     줄을 다음 두 줄로 바꿉니다.

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Dispatcher 구성 예

+ [Dispatcher 구성의 예는 WKND 프로젝트에서 찾을 수 있습니다.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
