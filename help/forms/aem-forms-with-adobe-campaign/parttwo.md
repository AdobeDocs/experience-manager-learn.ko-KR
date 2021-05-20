---
title: 적응형 양식 제출에서 캠페인 프로필 만들기
seo-title: 적응형 양식 제출에서 캠페인 프로필 만들기
description: 이 문서에서는 적응형 양식 제출 시 Adobe Campaign Standard에서 프로필을 만드는 데 필요한 단계에 대해 설명합니다. 이 프로세스에서는 사용자 지정 제출 메커니즘을 사용하여 적응형 양식 제출을 처리합니다.
seo-description: 이 문서에서는 적응형 양식 제출 시 Adobe Campaign Standard에서 프로필을 만드는 데 필요한 단계에 대해 설명합니다. 이 프로세스에서는 사용자 지정 제출 메커니즘을 사용하여 적응형 양식 제출을 처리합니다.
uuid: f3cb7b3c-1a1c-49eb-9447-a9e52c675244
feature: 적응형 Forms, 양식 데이터 모델
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 46ec4136-4898-4b01-86bb-ac638a29b242
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---


# 적응형 양식 제출 시 캠페인 프로필 만들기 {#creating-campaign-profile-on-adaptive-form-submission}

이 문서에서는 적응형 양식 제출 시 Adobe Campaign Standard에서 프로필을 만드는 데 필요한 단계에 대해 설명합니다. 이 프로세스에서는 사용자 지정 제출 메커니즘을 사용하여 적응형 양식 제출을 처리합니다.

이 튜토리얼에서는 적응형 양식 제출 시 Campaign 프로필을 만드는 단계를 설명합니다. 이 사용 사례를 수행하려면 다음을 수행해야 합니다

* REST API를 사용하여 Adobe Campaign Standard 프로필을 만들려면 AEM 서비스(CampaignService)를 만드십시오
* 적응형 양식 제출 처리를 위한 사용자 지정 제출 작업 만들기
* CampaignService의 createProfile 메서드 호출

## AEM 서비스 만들기 {#create-aem-service}

AEM 서비스를 만들어 Adobe Campaign 프로필을 만듭니다. 이 AEM 서비스는 OSGI 구성에서 Adobe Campaign 자격 증명을 가져옵니다. 캠페인 자격 증명을 획득하면 액세스 토큰이 생성되고 액세스 토큰 HTTP Post 호출이 수행되어 Adobe Campaign에서 프로필을 만듭니다. 다음은 프로필을 만들기 위한 코드입니다.

