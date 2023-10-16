---
title: Azure Storage에서 양식 제출 저장
description: REST API를 사용하여 Azure Storage에 양식 데이터 저장
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Azure Storage에 양식 제출 저장

이 문서에서는 제출된 AEM Forms 데이터를 Azure Storage에 저장하기 위해 REST를 호출하는 방법을 보여 줍니다.
제출된 양식 데이터를 Azure Storage에 저장하려면 다음 단계를 수행해야 합니다.

## Azure 스토리지 계정 만들기

[Azure 포털 계정에 로그인하고 저장소 계정을 만듭니다.](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). 저장소 계정에 의미 있는 이름을 입력하고 검토 를 클릭한 다음 만들기 를 클릭합니다. 이렇게 하면 모든 기본값으로 저장소 계정이 만들어집니다. 이 문서에서는 당사의 스토리지 계정을 명시했습니다 `aemformstutorial`.

## 공유 액세스 권한 만들기

Azure 스토리지 컨테이너와 상호 작용할 수 있는 인증 방법인 공유 액세스 서명 또는 SAS 방법을 사용합니다.
포털의 저장소 계정 페이지에서 왼쪽의 공유 액세스 서명 메뉴 항목을 클릭하여 새 공유 액세스 서명 키 설정 페이지를 엽니다. 아래 스크린샷에 표시된 대로 설정과 적절한 종료 날짜를 지정하고 SAS 및 연결 문자열 생성 버튼을 클릭하십시오. Blob 서비스 SAS URL을 복사합니다. 이 URL을 사용하여 HTTP 호출을 수행합니다
![공유 액세스 키](./assets/shared-access-signature.png)

## 컨테이너 만들기

다음으로 양식 제출의 데이터를 저장할 컨테이너를 만들어야 합니다.
저장소 계정 페이지에서 왼쪽의 컨테이너 메뉴 항목을 클릭하고 라는 컨테이너를 만듭니다. `formssubmissions`. 공개 액세스 수준이 비공개로 설정되어 있는지 확인하십시오.
![컨테이너](./assets/new-container.png)

## PUT 요청 만들기

다음 단계는 제출된 양식 데이터를 Azure Storage에 저장하기 위한 PUT 요청을 만드는 것입니다. 컨테이너 이름과 BLOB ID를 URL에 포함하도록 Blob 서비스 SAS URL을 수정해야 합니다. 모든 양식 제출은 고유한 BLOB ID로 식별해야 합니다. 고유 BLOB ID는 일반적으로 코드에서 만들어져 PUT 요청의 URL에 삽입됩니다.
다음은 PUT 요청의 부분 URL입니다. 다음 `aemformstutorial` 는 저장소 계정의 이름이고, formsubmissions는 고유한 BLOB ID로 데이터가 저장되는 컨테이너입니다. URL의 나머지 부분은 그대로 유지됩니다.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

다음은 PUT 요청을 사용하여 제출된 양식 데이터를 Azure Storage에 저장하도록 작성된 함수입니다. URL에서 컨테이너 이름과 UUID를 사용합니다. 아래 나열된 샘플 코드를 사용하여 OSGi 서비스 또는 sling 서블릿을 만들고 양식 제출물을 Azure Storage에 저장할 수 있습니다.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## 컨테이너에 저장된 데이터 확인

![form-data-in-컨테이너](./assets/form-data-in-container.png)
