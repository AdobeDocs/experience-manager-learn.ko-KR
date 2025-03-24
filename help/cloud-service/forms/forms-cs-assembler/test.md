---
title: Forms 어셈블러 솔루션 테스트
description: ExecuteAssemblerService.java를 실행하여 솔루션을 테스트합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Eclipse 프로젝트 가져오기

* [zip 파일](./assets/pdf-manipulation.zip) 다운로드 및 압축 풀기
* Eclipse를 시작하고 프로젝트를 Eclipse로 가져오기
* 이 프로젝트에는 리소스 폴더에 있는 다음 폴더가 포함됩니다.
   * ddxFiles - 이 폴더에는 생성하려는 출력을 설명하는 ddx 파일이 포함되어 있습니다.
   * pdffiles - 이 폴더에는 어셈블할 pdf 파일과 PDFA 유틸리티를 테스트할 pdf 파일이 포함되어 있습니다.
   * 자격 증명 - 이 폴더에는 pdfa-options.json 파일이 포함되어 있습니다.

![resources-file](./assets/resources.png)

## PDF 파일 어셈블 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여넣습니다.
* AssemblePDFFiles.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다
* ExecuteAssemblerService.java를 엽니다. 인스턴스를 가리키도록 변수 _AEM_FORMS_CS_&#x200B;의 값을 설정하십시오.
* 두 개 이상의 PDF 파일 조립을 테스트하려면 해당 줄의 주석 처리를 제거하십시오
* ExecuteAssemblerService.java를 java 응용 프로그램으로 실행합니다

### PDFA 유틸리티 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여넣습니다.
* PDFAUtilities.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다.
* ExecuteAssemblerService.java를 엽니다. 인스턴스를 가리키도록 변수 _AEM_FORMS_CS_&#x200B;의 값을 설정하십시오.
* PDFA 작업을 테스트하려면 해당 줄의 주석 처리를 제거합니다.
* ExecuteAssemblerService.java를 java 응용 프로그램으로 실행합니다.



>[!NOTE]
> Java 프로그램을 처음 실행하면 HTTP 403 오류가 발생합니다. 이 문제를 해결하려면 [AEM의 기술 계정 사용자에게 적절한 권한을 부여](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)하세요.

**AEM Forms 사용자**&#x200B;는 이 강의에 사용한 역할입니다.
