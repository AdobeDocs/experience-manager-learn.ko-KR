---
title: 클라우드 서비스 구성 및 양식 데이터 모델을 클라우드 인스턴스로 푸시
description: Azure 스토리지 양식 데이터 모델을 기반으로 하는 적응형 양식을 만들어 클라우드 인스턴스에 푸시합니다.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 57
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# 프로젝트에 클라우드 서비스 구성 포함

클라우드 서비스 구성을 보관할 &#39;FormTutorial&#39;이라는 구성 컨테이너를 만듭니다. Azure 저장소 계정 세부 정보 및 Azure 액세스 키를 제공하여 &#39;FormTutorial&#39; 컨테이너에서 &#39;FormsCSAndAzureBlob&#39;라는 Azure 저장소에 대한 클라우드 서비스 구성을 만듭니다.

IntelliJ에서 AEM 프로젝트를 엽니다. ui.content 프로젝트에 아래와 같이 FormTutorial 폴더를 추가해야 합니다
![cloud-services-configuration](assets/cloud-services-configuration.png)

ui.content 프로젝트의 filter.xml에 다음 항목을 추가해야 합니다

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 프로젝트에 양식 데이터 모델 포함

이전 단계에서 만든 클라우드 서비스 구성을 기반으로 양식 데이터 모델을 만듭니다. 프로젝트에 양식 데이터 모델을 포함하려면 intelliJ의 AEM 프로젝트에 적절한 폴더 구조를 만듭니다. 예를 들어 내 양식 데이터 모델은 등록이라는 폴더에 있습니다
![fdm-content](assets/ui-content-fdm.png)

ui.content 프로젝트의 filter.xml에 적절한 항목을 포함합니다

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>이제 cloud manager를 사용하여 프로젝트를 빌드하고 배포할 때 클라우드 서비스 구성에 Azure 액세스 키를 다시 입력해야 합니다. 액세스 키를 다시 입력하지 않도록 하려면에서 설명한 대로 환경 변수를 사용하여 컨텍스트 인식 구성을 만드는 것이 좋습니다. [다음 문서](./context-aware-fdm.md)

## 다음 단계

[컨텍스트 인식 구성 만들기](./context-aware-fdm.md)
