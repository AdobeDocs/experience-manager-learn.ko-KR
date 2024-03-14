---
title: 워크플로우 단계를 사용하여 SharePoint 목록에 데이터 제출
description: FDM 워크플로 단계 호출을 사용하여 sharepoint 목록에 데이터 삽입
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# FDM 워크플로 단계 호출을 사용하여 SharePoint 목록에 데이터 삽입


이 문서에서는 AEM 워크플로에서 FDM 호출 단계를 사용하여 SharePoint 목록에 데이터를 삽입하는 데 필요한 단계에 대해 설명합니다.

이 문서에서는 다음을 보유한 것으로 가정합니다. [SharePoint 목록에 데이터를 제출하도록 적응형 양식을 구성했습니다.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## SharePoint 목록 데이터 소스를 기반으로 양식 데이터 모델 만들기

* SharePoint 목록 데이터 소스를 기반으로 새 양식 데이터 모델을 만듭니다.
* 적절한 모델을 추가하고 양식 데이터 모델의 서비스를 가져옵니다.
* 최상위 수준 모델 개체를 삽입하도록 삽입 서비스를 구성합니다.
* 삽입 서비스를 테스트합니다.


## 워크플로 만들기

* FDM 호출 단계를 사용하여 간단한 워크플로우를 생성합니다.
* 이전 단계에서 만든 양식 데이터 모델을 사용하도록 FDM 호출 단계를 구성합니다.
* ![associate-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* JSON 점 표기법을 사용하십시오. 제출된 데이터는 아래 형식으로 되어 있으며, 우리는 제출된 데이터에서 ContactUS 객체를 추출합니다.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## AEM 워크플로우를 트리거할 적응형 양식 구성

* 이전 단계에서 만든 양식 데이터 모델을 사용하여 적응형 양식을 만듭니다.
* 데이터 소스의 일부 필드를 양식으로 끌어다 놓습니다.
* 아래와 같이 양식의 제출 액션을 구성합니다
* ![submit-action](assets/configure-af.png)



## 양식 테스트

이전 단계에서 만든 양식을 미리 봅니다. 양식을 작성하고 제출하십시오. 양식의 데이터는 SharePoint 목록에 삽입해야 합니다.

