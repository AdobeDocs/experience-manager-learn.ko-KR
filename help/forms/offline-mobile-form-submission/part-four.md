---
title: HTM5 양식 제출 시 AEM 워크플로우 트리거
seo-title: HTML5 양식 제출 시 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 전송하여 AEM 워크플로우를 트리거할 수 있습니다.
seo-description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 전송하여 AEM 워크플로우를 트리거할 수 있습니다.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---


# 시스템에서 이 사용 사례 가져오기

>[!NOTE]
>
>샘플 에셋이 시스템에서 작동하려면 포트 4502 및 4503에서 각각 AEM 작성자 및 게시 인스턴스가 실행 중이라고 가정합니다. 또한 AEM 작성자가 `admin`/`admin`을(를) 통해 액세스할 수 있다고 가정합니다. 포트 번호 또는 관리자 암호가 변경된 경우 이러한 샘플 에셋이 작동하지 않습니다. 제공된 샘플 코드를 사용하여 직접 에셋을 만들어야 합니다.

로컬 시스템에서 이 사용 사례를 사용하려면 다음 단계를 따르십시오.

* 포트 4502 및 포트 4503에 AEM 작성자 인스턴스 설치
* [AEM Forms에서 서비스 사용자와 함께 개발하는 동안 지정한 지침을 따르십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). 서비스 사용자를 만들고 AEM 작성자 및 게시 인스턴스에 번들을 배포하십시오.
* [osgi 구성을 엽니다 ](http://localhost:4503/system/console/configMgr).
* **Apache Sling 레퍼러 필터**&#x200B;를 검색합니다. [비어 있음] 확인란이 선택되어 있는지 확인합니다.
* [사용자 지정 AEMFormDocumentService 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) 배포.이 번들을 AEM 게시 인스턴스에 배포해야 합니다. 이 번들에는 모바일 양식에서 대화형 PDF를 생성하는 코드가 포함되어 있습니다.
* [이 아티클과 관련된 에셋을 다운로드하고 압축을 해제합니다.](assets/offline-pdf-submission-assets.zip) 다음 항목을 확인할 수 있습니다.
   * **offline-submission-profile.zip** - 이 AEM 패키지에는 로컬 파일 시스템에 대화형 pdf를 다운로드할 수 있는 사용자 정의 프로필이 포함되어 있습니다. AEM 게시 인스턴스에 이 패키지를 배포합니다.
   * **xdp-form-and-workflow.zip** - 이 AEM 패키지에는 노드 컨텐츠/pdfsubmissions에 구성된 XDP, 샘플 워크플로우,launcher가 포함되어 있습니다. AEM 작성자 및 게시 인스턴스 모두에 이 패키지를 배포합니다.
   * **HandlePDFubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - 대부분의 작업을 수행하는 AEM 번들입니다. 이 번들은 `/bin/startworkflow`에 마운트된 서블릿을 포함합니다. 이 서블릿은 AEM 저장소의 `/content/pdfsubmissions` 노드 아래에 제출된 양식 데이터를 저장합니다. AEM 작성자 및 게시 인스턴스 모두에 이 번들을 배포합니다.
* [모바일 양식 미리 보기](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 여러 필드를 채운 다음 도구 모음의 단추를 클릭하여 대화형 PDF를 다운로드합니다.
* Acrobat을 사용하여 다운로드한 PDF를 채우고 제출 버튼을 누릅니다.
* 성공 메시지를 받아야 합니다.
* AEM 작성자 인스턴스에 관리자로 로그인
* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)
* 제출한 PDF를 검토할 수 있는 작업 항목이 있어야 합니다.

>[!NOTE]
>
>게시 인스턴스에서 실행되는 서블릿에 PDF를 전송하는 대신 일부 고객은 Tomcat과 같은 서블릿 컨테이너에 서블릿을 배포했습니다. 이 모든 것은 고객이 익숙한 토폴로지에 따라 다릅니다. 이 자습서의 목적에 따라 게시 인스턴스에 배포된 서블릿을 사용하여 pdf 제출을 처리하게 됩니다.

