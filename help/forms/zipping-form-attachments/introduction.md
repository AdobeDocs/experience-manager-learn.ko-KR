---
title: 적응형 양식 첨부 파일 보내기
description: 이메일 구성 요소 보내기를 사용하여 적응형 양식 첨부 파일 보내기
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 2%

---

# 소개



일반적인 사용 사례는 AEM 워크플로우에서 이메일 구성 요소 보내기 를 사용하여 적응형 양식 첨부 파일을 보내는 것입니다.
고객은 일반적으로 양식 첨부 파일을 압축하거나 이메일 구성 요소 보내기를 사용하여 첨부 파일을 개별 파일로 보냅니다.

## 양식 첨부 파일을 zip 파일로 보내기

사용 사례를 달성하기 위해 사용자 지정 워크플로우 프로세스 단계가 작성되었습니다. 이 맞춤형 프로세스 단계에서는 양식 첨부 파일이 포함된 zip 파일을 만들어 이라는 파일의 페이로드 폴더에 저장합니다. *zipped_attachments.zip*

![양식 첨부 파일 보내기](assets/send-form-attachments.JPG)

## 개별적으로 양식 첨부 파일 보내기

이 사용 사례를 달성하기 위해 사용자 지정 워크플로우 프로세스 단계가 작성되었습니다. 이 사용자 지정 프로세스 단계에서는 ArrayList of Documents 및 ArrayList of Strings 유형의 워크플로 변수를 채웁니다.

![문서 목록 보내기](assets/send-list-of-documents.JPG)

## 다음 단계

[Zip 양식 첨부 파일](./custom-process-step.md)