```java
package aemformwithcampaign.core.services.impl;

import static io.jsonwebtoken.SignatureAlgorithm.RS256;

import java.io.BufferedInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.KeySpec;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
import io.jsonwebtoken.Jwts;

@Component(service = CampaignService.class, immediate = true)
public class CampaignServiceImpl implements CampaignService {
private final Logger log = LoggerFactory.getLogger(getClass());

@Reference
CampaignConfigurationService config;
@Reference
GetResolver getResolver;
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String AUTH_SERVER_FQDN = "ims-na1.adobelogin.com";
private static final String AUTH_ENDPOINT = "/ims/exchange/jwt/";
private static final String CREATE_PROFILE_ENDPOINT = "/campaign/profileAndServicesExt/profile/";

@SuppressWarnings("unused")
@Override
public String getAccessToken() throws IOException, NoSuchAlgorithmException, InvalidKeySpecException {
// TODO Auto-generated method stub
log.info("JWT: Generating Token");

String apikey = config.getApiKey();
log.debug("The API Key i got was " + apikey);
String techact = config.getTechAcct();
String orgid = config.getOrgId();
String clientsecret = config.getClientSecret();
String realm = config.getDomainRealm();

HttpClient httpClient = HttpClientBuilder.create().build();
Long expirationTime = System.currentTimeMillis() / 1000 + 86400L;
try {
ResourceResolver rr = getResolver.getServiceResolver();
Resource privateKeyRes = rr.getResource("/etc/key/campaign/private.key");
InputStream is = privateKeyRes.adaptTo(InputStream.class);
BufferedInputStream bis = new BufferedInputStream(is);
ByteArrayOutputStream buf = new ByteArrayOutputStream();
int result = bis.read();
while (result != -1) {
byte b = (byte) result;
buf.write(b);
result = bis.read();
}

String privatekeyString = buf.toString();
privatekeyString = privatekeyString.replaceAll("\\n", "").replace("-----BEGIN PRIVATE KEY-----", "")
.replace("-----END PRIVATE KEY-----", "");
log.debug("The sanitized private key string is " + privatekeyString);
// Create the private key
KeyFactory keyFactory = KeyFactory.getInstance("RSA");
log.debug("The key factory algorithm is " + keyFactory.getAlgorithm());
byte[] byteArray = privatekeyString.getBytes();
log.debug("The array length is " + byteArray.length);
// KeySpec ks = new
// PKCS8EncodedKeySpec(Base64.getDecoder().decode(keyString));
byte[] encodedBytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(privatekeyString);
// KeySpec ks = new PKCS8EncodedKeySpec(byteArray);
KeySpec ks = new PKCS8EncodedKeySpec(encodedBytes);
String metascopes[] = new String[] { "ent_campaign_sdk" };
RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);
HashMap<String, Object> jwtClaims = new HashMap<>();
jwtClaims.put("iss", orgid);
jwtClaims.put("sub", techact);
jwtClaims.put("exp", expirationTime);
jwtClaims.put("aud", "https://" + AUTH_SERVER_FQDN + "/c/" + apikey);
// jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + realm,
// true);
for (String metascope : metascopes) {
jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + metascope, java.lang.Boolean.TRUE);
}

// Create the final JWT token
String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();
log.debug("#####The jwtToken is  ####" + jwtToken + "#######");
HttpHost authServer = new HttpHost(AUTH_SERVER_FQDN, 443, "https");
HttpPost authPostRequest = new HttpPost(AUTH_ENDPOINT);
authPostRequest.addHeader("Cache-Control", "no-cache");
List<NameValuePair> params = new ArrayList<NameValuePair>();
params.add(new BasicNameValuePair("client_id", apikey));
params.add(new BasicNameValuePair("client_secret", clientsecret));
params.add(new BasicNameValuePair("jwt_token", jwtToken));
authPostRequest.setEntity(new UrlEncodedFormEntity(params, Consts.UTF_8));
HttpResponse response = httpClient.execute(authServer, authPostRequest);
if (200 != response.getStatusLine().getStatusCode()) {
HttpEntity ent = response.getEntity();
String content = EntityUtils.toString(ent);
log.error("JWT: Server Returned Error\n", response.getStatusLine().getReasonPhrase());
log.error("ERROR DETAILS: \n", content);
throw new IOException("Server returned error: " + response.getStatusLine().getReasonPhrase());
}
HttpEntity entity = response.getEntity();
JsonObject jo = new JsonParser().parse(EntityUtils.toString(entity)).getAsJsonObject();

log.debug("Returning access_token " + jo.get("access_token").getAsString());
return jo.get("access_token").getAsString();


} catch (Exception e) {
e.printStackTrace();
}
return null;
}

@Override
public String createProfile(JsonObject profile) {
// TODO Auto-generated method stub
String jwtToken = null;
try {
jwtToken = getAccessToken();
} catch (NoSuchAlgorithmException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (InvalidKeySpecException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (IOException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
}
String tenant = config.getTenant();
String apikey = config.getApiKey();
String path = "/" + tenant + CREATE_PROFILE_ENDPOINT;
log.debug("The API Key is " + apikey);
log.debug("###The Path is " + path);
HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
HttpPost postReq = new HttpPost(path);
postReq.addHeader("Cache-Control", "no-cache");
postReq.addHeader("Content-Type", "application/json");
postReq.addHeader("X-Api-Key", apikey);
postReq.addHeader("Authorization", "Bearer " + jwtToken);
StringEntity se = null;
log.debug("Creating profile for" + profile.toString());
try {
se = new StringEntity(profile.toString());

} catch (UnsupportedEncodingException e1) {
// TODO Auto-generated catch block
e1.printStackTrace();
}
postReq.setEntity(se);
HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse result;
try {
result = httpClient.execute(server, postReq);
// JSONObject responseJson = new
// JSONObject(EntityUtils.toString(result.getEntity()));
JsonObject responseJson = new JsonParser().parse(EntityUtils.toString(result.getEntity()))
.getAsJsonObject();
log.debug("The response on creating profile is " + responseJson.toString());
return responseJson.get("PKey").getAsString();
// return responseJson.getString("PKey");

} catch (ClientProtocolException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

}
```

## 사용자 지정 제출 {#custom-submit}

적응형 양식 제출을 처리할 사용자 지정 제출 핸들러를 만듭니다. 이 사용자 지정 제출 처리기에서 CampaignService의 createProfile 메서드를 호출합니다. createProfile 메서드는 만들어야 하는 프로필을 나타내는 JSONObject를 허용합니다.

AEM Forms의 사용자 지정 제출 처리기에 대한 자세한 내용은 이 [link](/help/forms/adaptive-forms/custom-submit-aem-forms-article.md)을 참조하십시오

다음은 사용자 지정 제출의 코드입니다

```java
aemforms.campaign.core.CampaignService addNewProfile = sling.getService(aemforms.campaign.core.CampaignService.class);
com.google.gson.JsonObject profile = new com.google.gson.JsonObject();
profile.addProperty("email",request.getParameter("email"));
profile.addProperty("firstName",request.getParameter("fname"));
profile.addProperty("lastName",request.getParameter("lname"));
profile.addProperty("mobilePhone",request.getParameter("phone"));

String pkey = addNewProfile.createProfile(profile);
```

## 솔루션 {#test-the-solution} 테스트

서비스와 사용자 지정 제출 작업을 정의했으면 솔루션을 테스트할 준비가 되었습니다. 솔루션을 테스트하려면 다음 단계를 수행하십시오


* [여기에 설명된 대로 단계를 따랐는지 확인합니다](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [패키지 관리자를 사용하여 적응형 양식 및 사용자 지정 제출 처리기 가져오기](assets/create-acs-profile-on-af-submission.zip). 이 패키지에는 사용자 지정 제출 작업에 제출하도록 구성된 적응형 양식이 포함되어 있습니다.
* [양식](http://localhost:4502/content/dam/formsanddocuments/createcampaignprofile/jcr:content?wcmmode=disabled) 미리 보기
* 모든 필드를 입력하고 제출
* ACS 인스턴스에서 새 프로필이 만들어집니다
