---
title: JWT를 액세스 토큰으로 교환
description: JSON 웹 토큰(JWT)을 AEM 액세스 토큰용 Adobe IMS API와 교환합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: KT-8185.jpg
exl-id: ab7b8a06-3009-477d-9e98-590912e8e176
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# JWT를 액세스 토큰으로 교환


AEM 이전 단계에서 생성된 JWT는 액세스 토큰용 Adobe IMS API와 교환되며, as a Cloud Service으로 액세스하는 데 사용할 수 있습니다. 액세스 토큰을 요청하려면 JWT, client_id, client_secret이 포함된 POST 요청을 IMS 인증 서비스로 전송합니다.

다음 코드는 액세스 토큰에 대한 교환 JWT를 생성하는 데 사용되었습니다

```java
public String getAccessToken() {
        
        String jwtToken = getJWTToken();
        GetServiceCredentials getCredentials = new GetServiceCredentials();
        System.out.println("Getting Access Token");

        try {
                HttpClient httpClient = HttpClientBuilder.create().build();
                HttpHost authServer = new HttpHost(getCredentials.getIMS_ENDPOINT(), 443, "https");
                HttpPost authPostRequest = new HttpPost("/ims/exchange/jwt");
                List < NameValuePair > nameValuePairs = new ArrayList < NameValuePair > ();
                nameValuePairs.add(new BasicNameValuePair("jwt_token", jwtToken));
                nameValuePairs.add(new BasicNameValuePair("client_id", getCredentials.getCLIENT_ID()));
                nameValuePairs.add(new BasicNameValuePair("client_secret", getCredentials.getCLIENT_SECRET()));
                authPostRequest.setEntity(new UrlEncodedFormEntity(nameValuePairs, Consts.UTF_8));
                HttpResponse response;
                response = httpClient.execute(authServer, authPostRequest);
                StatusLine statusLine = response.getStatusLine();
                System.out.println("The status code is " + statusLine.getStatusCode());
                HttpEntity result = response.getEntity();
                String jsonResponseStr = EntityUtils.toString(result);
                System.out.println(jsonResponseStr);
                JsonReader jsonReader = new JsonReader(new StringReader(jsonResponseStr));
                JsonObject jsonObject = JsonParser.parseReader(jsonReader).getAsJsonObject();
                
                System.out.println("Returning access_token " + jsonObject.get("access_token").getAsString());
                return jsonObject.get("access_token").getAsString();

        } catch (Exception e) {
                System.out.print("Error: " + e.getMessage());
        }
        return "null";

}
```
