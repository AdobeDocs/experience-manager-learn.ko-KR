---
title: 최신 Archetype으로 클라우드 서비스 프로젝트 업데이트
description: 최신 Archetype으로 AEM Forms 클라우드 서비스 프로젝트 업데이트
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 85
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 이전 aem Archetype에서 마이그레이션

기존 AEM Forms 프로젝트를 최신 Maven Archetype으로 업데이트하려면 코드/구성 등을 이전 프로젝트에서 새 프로젝트로 수동으로 복사해야 합니다.

Archetype 30을 사용하여 생성된 프로젝트를 Archetype 33 프로젝트로 마이그레이션하려면 다음 단계를 따르십시오

## 최신 Archetype을 사용하여 Maven 프로젝트 만들기

* 명령 프롬프트를 열고 c:\cloudmanager로 이동합니다.
* 최신 Archetype을 사용하여 Maven 프로젝트를 만듭니다.
* 콘텐츠 복사 및 붙여넣기 [텍스트 파일](assets/creating-maven-project.txt) 명령 프롬프트 창에서 실행하십시오. 다음에 따라 DarchetypeVersion=33을 변경해야 할 수 있습니다. [최신 버전](https://github.com/adobe/aem-project-archetype/releases). Archetype 33에는 새로운 AEM Forms 테마가 포함됩니다.
이미 aem-banking-application 프로젝트가 있는 cloudmanager 폴더에 새 Maven 프로젝트를 생성하고 있으므로 **DartifactId** aem-banking-application에서 다른 방식으로 변경되었습니다. 이 문서에 aem-banking-application1을 사용했습니다.

>[!NOTE]
>
>이 새 프로젝트를 그대로 배포하면 클라우드 서비스 인스턴스에 HandleFormSubmission 및 SubmitToAEMServlet이 없습니다. Cloud Manager를 사용하여 프로젝트를 배포할 때마다 `/apps` 폴더가 삭제되고 덮어쓰기됩니다.

## Java 코드 복사

프로젝트가 정상적으로 생성되면 이전 프로젝트에서 이 새 프로젝트에 코드/구성 등을 복사할 수 있습니다

* 에서 HandleFormSubmission 서블릿 복사 ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
끝
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 다음에서 CustomSubmit 복사
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` aem-banking-application에서 aem-banking-application1 프로젝트로

* 새 프로젝트를 IntelliJ로 가져오기

* 다음 행을 포함하도록 aem-banking-application1 프로젝트의 ui.apps 모듈에서 filter.xml을 업데이트합니다
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

모든 코드를 새 프로젝트에 복사한 후 이 프로젝트를 cloud manager에 푸시할 수 있습니다.

>[!NOTE]
>
>콘텐츠(적응형 Forms, 양식 데이터 모델 등)를 새 프로젝트에 동기화하려면 IntelliJ 프로젝트에서 적절한 폴더 구조를 만든 다음 저장소 도구의 Get 명령을 사용하여 AEM 인스턴스와 IntelliJ 프로젝트를 동기화해야 합니다.
