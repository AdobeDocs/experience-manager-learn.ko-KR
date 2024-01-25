---
title: 데이터베이스에 양식 첨부 파일 삽입
description: AEM 워크플로우를 사용하여 데이터베이스에 양식 첨부 파일을 삽입합니다.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 98
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# 데이터베이스에 양식 첨부 파일 삽입

이 문서에서는 MySQL 데이터베이스에 양식 첨부 파일을 저장하는 사용 사례를 살펴봅니다.

캡처된 양식 데이터와 양식 첨부 파일을 데이터베이스 테이블에 저장하는 것이 고객의 일반적인 요구입니다.
이 사용 사례를 달성하려면 다음 단계를 수행하십시오

## 양식 데이터 및 첨부 파일을 보관할 데이터베이스 테이블 만들기

양식 데이터를 저장할 newhire라는 테이블을 만들었습니다. 유형의 열 이름 그림을 확인합니다. **롱블롭** 양식 첨부 파일을 저장하려면
![테이블 스키마](assets/insert-picture-table.png)

## 양식 데이터 모델 만들기

MySQL 데이터베이스와 통신하기 위해 양식 데이터 모델을 만들었습니다. 다음을 만들어야 합니다

* [AEM의 JDBC 데이터 소스](./data-integration-technical-video-setup.md)
* [JDBC 데이터 소스를 기반으로 하는 양식 데이터 모델](./jdbc-data-model-technical-video-use.md)

## 워크플로우 만들기

AEM 워크플로에 제출하도록 적응형 양식을 구성하려면 양식 첨부 파일을 워크플로 변수에 저장하거나 페이로드 아래의 지정된 폴더에 저장할 수 있습니다. 이 사용 사례에서는 첨부 파일을 ArrayList of Document 유형의 워크플로 변수에 저장해야 합니다. 이 ArrayList에서 첫 번째 항목을 추출하고 문서 변수를 초기화해야 합니다. 라는 워크플로우 변수 **listOfDocuments** 및 **employeePhoto** 이(가) 만들어졌습니다.
워크플로를 트리거하기 위해 적응형 양식이 제출되면 워크플로의 한 단계는 ECMA 스크립트를 사용하여 employeePhoto 변수를 초기화합니다. 다음은 ECMA 스크립트 코드입니다

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

워크플로우의 다음 단계는 양식 데이터 모델 호출 서비스 구성 요소를 사용하여 데이터 및 양식 첨부 파일을 테이블에 삽입하는 것입니다.
![삽입 사진](assets/fdm-insert-pic.png)
[샘플 ecma 스크립트를 사용하는 전체 워크플로우는 여기에서 다운로드할 수 있습니다.](assets/add-new-employee.zip).

>[!NOTE]
> 새 JDBC 기반 양식 데이터 모델을 만들고 워크플로우에서 해당 양식 데이터 모델을 사용해야 합니다

## 적응형 양식 만들기

이전 단계에서 만든 양식 데이터 모델을 기반으로 적응형 양식을 만듭니다. 양식 데이터 모델 요소를 양식에 끌어다 놓습니다. 워크플로우를 트리거하기 위한 양식 제출을 구성하고 아래 스크린샷에 표시된 대로 다음 속성을 지정합니다
![양식 첨부 파일](assets/form-attachments.png)
