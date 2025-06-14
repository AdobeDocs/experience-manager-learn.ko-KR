---
title: Sharepoint 목록의 데이터로 적응형 양식 미리 채우기
description: 공유 지점 목록에서 지원하는 양식 데이터 모델을 사용하여 적응형 양식을 미리 채우는 방법에 대해 알아봅니다
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14795
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 46
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 공유 지점 목록 데이터를 사용하여 적응형 양식 미리 채우기

이전 버전의 AEM Form(6.5)에서는 요청 속성을 사용하여 양식 데이터 모델 지원 적응형 양식을 미리 채우도록 사용자 지정 코드를 작성해야 했습니다. AEM Forms as cloud service에서는 더 이상 사용자 지정 코드를 작성할 필요가 없습니다.

이 문서에서는 양식 데이터 모델 미리 채우기 서비스를 사용하여 SharePoint 목록에서 가져온 데이터로 적응형 양식을 미리 채우기/미리 채우는 데 필요한 단계에 대해 설명합니다.

이 문서에서는 데이터를 SharePoint 목록에 제출하기 위해 [적응형 양식을 구성했다고 가정합니다.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=ko#connect-af-sharepoint-list)

다음은 SharePoint 목록의 데이터입니다
![sharepoint-list](assets/list-data.png)

적응형 양식에 특정 guid와 연결된 데이터를 미리 채우려면 다음 단계를 수행해야 합니다

## get 서비스 구성

* guid 특성을 사용하여 양식 데이터 모델의 최상위 수준 개체에 대한 get 서비스를 만듭니다.
  ![get-service](assets/mapping-request-attribute.png)

이 스크린샷에서 guid 열은 `submissionid`(이)라는 요청 특성을 통해 바인딩됩니다.

완전히 구성된 get 서비스는 다음과 같습니다

![get-service](assets/fdm-request-attribute.png)

## 양식 데이터 모델 미리 채우기 서비스를 사용하도록 적응형 양식 구성

* 공유 지점 목록 양식 데이터 모델을 기반으로 적응형 양식을 엽니다. 양식 데이터 모델 미리 채우기 서비스 연결
  ![양식 미리 채우기 서비스](assets/form-prefill-service.png)

## 양식 테스트

아래와 같이 URL에 `submissionid`을(를) 포함하여 양식을 미리 봅니다.

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
