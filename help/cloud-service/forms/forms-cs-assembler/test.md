---
title: 솔루션 테스트
description: ExecuteAssemblerService.java를 실행하여 솔루션을 테스트합니다
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Eclipse 프로젝트 가져오기

* 다운로드 및 압축 해제 [zip 파일](./assets/pdf-manipulation.zip)
* Eclipse를 실행하고 프로젝트를 Eclipse로 가져오기
* 이 프로젝트에는 리소스 폴더에 다음 폴더가 포함되어 있습니다.
   * ddxFiles - 이 폴더에는 생성하려는 출력을 설명하는 ddx 파일이 포함되어 있습니다
   * pdffile - 이 폴더에는 어셈블할 pdf 파일과 PDFA 유틸리티를 테스트하기 위한 pdf 파일이 포함되어 있습니다
   * 자격 증명 - 이 폴더에는 pdfa-options.json 파일이 포함되어 있습니다

![리소스 파일](./assets/resources.png)

## 테스트 조립 PDF 파일

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여 넣습니다.
* AssemblePDFFfiles.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다
* ExecuteAssemblerService.java를 엽니다. 변수의 값 설정 _AEM_FORMS._CS_ 을 눌러 인스턴스를 가리킵니다.
* 두 개 이상의 PDF 파일 조립을 테스트하려면 적절한 줄의 주석을 해제합니다
* ExecuteAssemblerService.java를 java 응용 프로그램으로 실행합니다

### PDFA 유틸리티 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여 넣습니다.
* PDFAUtilities.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다.
* ExecuteAssemblerService.java를 엽니다. 변수의 값 설정 _AEM_FORMS._CS_ 을 눌러 인스턴스를 가리킵니다.
* PDFA 작업을 테스트할 적절한 줄의 주석을 해제합니다.
* ExecuteAssemblerService.java를 java 응용 프로그램으로 실행합니다.



>[!NOTE]
> Java 프로그램을 처음 실행하면 HTTP 403 오류가 발생합니다. 이 작업을 통과하려면 다음을 수행하십시오 [AEM의 기술 계정 사용자에게 적합한 권한](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms 사용자** 이 강좌에서 제가 사용한 역할입니다.
