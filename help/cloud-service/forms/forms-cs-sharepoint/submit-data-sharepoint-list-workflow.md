---
title: 워크플로우 단계를 사용하여 SharePoint 목록에 데이터 제출
description: FDM 워크플로 단계 호출을 사용하여 sharepoint 목록에 데이터 삽입
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---

# FDM 워크플로 단계 호출을 사용하여 SharePoint 목록에 데이터 삽입


이 문서에서는 AEM 워크플로에서 FDM 호출 단계를 사용하여 SharePoint 목록에 데이터를 삽입하는 데 필요한 단계에 대해 설명합니다.

이 문서에서는 데이터를 SharePoint 목록에 제출하기 위해 [적응형 양식을 구성했다고 가정합니다.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=ko#connect-af-sharepoint-list)


## SharePoint 목록 데이터 소스를 기반으로 양식 데이터 모델 만들기

* SharePoint 목록 데이터 소스를 기반으로 새 양식 데이터 모델을 만듭니다.
* 적절한 모델을 추가하고 양식 데이터 모델의 서비스를 가져옵니다.
* 최상위 수준 모델 개체를 삽입하도록 삽입 서비스를 구성합니다.
* 삽입 서비스를 테스트합니다.


## 워크플로 만들기

* FDM 호출 단계를 사용하여 간단한 워크플로우를 생성합니다.
* 이전 단계에서 만든 양식 데이터 모델을 사용하도록 FDM 호출 단계를 구성합니다.
* ![associate-fdm](assets/fdm-insert-1.png)

## 핵심 구성 요소를 기반으로 하는 적응형 양식

제출된 데이터는 다음과 같은 형식입니다. 스크린샷과 같이 양식 데이터 모델 서비스 호출 워크플로우 단계에서 점 표기법을 사용하여 ContactUS 객체를 추출해야 합니다

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


* ![map-input-parameters](assets/fdm-insert-2.png)


## 기초 구성 요소를 기반으로 하는 적응형 양식

제출된 데이터는 다음과 같은 형식입니다. 양식 데이터 모델 서비스 호출 워크플로우 단계에서 점 표기법을 사용하여 ContactUS JSON 개체 추출

```json
{
    "afData": {
        "afUnboundData": {
            "data": {}
        },
        "afBoundData": {
            "data": {
                "ContactUS": {
                    "Title": "Lord",
                    "HighNetWorth": "true",
                    "SubmitterName": "John Doe",
                    "Products": "Forms"
                }
            }
        },
        "afSubmissionInfo": {
            "lastFocusItem": "guide[0].guide1[0].guideRootPanel[0].afJsonSchemaRoot[0]",
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/foundationform",
            "afSubmissionTime": "20240517100126"
        }
    }
}
```

![foundation-based-form](assets/foundation-based-form.png)

## AEM 워크플로우를 트리거하는 적응형 양식 구성

* 이전 단계에서 만든 양식 데이터 모델을 사용하여 적응형 양식을 만듭니다.
* 데이터 소스의 일부 필드를 양식으로 끌어다 놓습니다.
* 아래와 같이 양식의 제출 액션을 구성합니다
* ![제출 액션](assets/configure-af.png)



## 양식 테스트

이전 단계에서 만든 양식을 미리 봅니다. 양식을 작성하고 제출하십시오. 양식의 데이터는 SharePoint 목록에 삽입해야 합니다.
