---
title: 사용자 지정 메타데이터 태그를 추가하는 자습서
description: Azure에서 메타데이터 태그가 있는 양식 데이터를 저장하려면 사용자 지정 제출을 만드세요.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 14501
source-git-commit: b770fc33ee0752911135d1a94144406bad8f295b
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---

# 양식 제출을 처리할 사용자 정의 제출 만들기

AEM Forms CS는 양식 데이터를 Azure에 저장하는 즉시 사용 가능한 제출 액션을 제공하지만, blob에 blob 인덱스 태그를 만들 수 있는 기능은 없습니다. 사용 사례를 충족하기 위해 제출된 데이터를 Azure에 저장하고 양식에서 검색 가능한 것으로 표시된 필드를 사용하여 Blob 인덱스 데이터 태그를 만드는 사용자 지정 제출 서비스를 만들었습니다.

[핵심 구성 요소 기반 적응형 양식에 대한 샘플 사용자 지정 제출 처리기는 여기에서 사용할 수 있습니다](https://github.com/adobe/aem-core-forms-components/blob/master/it/core/src/main/java/com/adobe/cq/forms/core/components/it/service/CustomAFSubmitService.java#L56). 양식 제출을 처리하기 위해 다음과 같은 사용자 정의 제출을 작성했습니다

```java
package com.aemforms.saveandfecthfromazure.prefill;

import com.adobe.aemds.guide.common.GuideValidationResult;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.adobe.forms.common.service.FileAttachmentWrapper;
import com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage;
import org.apache.commons.lang3.StringUtils;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.UUID;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class CustomAFSubmitService implements FormSubmitActionService {
    private static final String serviceName = "Store Form Submission with Metadata tags in Azure";
    private static Logger logger = LoggerFactory.getLogger(CustomAFSubmitService.class);

    @Reference
    DataManager dataManager;
    @Reference
    StoreAndFetchDataFromAzureStorage storeAndFetchDataFromAzureStorage;

    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        Map<String, Object> result = new HashMap<>();
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.FALSE);
        try {

            String guideContainerPath = formSubmitInfo.getFormContainerPath();

            String submittedFormName = formSubmitInfo.getFormContainerResource().getParent().getParent().getName();
            logger.debug("The submitted form name is "+submittedFormName);
            String data = formSubmitInfo.getData();
            logger.debug(data);
            String metadataTags = storeAndFetchDataFromAzureStorage.getMetaDataTags(submittedFormName,formSubmitInfo.getFormContainerPath(),formSubmitInfo.getFormContainerResource().getResourceResolver().adaptTo(Session.class),data);
            String uniqueID = UUID.randomUUID().toString();
            logger.debug("The metadata tags are  "+metadataTags);
            // The code for saveFormDatainAzure is listed in this article
            logger.debug(storeAndFetchDataFromAzureStorage.saveFormDatainAzure(data,metadataTags));
            
            if(dataManager != null) {
                dataManager.put(uniqueID, data);
               
            }
            logger.info("AF Submission successful using custom submit service for: {}", guideContainerPath);
            result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
            result.put(DataManager.UNIQUE_ID, uniqueID);
            // adding id here so that this available in redirect parameters in final thank you page
            Map<String, Object> redirectParamMap = new HashMap<String, Object>() {{
                put(DataManager.UNIQUE_ID, uniqueID);
            }};
            // todo: move this to constant, once forms SDK is released
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

## 서비스 구현 saveFormDataAzure

```java
    @Override
    public String saveFormDatainAzure(String formData,String metaDataTags) {
        String sasToken = azurePortalConfigurationService.getSASToken();
        String storageURI = azurePortalConfigurationService.getStorageURI();
        logger.debug("The SAS Token is "+sasToken);
        logger.debug("The Storage URL is "+storageURI);
        logger.debug("The form data is "+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        String putRequestURL = storageURI+uuid.toString();
        putRequestURL = putRequestURL+sasToken;
        logger.debug("The put Request "+putRequestURL);
        HttpPut httpPut = new HttpPut(putRequestURL);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        httpPut.addHeader("x-ms-tags",metaDataTags);

        try
        {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            logger.debug("Response code "+response.getStatusLine().getStatusCode());
            if(response.getStatusLine().getStatusCode() == 201)
            {
                return uuid.toString();
            }
        }
        catch (IOException e)
        {
            logger.debug("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return null;
    }
```

