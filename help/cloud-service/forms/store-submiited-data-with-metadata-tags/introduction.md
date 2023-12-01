---
title: 사용자 지정 메타데이터 태그를 추가하는 자습서
description: Azure 스토리지 계정에서 적응형 양식 데이터를 저장하고 검색하는 방법에 대해 알아봅니다.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 3%

---

# 소개

이 자습서에서는 Azure 스토리지에 양식 제출을 Blob 인덱스 태그와 함께 저장하는 간단한 사용 사례를 구현합니다. Blob 인덱스 태그는 키-값 인덱스 태그 속성을 사용하여 데이터 관리 및 검색 기능을 제공합니다. 단일 컨테이너 내에서 또는 저장소 계정의 모든 컨테이너에서 개체를 분류하고 찾을 수 있습니다.
![blob-index-tags](assets/blob-with-index-tags.png)

## 전제 조건

* AEM Forms CS에 대한 경험.
* Cloud Manager를 사용하여 코드를 배포한 경험이 있습니다.
* AEM Forms CS의 클라우드 지원 인스턴스에 액세스합니다.

AEM Forms CS에서 위의 사용 사례를 구현하려면 다음이 필요합니다

* [AEM Forms CS 클라우드 지원 인스턴스](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure 포털 계정](https://portal.azure.com/)


### 다음 단계

[extend-choice-group-components](./extend-choice-group-components.md)
