---
title: API를 사용하여 대화형 통신 문서 생성
description: React 앱의 데이터를 병합하여 대화형 통신 문서를 생성합니다
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: d97ff584-7fa0-48bc-9b83-ba45c26b7d87
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 1%

---

# 대화형 통신 문서 생성

API를 사용하여 대화형 통신 문서를 생성하려면 다음을 수행해야 합니다

* 미리 채우기 서비스 만들기
* 대화형 통신 문서 생성

서비스 이름 `ccm-print-test` 이 서비스를 액세스하는 데 사용됩니다. 이 사전 채우기 서비스가 정의되면 서블릿 또는 워크플로우 프로세스 단계 구현에서 이 서비스에 액세스하여 인쇄 채널 문서를 생성할 수 있습니다.

```java
package com.acrobatsign.core;

import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {
  DataProvider.class
})
public class PrintChannelPrefill implements DataProvider {

  @Override
  public String getServiceDescription() {

    return "Prefill Service for IC Print Channel";
  }

  @Override
  public String getServiceName() {

    return "ccm-print-test";
  }

  @Override
  public PrefillData getPrefillData(DataOptions options) throws FormsException {

    PrefillData data = null;
    if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
      InputStream is = (InputStream) options.getExtras().get("data");
      data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
    }
    return data;
  }
}
```

## 문서 생성

```java
package com.acrobatsign.core.impl;

import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.concurrent.Callable;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.acrobatsign.core.AcrobatSignHelperMethods;
import com.acrobatsign.core.PrintChannelDocumentHelperMethods;
import com.adobe.fd.ccm.channels.print.api.model.PrintChannel;
import com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions;
import com.adobe.fd.ccm.channels.print.api.service.PrintChannelService;
import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.granite.resourceresolverhelper.ResourceResolverHelper;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = PrintChannelDocumentHelperMethods.class, immediate = true)
public class PrintChannelDocumentHelperMethodsImpl implements PrintChannelDocumentHelperMethods {
  @Reference
  GetResolver getResolver;
  @Reference
  ResourceResolverHelper resHelper;
  @Reference
  PrintChannelService printChannelService;
  @Reference
  AcrobatSignHelperMethods acrobatSignHelperMethods;

  @Override
  public String getPrintChannelDocument(String templateName, InputStream dataStream) throws Exception {
    return resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable < String > () {

      @Override
      public String call() throws Exception {
        PrintChannel printChannel = null;
        printChannel = printChannelService.getPrintChannel(templateName);
        PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
        options.setMergeDataOnServer(true);
        options.setRenderInteractive(false);
        DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
        //dataOptions.setServiceName(printChannel.getPrefillService());
        // dataOptions.setExtras(map);
        dataOptions.setContentType(ContentType.JSON);

        dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(templateName));
        dataOptions.setServiceName("ccm-print-test");
        dataOptions.setExtras(new HashMap < String, Object > ());
        dataOptions.getExtras().put("data", dataStream);
        options.setDataOptions(dataOptions);

        com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel.render(options);
        com.adobe.aemfd.docmanager.Document icDocument = new com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());

        String widgetId = acrobatSignHelperMethods.getWidgetID(acrobatSignHelperMethods.getTransientDocumentID(icDocument));

        return acrobatSignHelperMethods.getWidgetURL(widgetId);

      }

    });

  }

}
```java

A custom AEM bundle was developed to create and return the web form to the end user

