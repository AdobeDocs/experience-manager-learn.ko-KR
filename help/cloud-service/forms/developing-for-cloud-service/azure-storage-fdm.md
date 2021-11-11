---
title: 클라우드 서비스에 클라우드 서비스 구성 및 양식 데이터 모델 푸시
description: Azure 저장소 양식 데이터 모델을 기반으로 적응형 양식을 만들고 클라우드 인스턴스에 푸시합니다.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# 프로젝트에 클라우드 서비스 구성 포함

클라우드 서비스 구성을 유지하기 위해 &#39;FormsTutorial&#39;이라는 구성 컨테이너를 만듭니다. &#39;FormsTutorial&#39; 컨테이너에서 &#39;Azure에서 양식 제출 저장&#39;이라는 Azure 저장소에 대한 클라우드 서비스 구성을 만드십시오. Azure 저장소 계정 세부 정보와 계정 키를 제공합니다

IntelliJ에서 AEM 프로젝트를 엽니다. ui.content 프로젝트에서 아래에 표시된 대로 FormTutorial 폴더를 추가해야 합니다
![cloud-services-configuration](assets/cloud-services-configuration.png)

ui.content 프로젝트의 filter.xml에 다음 항목을 추가해야 합니다

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 프로젝트에 양식 데이터 모델 포함

이전 단계에서 만든 클라우드 서비스 구성을 기반으로 양식 데이터 모델을 만듭니다. 프로젝트에 양식 데이터 모델을 포함하려면 IntelliJ에서 AEM 프로젝트에 적절한 폴더 구조를 만듭니다. 예를 들어, 내 양식 데이터 모델은 등록이라는 폴더에 있습니다
![fdm-content](assets/ui-content-fdm.png)

ui.content 프로젝트의 filter.xml에 적절한 항목을 포함합니다

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>이제 프로젝트를 빌드하고 배포하면 프로젝트에서 클라우드 인스턴스에서 사용할 수 있는 클라우드 서비스 구성을 기반으로 양식 데이터 모델을 사용할 수 있습니다
