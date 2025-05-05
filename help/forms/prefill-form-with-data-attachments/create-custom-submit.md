---
title: 사용자 정의 제출을 만들어 핵심 구성 요소 기반 양식 제출을 처리합니다.
description: Azure에 첨부 파일이 있는 양식 데이터를 저장하기 위한 사용자 지정 제출 만들기
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager 6.5
topic: Integrations
jira: KT-14794
exl-id: c4257567-76bd-417e-a1a2-38a0647767d9
duration: 147
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 1%

---

# 양식 제출을 처리할 사용자 정의 제출 만들기

사용 사례를 충족하기 위해 제출된 데이터 및 첨부 파일을 Azure에 저장하는 사용자 지정 제출 서비스를 만들었습니다. 핵심 구성 요소 기반 양식을 제출하면 데이터는 다음 형식을 갖습니다

```json
{
    "Name": "Gloria Rios",
    "EmailID": "grios@email.com",
    "ExistingCustomer": [
        "Yes"
    ],
    "PhoneNumber": "1234567890",
    "Solution": "Forms",
    "contractcopy": {
        "name": "annualcontract.pdf",
        "size": 80194,
        "mediaType": "application/pdf",
        "data": "[object File]"
    },
    "Message": "We would like to renew our annual contract "
}
```

_&#x200B;**contractcopy**&#x200B;_ 요소는 첨부 파일 구성 요소를 나타내며 양식과 함께 제출된 첨부 파일을 캡처하는 데 사용됩니다.
적응형 양식에 데이터 및 해당 첨부 파일을 미리 채울 수 있도록 제출된 첨부 파일이 Azure 포털에 저장되고 제출된 데이터에 있는 contractcopy 개체의 데이터 요소가 저장된 첨부 파일의 URL로 업데이트됩니다.
사용자 지정 제출 서비스는 Azure 포털에서 첨부 파일을 추출하여 저장합니다.  업데이트된 제출 데이터는 다음과 같이 표시됩니다


```json
{
    "Name": "Gloria Rios",
    "EmailID": "grios@email.com",
    "ExistingCustomer": [
        "Yes"
    ],
    "PhoneNumber": "1234567890",
    "Solution": "Forms",
    "contractcopy": {
        "name": "annualcontract.pdf",
        "size": 80194,
        "mediaType": "application/pdf",
        "data": "https://aemformstutorial.blob.core.windows.net/benefiitsenrollment/69d5a2?sv=2022-11-02&ss=bfqt&srt=KiHs0%3D"
    },
    "Message": "We would like to renew our annual contract "
}
``
```


