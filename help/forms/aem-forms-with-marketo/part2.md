---
title: AEM Forms과 Marketo(2부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 자습서입니다.
feature: 적응형 Forms, 양식 데이터 모델
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 1%

---


# Marketo 인증 서비스

Marketo의 REST API는 2개의 다리를 가진 OAuth 2.0으로 인증됩니다. Marketo에 대해 인증하려면 사용자 지정 인증을 만들어야 합니다. 이 사용자 지정 인증은 일반적으로 OSGI 번들 내에 작성됩니다. 다음 코드는 이 자습서의 일부로 사용된 사용자 지정 인증자를 보여줍니다.

## 사용자 지정 인증 서비스

다음 코드는 Marketo에 대한 인증에 필요한 access_token 이 있는 AuthenticationDetails 개체를 만듭니다

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

MarketoAuthenticationService는 IAuthentication 인터페이스를 구현합니다. 이 인터페이스는 AEM Forms 클라이언트 SDK의 일부입니다. 이 서비스는 액세스 토큰을 가져오고 토큰을 AuthenticationDetails의 HttpHeader에 삽입합니다. AuthenticationDetails 개체의 HttpHeaders를 채우면 AuthenticationDetails 개체가 양식 데이터 모델의 Dermis 레이어에 반환됩니다.

getAuthenticationType 메서드에서 반환된 문자열에 주의하십시오. 이 문자열은 데이터 소스를 구성할 때 사용됩니다.

### 액세스 토큰 가져오기

단순 인터페이스는 access_token 을 반환하는 한 가지 메서드로 정의됩니다. 이 인터페이스를 구현하는 클래스의 코드가 페이지 아래쪽에 나열됩니다.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

다음 코드는 REST API 호출 시 사용할 access_token 을 반환하는 서비스의 코드입니다. 이 서비스의 코드는 GET 호출을 수행하는 데 필요한 구성 매개 변수에 액세스합니다. 보시다시피 GET URL에 client_id,client_secret을 전달하여 access_token을 생성합니다. 그런 다음 이 access_token 이 호출 응용 프로그램으로 반환됩니다.

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

아래 스크린샷은 설정해야 하는 구성 속성을 보여줍니다. 이러한 구성 속성은 access_token을 가져오려면 위에 나열된 코드에서 읽습니다

![config](assets/marketoconfig.jfif)

### 구성

구성 속성을 만드는 데 다음 코드가 사용되었습니다. 이러한 속성은 Marketo 인스턴스에만 적용됩니다

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

다음 코드는 구성 속성을 읽고 getter 메서드를 통해 동일하게 반환합니다

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

1. 번들을 작성하고 AEM 서버에 배포합니다.
1. [브라우저를 ](http://localhost:4502/system/console/configMgr) &quot;Marketo 자격 증명 서비스 구성&quot;에 대한 configMgrand 검색을 가리킵니다.
1. Marketo 인스턴스에 대한 적절한 속성을 지정합니다
