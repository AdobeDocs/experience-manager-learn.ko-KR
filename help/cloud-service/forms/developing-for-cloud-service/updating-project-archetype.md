---
title: 최신 원형 중 클라우드 서비스 프로젝트 업데이트
description: 최신 원형 중 AEM Forms 클라우드 서비스 프로젝트 업데이트
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---

# 이전 aem 원형 중에서 마이그레이션

기존 AEM Forms 프로젝트를 최신 maven 원형 상태로 업데이트하려면 이전 프로젝트의 코드/구성 등을 새 프로젝트에 수동으로 복사해야 합니다.

Archetype 30을 사용하여 만든 프로젝트를 Archetype 33 프로젝트로 마이그레이션하기 위해 다음 단계가 수행되었습니다

## 최신 원형 을 사용하여 maven 프로젝트 만들기

* 명령 프롬프트를 열고 c:\cloudmanager으로 이동합니다.
* 최신 원형 을 사용하여 maven 프로젝트를 만듭니다.
* 의 컨텐츠를 복사하여 붙여넣습니다 [텍스트 파일](assets/creating-maven-project.txt) 명령 프롬프트 창에 DarchetypeVersion=33은 [최신 버전](https://github.com/adobe/aem-project-archetype/releases). Archetype 33에는 새로운 AEM Forms 테마가 포함되어 있습니다.
이미 aem-banking-application 프로젝트가 있는 cloudmanager 폴더에서 새 maven 프로젝트를 만들고 있으므로 **DartifactId** aem 뱅킹 애플리케이션에서 다른 주소로 이동합니다. 이 문서에 aem-banking-application1을 사용했습니다.

>[!NOTE]
>
>이 새 프로젝트를 클라우드 서비스 인스턴스로 배포하는 경우 HandleFormSubmission 및 SubmitToAEMServlet이 없습니다. Cloud Manager를 사용하여 프로젝트를 배포할 때마다 앱 폴더 아래에 있는 모든 항목이 삭제되고 덮어쓰여지기 때문입니다.

## Java 코드 복사

프로젝트를 성공적으로 만들면 이전 프로젝트에서 이 새 프로젝트로 코드/구성 등의 복사를 시작할 수 있습니다

* HandleFormSubmission 서블릿을 ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
to

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* CustomSubmit 복사 위치
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` aem-banking-application에서 aem-banking-application1 프로젝트로

* IntelliJ로 새 프로젝트 가져오기

* 다음 줄을 포함하도록 aem-banking-application1 프로젝트의 ui.apps 모듈에서 filter.xml을 업데이트합니다
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

모든 코드를 새 프로젝트에 복사한 후 이 프로젝트를 cloud manager에 푸시할 수 있습니다.

>[!NOTE]
>
>컨텐츠(적응형 Forms, 양식 데이터 모델 등)를 새 프로젝트에 동기화하려면 IntelliJ 프로젝트에서 적절한 폴더 구조를 만든 다음 리포지토리 도구의 가져오기 명령을 사용하여 IntelliJ 프로젝트를 AEM 인스턴스와 동기화해야 합니다.
