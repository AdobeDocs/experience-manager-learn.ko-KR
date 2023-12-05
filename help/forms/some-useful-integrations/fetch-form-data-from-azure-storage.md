---
title: Azure Storage에서 양식 제출 저장
description: REST API를 사용하여 Azure Storage에 양식 데이터 저장
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 108
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Azure 스토리지에서 데이터 가져오기

이 문서에서는 적응형 양식을 Azure 스토리지에 저장된 데이터로 채우는 방법을 보여 줍니다.
적응형 양식 데이터를 Azure 스토리지에 저장했으며 이제 적응형 양식을 해당 데이터로 미리 채우려고 한다고 가정합니다.

## GET 요청 만들기

다음 단계는 blobID를 사용하여 Azure 저장소에서 데이터를 가져오는 코드를 작성하는 것입니다. 데이터를 가져오기 위해 다음 코드를 작성했습니다. URL은 OSGi 구성의 sasToken 및 storageURI 값과 getBlobData 함수에 전달된 blobID를 사용하여 생성되었습니다

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

적응형 양식이 URL에서 guid 매개 변수로 렌더링되면 템플릿과 연결된 사용자 지정 페이지 구성 요소가 적응형 양식을 가져와서 Azure 스토리지의 데이터로 채웁니다.
다음은 템플릿과 연관된 페이지 구성 요소의 jsp에 있는 코드입니다

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## 솔루션 테스트

* [사용자 지정 OSGi 번들 배포](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [사용자 지정 적응형 양식 템플릿 및 템플릿과 연결된 페이지 구성 요소 가져오기](./assets/store-and-fetch-from-azure.zip)

* [샘플 적응형 양식 가져오기](./assets/bank-account-sample-form.zip)

* OSGi 구성 콘솔을 사용하여 Azure 포털 구성에서 적절한 값을 지정합니다.

* [BankAccount 양식 미리 보기 및 제출](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* 선택한 Azure 스토리지 컨테이너에 데이터가 저장되었는지 확인합니다. Blob ID를 복사합니다.

* [BankAccount 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) 및 Azure 스토리지의 데이터로 미리 채워질 양식의 URL에서 Blob ID를 guid 매개 변수로 지정합니다

