---
title: AEM Forms(Marketing 2부)
seo-title: AEM Forms(Marketing 2부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketing과 통합하는 자습서입니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketing과 통합하는 자습서입니다.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# Marketing To 인증 서비스

Marketing의 REST API는 다리 2개 OAuth 2.0에서 인증됩니다. Marketing에 대해 인증하려면 사용자 정의 인증을 만들어야 합니다. 이 사용자 정의 인증은 일반적으로 OSGI 번들 내에서 작성됩니다. 다음 코드는 이 자습서의 일부로 사용된 사용자 정의 인증자를 보여줍니다.

## 사용자 정의 인증 서비스

다음 코드는 Marketing에 대한 인증에 필요한 access_token을 가진 AuthenticationDetails 개체를 만듭니다

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketingAuthenticationService는 IAuthentication 인터페이스를 구현합니다. 이 인터페이스는 AEM Forms 클라이언트 SDK의 일부입니다. 서비스는 액세스 토큰을 가져와 AuthenticationDetails의 HttpHeader에 토큰을 삽입합니다. AuthenticationDetails 개체의 HttpHeaders가 채워지면 AuthenticationDetails 개체가 양식 데이터 모델의 Dermis 레이어로 반환됩니다.

getAuthenticationType 메서드에서 반환되는 문자열에 주의하십시오. 이 문자열은 데이터 소스를 구성할 때 사용됩니다.

### 액세스 토큰 가져오기

단순 인터페이스는 access_token을 반환하는 한 가지 방법으로 정의됩니다. 이 인터페이스를 구현하는 클래스에 대한 코드가 페이지 아래에 더 나열됩니다.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

다음 코드는 REST API 호출을 수행하는 데 사용할 access_token을 반환하는 서비스의 코드입니다. 이 서비스의 코드는 GET 호출을 수행하는 데 필요한 구성 매개 변수에 액세스합니다. 보시다시피 GET URL에 client_id,client_secret을 전달하여 access_token을 생성합니다. 그러면 이 access_token이 호출 응용 프로그램으로 돌아갑니다.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

아래 스크린샷은 설정해야 하는 구성 속성을 보여줍니다. 이러한 구성 속성은 위에 나열된 코드에서 access_token을 가져오기 위해 읽습니다

![config](assets/marketoconfig.jfif)

### 구성

구성 속성을 만드는 데 다음 코드가 사용되었습니다. 이러한 속성은 마케팅 인스턴스에만 적용됩니다

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

다음 코드는 구성 속성을 읽고 getter 메서드를 통해 동일한 값을 반환합니다

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. 번들을 구축하여 AEM 서버에 배포합니다.
1. [브라우저에서 &quot;Marketing To 자격 증명 서비스 구성&quot;을 ](http://localhost:4502/system/console/configMgr) configMgrand로 검색하십시오.
1. 마케팅 인스턴스와 관련된 적절한 속성 지정
