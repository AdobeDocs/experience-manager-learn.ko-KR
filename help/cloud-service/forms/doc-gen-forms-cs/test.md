---
title: 솔루션 테스트
description: Main.java를 실행하여 솔루션을 테스트합니다
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: 적응형 양식
topic: 개발
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 1%

---


# Eclipse 프로젝트 가져오기

[zip 파일](./assets/aem-forms-doc-gen.zip) 다운로드 및 압축 해제

Eclipse를 실행하고 프로젝트를 Eclipse로 가져오기
프로젝트는 리소스 폴더에 다음 파일을 포함합니다.

* DataFile1 및 DataFile2 - 최종 PDF 파일을 생성하기 위해 템플릿과 병합되는 샘플 xml 데이터 파일
* address.xdp - XDP 템플릿
* service_token.json - 이 파일의 내용을 계정별 자격 증명으로 바꾸어야 합니다
* options.json - 이 파일에 지정된 옵션은 API에서 생성된 PDF 파일의 속성을 설정하는 데 사용됩니다

![리소스 파일](./assets/resource-files.JPG)

## 솔루션 테스트

* 프로젝트의 service_token.json 리소스 파일에 서비스 자격 증명을 복사하여 붙여 넣습니다.
* DocumentGeneration.java 파일을 열고 생성된 PDF 파일을 저장할 폴더를 지정합니다
* Main.java를 엽니다. 인스턴스를 가리키도록 변수 postURL의 값을 설정합니다.
* Main.java를 Java 응용 프로그램으로 실행

>[!NOTE]
> Java 프로그램을 처음 실행하면 HTTP 403 오류가 발생합니다. 이 작업을 마치려면 [AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)에서 기술 계정 사용자에게 적절한 권한을 부여해야 합니다.

**AEM Forms** 사용자는 이 교육 과정에서 사용한 역할을 지정합니다.