* [Create Transient Document](https://secure.na1.echosign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument). The document uploaded through this call is referred to as transient since it is available only for 7 days after the upload. The returned transient document ID can be used in the API calls where the uploaded file needs to be referred. The transient document request is a multipart request consisting of three parts - filename, mime type and the file stream. You can only upload one file at a time in this request.
* [Create web form](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/createWidget).This is a primary endpoint which is used to create a new web form. The web form was created in an ACTIVE state to immediately host the web form.
* [Retrieve the web form](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/getWidgets).Retrieve's web form the user. This web form is then presented to the calling application for signing the document.

## Create Acrobat Sign OSGi configuration

Acrobat Sign REST API require the integration key and email associated with the integration key. These two values are provided as an OSGi configuration properties as shown below


```java
package com.acrobatsign.core.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;



@ObjectClassDefinition(name="Acrobat Sign Configuration", description = "Acrobat SignConfiguration")
public @interface AcrobatSignConfiguration
{
    @AttributeDefinition(name="Acrobat Sign Integration Key", description = "Integration key you created in Acrobat Sign ")
        String getIntegrationKey();
    
    @AttributeDefinition(name="X-API-USER", description = "X-API-USER")
    String getApiUser();


}
```

```java
package com.acrobatsign.core.configuration;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(immediate = true, service = AcrobatSignConfigurationService.class)
@Designate(ocd = AcrobatSignConfiguration.class)
public class AcrobatSignConfigurationService {

  private String IntegrationKey;
  private String apiUserEmail;
  public String getIntegrationKey() {
    return IntegrationKey;
  }
  public String getApiUserEmail() {
    return apiUserEmail;
  }

  @Activate
  protected final void activate(AcrobatSignConfiguration config) {
    IntegrationKey = config.getIntegrationKey();

    apiUserEmail = config.getApiUser();
  }

}
```

## 임시 문서 ID 가져오기

임시 문서를 만들기 위해 다음 코드가 작성됩니다

```java
public String getTransientDocumentID(Document documentForSigning) throws IOException {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com/api/rest/v6/transientDocuments";
  MultipartEntityBuilder builder = MultipartEntityBuilder.create();
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);
  builder.addBinaryBody("File", documentForSigning.getInputStream(), ContentType.DEFAULT_BINARY, "NDA.PDF");
  builder.addTextBody("File-Name", "NDA.pdf");
  HttpEntity entity = builder.build();
  log.debug("Build the entity");
  httpPost.setEntity(entity);
  CloseableHttpResponse response = httpClient.execute(httpPost);
  log.debug("Sent the request!!!!");
  log.debug("REsponse code " + response.getStatusLine().getStatusCode());
  HttpEntity httpEntity = response.getEntity();
  String transientDocumentId = JsonParser.parseString(EntityUtils.toString(httpEntity)).getAsJsonObject().get("transientDocumentId").getAsString();
  log.debug("Transient ID  " + transientDocumentId);
  return transientDocumentId;

}
```

## 위젯 ID 가져오기

```java
public String getWidgetID(String transientDocumentID) {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);

  String jsonREquest = "{\r\n" +
    "  \"fileInfos\": [\r\n" +
    "    {\r\n" +
    "      \"transientDocumentId\": \"a\"\r\n" +
    "    }\r\n" +
    "  ],\r\n" +
    "  \"name\": \"Release and Waiver Agreement\",\r\n" +
    "  \"state\": \"ACTIVE\",\r\n" +
    "  \"widgetParticipantSetInfo\": {\r\n" +
    "    \"memberInfos\": [\r\n" +
    "      {\r\n" +
    "        \"email\": \"\"\r\n" +
    "      }\r\n" +
    "    ],\r\n" +
    "    \"role\": \"SIGNER\"\r\n" +
    "  }\r\n" +
    "}";
  JsonObject jsonReq = JsonParser.parseString(jsonREquest).getAsJsonObject();
  jsonReq.getAsJsonArray("fileInfos").get(0).getAsJsonObject().addProperty("transientDocumentId", transientDocumentID);
  log.debug("The updated json object is " + jsonReq);
  try {
    StringEntity stringEntity = new StringEntity(jsonReq.toString());
    httpPost.setEntity(stringEntity);
    httpPost.addHeader("Content-Type", "application/json");
    CloseableHttpResponse response = httpClient.execute(httpPost);
    log.debug("The response is  " + response.getStatusLine().getStatusCode());
    String widgetID = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject().get("id").getAsString();
    log.debug("The widget id is " + widgetID);
    return widgetID;
  } catch (Exception e) {
    log.debug("Error in getting Widget ID:" + e.getMessage());

  }
  return null;
}
```

## 위젯 URL 가져오기

```java
public String getWidgetURL(String widgetId) throws ClientProtocolException, IOException {
        
        log.debug("$$$$ in get Widget URL for "+widgetId+ "widget id");
        String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        HttpGet httpGet = new HttpGet(url);
        httpGet.addHeader("x-api-user", "email:"+acrobatSignConfig.getApiUserEmail());
        httpGet.addHeader("Authorization", "Bearer "+acrobatSignConfig.getIntegrationKey());
        CloseableHttpResponse response = httpClient.execute(httpGet);
        JsonObject jsonResponse = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject();
         log.debug("The json response from get widgets is "+jsonResponse.toString());
         JsonArray userWidgetList = jsonResponse.get("userWidgetList").getAsJsonArray();
         log.debug("The array size is "+userWidgetList.size());
          for(int i=0;i<userWidgetList.size();i++)
         {
        
             log.debug("Getting widget object "+i);
             JsonObject temp = userWidgetList.get(i).getAsJsonObject();
             log.debug("The widget object "+i+"is "+temp.toString());
             if(temp.get("id").getAsString().equalsIgnoreCase(widgetId))
             {
                 log.debug("Bingo found the matching widget id  "+i);
                 String widgetURL = temp.get("url").getAsString();
                 return widgetURL;
                 
             }
            
         }
        
        return null;
        
    }
```

## 다음 단계

쓰기 [Acrobat Sign API를 노출하는 OSGi 서비스 래퍼](./wrapper-sign-api.md)