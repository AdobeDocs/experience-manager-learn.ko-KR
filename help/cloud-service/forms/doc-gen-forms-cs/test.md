---
title: 솔루션 테스트
description: Main.java를 실행하여 솔루션을 테스트합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Eclipse 프로젝트 가져오기

을(를) 다운로드하고 압축 해제합니다. [zip 파일](./assets/aem-forms-cs-doc-gen.zip)

Eclipse를 시작하고 프로젝트를 Eclipse로 가져오기 프로젝트에 리소스 폴더에 다음 파일이 포함됩니다.

* DataFile1,DataFile2 및 DataFile3 - 최종 PDF 파일을 생성하기 위해 템플릿과 병합할 샘플 xml 데이터 파일입니다.
* custom_fonts.xdp - XDP 템플릿.
* service_token.json - 이 파일의 컨텐츠를 계정별 자격 증명으로 바꿔야 합니다.
* options.json - 이 파일에 지정된 옵션은 API에 의해 생성된 PDF 파일의 속성을 설정하는 데 사용됩니다

![resources-file](./assets/resource-files.png)

## 솔루션 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여넣습니다.
* DocumentGeneration.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다
* Main.java를 엽니다. 인스턴스를 가리키도록 변수 postURL의 값을 설정합니다.
* Java 애플리케이션으로 Main.java 실행

>[!NOTE]
> Java 프로그램을 처음 실행하면 HTTP 403 오류가 발생합니다. 이 문제를 해결하려면 다음을 지정하십시오. [AEM의 기술 계정 사용자에게 적절한 권한](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms 사용자** 이 강의에서 사용한 역할입니다.
