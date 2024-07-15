---
title: 적응형 Forms Workflow에서 워크플로우 주석 캡처
description: AEM Workflow에서 워크플로우 주석 캡처
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# 적응형 Forms Workflow에서 워크플로우 주석 캡처{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms 6.4에만 적용됩니다. AEM Forms 6.5에서 변수 기능을 사용하여 이 사용 사례를 달성하십시오]

일반적인 요청은 작업 검토자가 이메일에 입력한 주석을 포함할 수 있는 기능에 대한 것입니다. AEM Forms 6.4에는 사용자가 입력한 댓글을 캡처하고 이러한 댓글을 이메일에 포함하는 기본 메커니즘이 없습니다.

이 요구 사항을 충족하기 위해 설명을 캡처하고 이러한 설명을 워크플로우 메타데이터 속성으로 저장하는 데 사용할 수 있는 샘플 OSGi 번들이 제공됩니다.

다음 스크린샷은 [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)에서 프로세스 단계를 사용하여 댓글을 캡처하고 메타데이터 속성으로 저장하는 방법을 보여 줍니다. &quot;워크플로우 주석 캡처&quot;는 프로세스 단계에서 사용해야 하는 Java 클래스의 이름입니다. 주석을 포함할 메타데이터 속성 이름을 전달해야 합니다. 아래 스크린샷에서 managerComments는 주석을 저장할 메타데이터 속성입니다.

![workflowcomments1](assets/workflowcomments1.gif)

시스템에서 이 기능을 테스트하려면 다음 단계를 따르십시오.
* [워크플로의 프로세스 단계가 워크플로 설명 캡처를 사용하도록 구성되어 있는지 확인](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue 번들을 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)합니다. 이 번들에는 댓글을 캡처하여 메타데이터 속성으로 저장하는 샘플 코드가 포함되어 있습니다

* [파일 시스템에 이 문서와 관련된 자산을 다운로드하고 압축 해제합니다](assets/capturecomments.zip) 자산에는 워크플로 모델 및 샘플 적응형 양식이 포함되어 있습니다.

* 패키지 관리자를 사용하여 AEM에 zip 파일 2개 가져오기

* [이 URL로 이동하여 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 양식 필드 입력 및 양식 제출

* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)

* 받은 편지함에서 작업을 열고 양식을 제출합니다. 메시지가 표시되면 설명을 입력하십시오.

주석은 AEM 저장소의 `managerComments` 메타데이터 속성에 저장됩니다. 관리자 자격으로 crx에 로그인하는 댓글에 대해 확인합니다. 워크플로 인스턴스는 다음 경로에 저장됩니다.

`/var/workflow/instances/server0`

적절한 워크플로우 인스턴스를 선택하고 메타데이터 노드에서 속성 managerComments를 확인합니다.
