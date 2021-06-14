---
title: 솔루션 테스트
description: 양식에 첨부 파일을 추가하여 솔루션을 테스트하고 워크플로우를 트리거하여 전자 메일을 보냅니다.
sub-product: forms
feature: 워크플로우
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: 개발
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# 두 접근 방식을 테스트하는 데 필요한 절차

* AEM Forms 서버에서 전자 메일을 보내도록 [일 CQ 메일 서비스](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service)를 구성합니다
* [양식 첨부 파일](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) 번들을 [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 배포합니다.

## zip 파일을 전자 메일 첨부 파일로 전송



* [SendFormAttachmentsViaEmail 워크플로우를 배포합니다.](assets/zipped-form-attachments-model.zip) 이 워크플로우는 전자 메일 전송 구성 요소를 사용하여 사용자 지정 프로세스 단계에 의해 페이로드 폴더 아래에 저장된 zip_attachments.zip 파일을 보냅니다. 필요에 따라 보낸 사람 및 받는 사람의 이메일 주소를 구성합니다.
* [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 [샘플 양식](assets/zip-form-attachments-form.zip)을 가져옵니다.
* [양식을 ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 미리 보고 첨부 파일 두 개를 추가하고 양식을 제출합니다.
* 워크플로우가 트리거되고 zip 파일이 있는 이메일 알림을 전송해야 합니다.

## 양식 첨부 파일을 개별 파일로 보내기

* [SendForm 워크플로우를 배포합니다.](assets/send-form-attachments-model.zip) 이 워크플로우는 전자 메일 구성 요소 보내기를 사용하여 양식 첨부 파일을 개별 파일로 보냅니다. 필요에 따라 보낸 사람 및 받는 사람 이메일 주소를 구성합니다.
* [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 [샘플 양식](assets/send-list-attachments-form.zip)을 가져옵니다.
* [양식을 ](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 미리 보고 첨부 파일 두 개를 추가하고 양식을 제출합니다.
* 워크플로우가 트리거되고 양식 첨부 파일이 있는 이메일 알림이 전송됩니다.
