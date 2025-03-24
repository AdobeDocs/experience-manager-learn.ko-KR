---
title: 솔루션 테스트 - 두 가지 접근 방식을 테스트하는 데 필요한 단계
description: 양식에 첨부 파일을 추가하여 솔루션을 테스트하고 워크플로우를 트리거하여 이메일을 전송합니다.
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 두 가지 접근 방식을 테스트하는 데 필요한 단계

* AEM Forms 서버에서 전자 메일을 보내도록 [일 CQ 메일 서비스](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) 구성
* [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) 번들 배포

## zip 파일을 이메일 첨부 파일로 보내기



* [SendFormAttachmentsViaEmail 워크플로우를 배포합니다.](assets/zipped-form-attachments-model.zip) 이 워크플로에서는 전자 메일 보내기 구성 요소를 사용하여 사용자 지정 프로세스 단계를 통해 페이로드 폴더에 저장된 zipped_attachments.zip 파일을 보냅니다. 필요에 따라 보낸 사람 및 받는 사람의 이메일 주소를 구성합니다.
* [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 [샘플 양식](assets/zip-form-attachments-form.zip) 가져오기
* [양식을 미리 보고](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 첨부 파일을 몇 개 추가한 다음 양식을 제출합니다.
* 워크플로우가 트리거되고 zip 파일이 포함된 이메일 알림이 전송되어야 합니다.

## 양식 첨부 파일을 개별 파일로 보내기

* [SendForm 워크플로우를 배포합니다.](assets/send-form-attachments-model.zip) 이 워크플로에서는 전자 메일 보내기 구성 요소를 사용하여 양식 첨부 파일을 개별 파일로 보냅니다. 필요에 따라 발신자 및 수신자 이메일 주소를 구성합니다.
* [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 [샘플 양식](assets/send-list-attachments-form.zip) 가져오기
* [양식을 미리 보고](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 첨부 파일을 몇 개 추가한 다음 양식을 제출합니다.
* 워크플로우가 트리거되고 양식 첨부 파일이 포함된 이메일 알림이 전송됩니다.
