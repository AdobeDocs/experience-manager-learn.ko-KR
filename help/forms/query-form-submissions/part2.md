---
title: 양식 제출 쿼리
description: Azure 포털에 저장된 양식 제출을 쿼리하는 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# 사용자 정의 제출 만들기

사용자 지정 제출 처리기가 양식 제출을 처리하도록 작성되었습니다. 높은 수준에서 사용자 지정 제출 처리기는 다음을 수행합니다

* 제출된 양식의 이름을 추출합니다.
* 제출된 데이터를 추출합니다. 핵심 구성 요소 기반 양식의 제출된 데이터는 항상 JSON 형식입니다.
* Azure 포털에서 양식 첨부 파일을 추출하여 저장합니다. 제출된 JSON 데이터를 첨부 파일의 URL로 업데이트합니다.
* Blob 인덱스 태그 만들기 - 양식에 대해 검색 가능한 필드 목록과 제출된 데이터에서 해당 값을 찾습니다.
* Blob 인덱스 태그를 제출된 데이터와 연결하여 Azure 포털에 저장합니다.

다음 스크린샷은 Azure 포털의 Blob 인덱스 태그를 보여 줍니다

![blob-index-tags](assets/blob-index-tags.png)

사용자 지정 제출 코드는 **_StoreFormDataWithBlobIndexTagsInAzure_**&#x200B;에 있으며 Azure에서 데이터를 저장하고 검색하는 코드는 구성 요소 **_SaveAndFetchFromAzure_**&#x200B;에 있습니다.

## 다음 단계

[쿼리 인터페이스 작성](./part3.md)
