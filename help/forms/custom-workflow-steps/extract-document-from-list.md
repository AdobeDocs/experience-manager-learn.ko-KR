---
title: AEM 워크플로의 문서 목록에서 문서 추출
description: 문서 목록에서 특정 문서를 추출하는 사용자 지정 워크플로 구성 요소
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 문서 목록에서 문서 추출

일반적인 사용 사례는 AEM 워크플로우에서 양식 데이터 모델 호출 단계를 사용하여 양식 데이터 및 양식 첨부 파일을 외부 시스템에 제출하는 것입니다. 예를 들어, ServiceNow에서 사례를 만들 때 지원 문서와 함께 사례 세부 정보를 제출하려고 합니다. 적응형 양식에 추가된 첨부 파일은 문서 유형 arraylist의 변수에 저장되며 이 arraylist에서 특정 문서를 추출하려면 사용자 지정 코드를 작성해야 합니다.

이 문서에서는 사용자 지정 워크플로 구성 요소를 사용하여 문서를 추출하고 문서 변수에 저장하는 단계를 설명합니다.

## 워크플로우 만들기

양식 제출을 처리하려면 워크플로우를 만들어야 합니다. 워크플로우에 다음 변수가 정의되어 있어야 합니다

* ArrayList of Document 형식의 변수(이 변수는 사용자가 추가한 양식 첨부 파일을 보유함)
* 유형 문서의 변수입니다.(이 변수는 ArrayList에서 추출한 문서를 보유합니다.)

* 사용자 지정 구성 요소를 워크플로우에 추가하고 해당 속성을 구성합니다
  ![extract-item-workflow](assets/extract-document-array-list.png)

## 적응형 양식 구성

* 적응형 양식의 제출 액션을 구성하여 AEM 워크플로 트리거
  ![제출 액션](assets/store-attachments.png)

## 솔루션 테스트

[OSGi 웹 콘솔을 사용하여 사용자 지정 번들 배포](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[패키지 관리자를 사용하여 워크플로우 구성 요소 가져오기](assets/Extract-item-from-documents-list.zip)

[샘플 워크플로우 가져오기](assets/extract-item-sample-workflow.zip)

[적응형 양식 가져오기](assets/test-attachment-extractions-adaptive-form.zip)

[양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

양식에 첨부 파일을 추가하고 제출합니다.

>[!NOTE]
>
>그런 다음 추출된 문서를 전자 메일 보내기 또는 FDM 호출 단계와 같은 다른 워크플로 단계에서 사용할 수 있습니다
