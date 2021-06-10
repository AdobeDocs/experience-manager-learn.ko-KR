---
title: 적응형 양식 첨부 파일 보내기
description: 적응형 양식 첨부 파일을 압축하여 전자 메일 구성 요소 보내기를 사용하여 전송합니다
feature: 적응형 양식
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: 개발
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 2%

---


# 소개



일반적인 사용 사례는 적응형 양식 첨부 파일을 압축하고 AEM 워크플로우에서 이메일 전송 구성 요소를 사용하여 전송하는 것입니다. 사용 사례를 수행하기 위해 사용자 지정 워크플로우 프로세스 단계가 작성됩니다. 이 사용자 지정 프로세스에서는 *zip_attachments.zip* 파일의 페이로드 폴더 아래에 양식 첨부 파일이 생성되고 저장되는 zip 파일을 단계에서 생성합니다

![양식 첨부 파일 보내기](assets/send-form-attachments.JPG)


