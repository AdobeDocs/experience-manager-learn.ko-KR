---
title: 적응형 양식 첨부 파일 보내기
description: 전자 메일 구성 요소를 사용하여 적응형 양식 첨부 파일 보내기
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 소개



일반적인 사용 사례는 AEM 워크플로우에서 전자 메일 구성 요소를 사용하여 적응형 양식 첨부 파일을 전송하는 것입니다.
고객은 일반적으로 양식 첨부 파일을 압축하거나 전자 메일 구성 요소를 사용하여 첨부 파일을 개별 파일로 보냅니다.

## Zip 파일로 양식 첨부 파일 보내기

사용 사례를 수행하기 위해 사용자 지정 워크플로우 프로세스 단계가 작성됩니다. 이 사용자 지정 프로세스에서는 파일에 있는 페이로드 폴더 아래에 양식 첨부 파일이 생성되고 저장되는 zip 파일을 단계에서 *zip_attachments.zip*

![양식 첨부 파일 보내기](assets/send-form-attachments.JPG)

## 양식 첨부 파일을 개별적으로 보내기

이 사용 사례를 수행하기 위해 사용자 지정 워크플로우 프로세스 단계가 작성됩니다. 이 사용자 지정 프로세스 단계에서는 ArrayList of Documents 및 ArrayList of Strings 유형의 워크플로우 변수를 채웁니다.

![문서 목록 보내기](assets/send-list-of-documents.JPG)