[핵심 구성 요소 기반 적응형 양식에 대한 샘플 사용자 지정 제출 처리기는 여기에서 사용할 수 있습니다](https://github.com/adobe/aem-core-forms-components/blob/master/it/core/src/main/java/com/adobe/cq/forms/core/components/it/service/CustomAFSubmitService.java#L56). 양식 제출을 처리하기 위해 다음과 같은 사용자 정의 제출을 작성했습니다

```java
package com.azuredemo.core;
import com.adobe.aemds.guide.common.GuideValidationResult;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.adobe.forms.common.service.FileAttachmentWrapper;
import com.azuredemo.core.SaveAndFetchFromAzure.StoreAndFetchDataFromAzureStorage;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

@Component(
    service = FormSubmitActionService.class,
    immediate = true
)
public class StoreFormDataWithAttachments implements FormSubmitActionService {
    private static final String serviceName = "Store Submitted Form Data in Azure";
    private static transient Logger logger = LoggerFactory.getLogger(StoreFormDataWithAttachments.class);

    @Reference
    DataManager dataManager;
    @Reference
    StoreAndFetchDataFromAzureStorage storeAndFetchDataFromAzureStorage;

    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map < String, Object > submit(FormSubmitInfo formSubmitInfo) {
        logger.debug("@@@@in my custom submit!!!");
        Map < String, Object > result = new HashMap < > ();
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.FALSE);
        try {

            String guideContainerPath = formSubmitInfo.getFormContainerPath();

            String submittedFormName = formSubmitInfo.getFormContainerResource().getParent().getParent().getName();
            logger.debug("The submitted form name is " + submittedFormName);
            String data = formSubmitInfo.getData();
            logger.debug("The submitted data is " + data);
            JsonObject jsonData = new Gson().fromJson(data, JsonObject.class);
            if (formSubmitInfo.getFileAttachments() != null && formSubmitInfo.getFileAttachments().size() > 0) {
                logger.debug("#####The form submitted has file attachments");
                for (int i = 0; i < formSubmitInfo.getFileAttachments().size(); i++) {
                    FileAttachmentWrapper fileAttachment = formSubmitInfo.getFileAttachments().get(i);

                    logger.debug("The url of the file attachment is " + fileAttachment.getUri());
                    logger.debug("The data ref of the file attachment is " + fileAttachment.getDataRef());
                    String fileName = fileAttachment.getFileName();
                    logger.debug("The fileName is  " + fileName);
                    logger.debug("#### Writing  " + fileName + " to azure");


                    String dataUrl = storeAndFetchDataFromAzureStorage.saveFormAttachmentinAzure(fileAttachment.getInputStream(), fileName, fileAttachment.getContentType());
                    jsonData.getAsJsonObject(fileAttachment.getDataRef()).addProperty("data", dataUrl);
                    logger.debug(jsonData.toString());

                }
            }


            String uniqueID = UUID.randomUUID().toString();

            logger.debug(storeAndFetchDataFromAzureStorage.saveFormDatainAzure(jsonData.toString(), "text/plain"));

            if (dataManager != null) {
                dataManager.put(uniqueID, data);

            }
            logger.info("AF Submission successful using custom submit service for: {}", guideContainerPath);
            result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
            result.put(DataManager.UNIQUE_ID, uniqueID);
            // adding id here so that this available in redirect parameters in final thank you page
            Map < String, Object > redirectParamMap = new HashMap < String, Object > () {
                {
                    put(DataManager.UNIQUE_ID, uniqueID);
                }
            };
            
            result.put("fd:redirectParameters", redirectParamMap);
        } catch (Exception ex) {
            logger.error("Error while using the AF Submit service", ex);
            GuideValidationResult guideValidationResult = new GuideValidationResult();
            guideValidationResult.setOriginCode("500");
            guideValidationResult.setErrorMessage("Internal server error");
            result.put(GuideConstants.FORM_SUBMISSION_ERROR, guideValidationResult);
        }
        return result;
    }

}
```

## Azure에서 양식 첨부 파일 저장

```java
@Override
// Save the submitted form attachment in azure and return its url
public String saveFormAttachmentinAzure(InputStream attachmentStream, String fileName, String contentType) {
    logger.debug("Saving " + fileName + "  form attachment in azure  ");
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    logger.debug("The SAS Token is " + sasToken);
    logger.debug("The Storage URL is " + storageURI);

    int timeout = 5;
    RequestConfig config = RequestConfig.custom()
        .setConnectTimeout(timeout * 1000)
        .setConnectionRequestTimeout(timeout)
        .setSocketTimeout(timeout * 1000).build();
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().setDefaultRequestConfig(config).build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    System.out.println("The put Request " + putRequestURL);
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");

    try {

        InputStreamEntity ies = new InputStreamEntity(attachmentStream, attachmentStream.available());

        httpPut.setEntity(ies);

        CloseableHttpResponse response = httpClient.execute(httpPut);
        logger.debug("Response code    " + response.getStatusLine().getStatusCode());
        logger.debug("Added file attachment  " + response.getStatusLine().getStatusCode() + "  uuid " + uuid.toString() + "   " + response.getStatusLine().getReasonPhrase());
        if (response.getStatusLine().getStatusCode() == 201) {
            return putRequestURL;
        }
    } catch (Exception e) {

        logger.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;
}
```

## Azure에서 양식 데이터 저장

```java
@Override
 public String saveFormDatainAzure(String formData, String contentType) {
     String sasToken = azurePortalConfigurationService.getSASToken();
     String storageURI = azurePortalConfigurationService.getStorageURI();
     logger.debug("The SAS Token is " + sasToken);
     logger.debug("The Storage URL is " + storageURI);
     logger.debug("The form data is " + formData);
     int timeout = 5;
     RequestConfig config = RequestConfig.custom()
         .setConnectTimeout(timeout * 1000)
         .setConnectionRequestTimeout(timeout)
         .setSocketTimeout(timeout * 1000).build();
     org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().setDefaultRequestConfig(config).build();
     UUID uuid = UUID.randomUUID();
     String putRequestURL = storageURI + uuid.toString();
     putRequestURL = putRequestURL + sasToken;
     logger.debug("The put Request " + putRequestURL);
     HttpPut httpPut = new HttpPut(putRequestURL);
     httpPut.addHeader("x-ms-blob-type", "BlockBlob");
     httpPut.addHeader("Content-Type", contentType);
     try {
         httpPut.setEntity(new StringEntity(formData));
         CloseableHttpResponse response = httpClient.execute(httpPut);
         logger.debug("Response code " + response.getStatusLine().getStatusCode());
         System.out.println("The response code is " + response.getStatusLine().getStatusCode() + "uuid " + uuid.toString());
         if (response.getStatusLine().getStatusCode() == 201) {
             return uuid.toString();
         }
     } catch (IOException e) {
         logger.error("Error: " + e.getMessage());
         throw new RuntimeException(e);
     }
     return null;

 }
```

## 다음 단계

[OSGi 구성 작성](./create-osgi-configuration.md)
