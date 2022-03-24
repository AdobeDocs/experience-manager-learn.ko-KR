---
title: 솔루션 테스트
description: ExecuteAssemblerService.java를 실행하여 솔루션을 테스트합니다
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Eclipse 프로젝트 가져오기

* 다운로드 및 압축 해제 [zip 파일](./assets/pdf-manipulation.zip)
* Eclipse를 실행하고 프로젝트를 Eclipse로 가져오기
* 이 프로젝트에는 리소스 폴더에 다음 폴더가 포함되어 있습니다.
   * ddxFiles - 이 폴더에는 생성하려는 출력을 설명하는 ddx 파일이 포함되어 있습니다
   * 파일 - 이 폴더에는 어셈블할 pdf 파일이 포함되어 있습니다

![리소스 파일](./assets/resources.png)

## 솔루션 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여 넣습니다.
* AssemblePDFFfiles.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다
* ExecuteAssemblerService.java를 엽니다. 인스턴스를 가리키도록 변수 assembleURL의 값을 설정합니다.
* ExecuteAssemblerService.java를 java 응용 프로그램으로 실행합니다

>[!NOTE]
> Java 프로그램을 처음 실행하면 HTTP 403 오류가 발생합니다. 이 작업을 통과하려면 다음을 수행하십시오 [AEM의 기술 계정 사용자에게 적합한 권한](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms 사용자** 이 강좌에서 제가 사용한 역할입니다.
