---
title: 적응형 양식에 대한 저장 및 재개 기능 구현
description: Azure 스토리지 계정에서 적응형 양식 데이터를 저장하고 검색하는 방법에 대해 알아봅니다.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 3%

---

# 소개

이 자습서에서는 양식 채우기를 저장하고 양식 채우기 프로세스를 다시 시작할 수 있도록 하는 간단한 사용 사례를 구현합니다. 사용 사례의 흐름은 다음과 같습니다

* 사용자가 적응형 양식 작성을 시작합니다.
* 사용자가 양식을 저장하고 나중에 양식 채우기를 계속합니다.
* 사용자가 저장된 양식에 대한 링크가 포함된 이메일 알림을 받습니다.

## 사전 요구 사항

* 특히 양식 데이터 모델을 만드는 AEM Forms CS에 대한 경험입니다.
* Cloud Manager를 사용하여 코드를 배포한 경험이 있습니다.
* AEM Forms CS의 클라우드 지원 인스턴스에 액세스합니다.

AEM Forms CS에서 위의 사용 사례를 구현하려면 다음이 필요합니다

* [AEM Forms CS 클라우드 지원 인스턴스](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=ko#set-up-aem-author-instance)
* [Azure 포털 계정](https://portal.azure.com/)
* [SendGrid 계정](https://sendgrid.com/)

### 다음 단계

[페이지 구성 요소 만들기](./page-component.md)
