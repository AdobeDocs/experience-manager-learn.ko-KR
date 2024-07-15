---
title: HTM5 양식 제출에서 AEM 워크플로우 트리거 - 활용 사례 작업
description: 오프라인 모드에서 모바일 양식을 계속 채우고 AEM 워크플로우를 트리거하기 위한 모바일 양식을 제출합니다.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# 시스템에서 작동하도록 이 사용 사례를 가져오는 중

>[!NOTE]
>
>샘플 에셋이 시스템에서 작동하는 경우 AEM Author 및 Publish 인스턴스가 각각 포트 4502 및 4503에서 실행되고 있다고 가정합니다. 또한 AEM 작성자는 `admin`/`admin`을(를) 통해 액세스할 수 있다고 가정합니다. 포트 번호 또는 관리자 암호가 변경된 경우 이러한 샘플 자산은 작동하지 않습니다. 제공된 샘플 코드를 사용하여 고유한 에셋을 만들어야 합니다.

이 사용 사례를 로컬 시스템에서 작업하려면 다음 단계를 따르십시오.

* 포트 4502에 AEM 작성자 인스턴스 및 포트 4503에 AEM Publish 인스턴스 설치
* [AEM Forms에서 서비스 사용자를 사용하여 개발에 지정된 지침을 따르십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). 서비스 사용자를 만들고 AEM Author 및 Publish 인스턴스에 번들을 배포해야 합니다.
* [osgi 구성을 엽니다](http://localhost:4503/system/console/configMgr).
* **Apache Sling 레퍼러 필터**&#x200B;를 검색합니다. 비어 있음 확인란이 선택되어 있는지 확인합니다.
* [사용자 지정 AEMFormDocumentService 번들을 배포](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)합니다. 이 번들은 AEM Publish 인스턴스에 배포해야 합니다. 이 번들에는 모바일 양식에서 대화형 PDF을 생성하는 코드가 있습니다.
* [이 문서와 관련된 자산을 다운로드하고 압축 해제합니다.](assets/offline-pdf-submission-assets.zip) 다음 항목을 받게 됩니다.
   * **offline-submission-profile.zip** - 이 AEM 패키지에는 대화형 pdf를 로컬 파일 시스템에 다운로드할 수 있는 사용자 지정 프로필이 포함되어 있습니다. AEM Publish 인스턴스에 이 패키지를 배포합니다.
   * **xdp-form-and-workflow.zip** - 이 AEM 패키지에는 노드 콘텐츠/pdfsubmissions에 구성된 XDP, 샘플 워크플로, 런처가 포함되어 있습니다. AEM 작성자 및 Publish 인스턴스 모두에 이 패키지를 배포합니다.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - 대부분의 작업을 수행하는 AEM 번들입니다. 이 번들에는 `/bin/startworkflow`에 탑재된 서블릿이 포함되어 있습니다. 이 서블릿은 제출된 양식 데이터를 AEM 저장소의 `/content/pdfsubmissions` 노드에 저장합니다. AEM 작성자 및 Publish 인스턴스 모두에 이 번들을 배포합니다.
* [모바일 양식 미리 보기](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 여러 필드를 입력한 다음 도구 모음의 단추를 클릭하여 대화형 PDF을 다운로드합니다.
* Acrobat을 사용하여 다운로드한 PDF을 입력하고 제출 버튼을 누릅니다.
* 성공 메시지를 받아야 합니다.
* AEM 작성자 인스턴스에 관리자로 로그인
* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)
* 제출된 PDF을 검토하려면 작업 항목이 있어야 합니다.

>[!NOTE]
>
>일부 고객은 게시 인스턴스에서 실행되는 서블릿에 PDF을 제출하는 대신 Tomcat과 같은 서블릿 컨테이너에 서블릿을 배포했습니다. 모든 것은 고객이 익숙한 토폴로지에 따라 다릅니다.이 자습서에서는 pdf 제출을 처리하기 위해 게시 인스턴스에 배포된 서블릿을 사용할 예정입니다.
