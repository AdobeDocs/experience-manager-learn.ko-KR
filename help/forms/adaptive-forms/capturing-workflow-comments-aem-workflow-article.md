---
title: 적응형 Forms Workflow에서 워크플로우 주석 캡처
seo-title: 적응형 Forms Workflow에서 워크플로우 주석 캡처
description: AEM 워크플로우에서 워크플로우 주석 캡처
seo-description: AEM 워크플로우에서 워크플로우 주석 캡처
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---


# 응용 Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}에서 워크플로 주석 캡처

>[AEM Forms 6.4에만 적용됩니다. AEM Forms 6.5에서는 변수 기능을 사용하여 이 사용 사례를 만드십시오]

일반적인 요청은 작업 검토자가 입력한 주석을 이메일에 포함하는 기능입니다. AEM Forms 6.4에서는 사용자가 입력한 주석을 캡처하고 이러한 주석을 이메일에 포함하는 즉시 사용 가능한 메커니즘이 없습니다.

이러한 요구 사항을 충족하기 위해 주석을 캡처하고 이러한 주석을 워크플로우 메타데이터 속성으로 저장하는 데 사용할 수 있는 샘플 OSGi 번들이 제공됩니다.

다음 스크린샷은 [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)에서 프로세스 단계를 사용하여 주석을 캡처하고 메타데이터 속성으로 저장하는 방법을 보여줍니다. &quot;Capture Workflow Comments&quot;는 프로세스 단계에서 사용해야 하는 java 클래스의 이름입니다. 주석을 저장할 메타데이터 속성 이름을 전달해야 합니다. 아래 스크린샷에서 managerComments는 주석을 저장할 메타데이터 속성입니다.

![워크플로 주석1](assets/workflowcomments1.gif)

시스템에서 이 기능을 테스트하려면 다음 단계를 따르십시오.
* [워크플로우의 프로세스 단계가 [캡처 워크플로우 주석]을 사용하도록 구성되었는지 확인합니다.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [DeveloperWithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue 번들을 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 이 번들에는 주석을 캡처하고 메타데이터 속성으로 저장하는 샘플 코드가 포함되어 있습니다

* [이 아티클과 관련된 에셋을 파일 시스템에 다운로드 및 압축 해제에셋에는 워크플로우 모델](assets/capturecomments.zip) 과 샘플 적응형 양식이 포함되어 있습니다.

* 패키지 관리자를 사용하여 2개의 zip 파일을 AEM으로 가져오기

* [이 URL로 이동하여 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 양식 필드를 채우고 양식을 제출합니다.

* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)

* 받은 편지함에서 작업을 열고 양식을 제출합니다. 메시지가 표시되면 주석을 입력하십시오.

주석은 crx의 managerComments라는 메타데이터 속성에 저장됩니다. 주석을 확인하려면 crx에 관리자로 로그인합니다. 워크플로우 인스턴스는 다음 경로에 저장됩니다

/var/workflow/instances/server0

적절한 워크플로우 인스턴스를 선택하고 메타데이터 노드에서 속성 manager주석을 확인합니다.

