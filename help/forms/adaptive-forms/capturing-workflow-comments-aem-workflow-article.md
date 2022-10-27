---
title: 적응형 Forms Workflow에서 워크플로우 댓글 캡처
description: AEM 워크플로우에서 워크플로우 주석 캡처
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---

# 적응형 Forms Workflow에서 워크플로우 댓글 캡처{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms 6.4에만 적용됩니다. AEM Forms 6.5에서는 변수 기능을 사용하여 이 사용 사례를 수행하십시오]

일반적인 요청은 작업 검토자가 전자 메일에 입력한 주석을 포함하는 기능입니다. AEM Forms 6.4에는 사용자가 입력한 주석을 캡처하고 전자 메일에 이러한 주석을 포함하는 기본 제공 메커니즘이 없습니다.

이 요구 사항을 충족하기 위해 주석을 캡처하고 이러한 주석을 워크플로우 메타데이터 속성으로 저장하는 데 사용할 수 있는 샘플 OSGi 번들이 제공됩니다.

다음 스크린샷에서는 의 프로세스 단계를 사용하는 방법을 보여 줍니다 [AEM 워크플로우](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) 주석을 캡처하고 메타데이터 속성으로 저장합니다. Capture Workflow Comments는 프로세스 단계에서 사용해야 하는 Java 클래스의 이름입니다. 주석이 포함될 메타데이터 속성 이름을 전달해야 합니다. 아래 스크린샷에서 managerComments는 주석을 저장할 메타데이터 속성입니다.

![workflowcomments1](assets/workflowcomments1.gif)

시스템에서 이 기능을 테스트하려면 다음 단계를 수행하십시오.
* [워크플로우의 프로세스 단계가 캡처 워크플로우 주석을 사용하도록 구성되어 있는지 확인합니다](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue 번들 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 이 번들에는 주석을 캡처하고 메타데이터 속성으로 저장하는 샘플 코드가 포함되어 있습니다

* [이 문서와 관련된 자산을 파일 시스템에서 다운로드 및 압축 해제합니다](assets/capturecomments.zip) 자산에는 워크플로우 모델과 샘플 적응형 양식이 포함되어 있습니다.

* 패키지 관리자를 사용하여 2개의 zip 파일을 AEM에 가져옵니다.

* [이 URL로 이동하여 양식을 미리 봅니다](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 양식 필드를 작성하고 양식을 제출합니다

* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)

* 받은 편지함에서 작업을 열고 양식을 제출합니다. 메시지가 표시되면 몇 가지 주석을 입력하십시오.

주석은 `managerComments` AEM 저장소 내 아래에 표시됩니다. 주석이 crx에 관리자로 로그인하는지 확인하려면 다음을 수행하십시오. 워크플로우 인스턴스는 다음 경로에 저장됩니다.

`/var/workflow/instances/server0`

적절한 워크플로우 인스턴스를 선택하고 메타데이터 노드에서 속성 managerComments를 확인합니다.
