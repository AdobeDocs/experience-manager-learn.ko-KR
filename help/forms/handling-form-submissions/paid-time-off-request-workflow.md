---
title: 간단한 유료 요청 시간 워크플로우
description: AEM 워크플로우에서 적응형 양식 패널 숨기기 및 표시
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: 적응형 양식
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# 간단한 유료 요청 시간 워크플로우

이 문서에서는 유료 시간 할인을 요청하는 데 사용되는 간단한 워크플로우를 살펴봅니다. 비즈니스 요구 사항은 다음과 같습니다.

* 사용자 A는 적응형 양식을 작성하여 시간 초과를 요청합니다.
* 양식이 AEM 관리자 사용자에게 라우팅됩니다(실제로 제출자의 관리자로 라우팅됩니다.)
* 관리자가 양식을 엽니다. 관리자는 제출자가 입력한 정보를 편집할 수 없습니다.
* 승인자 섹션은 승인자에게 표시되어야 합니다(이 경우 AEM 관리 사용자임).

위의 요구 사항을 충족하기 위해 양식에서 **initialstep**&#x200B;이라는 숨겨진 필드를 사용하고 기본값이 Yes로 설정되어 있습니다 양식을 제출하면 워크플로우의 첫 번째 단계에서 초기 단계의 값이 No로 설정됩니다. 양식에는 초기 단계 값에 따라 적절한 섹션을 숨기고 표시하는 비즈니스 규칙이 있습니다.

**AEM 워크플로우를 트리거할 양식 구성**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**워크플로우 연습**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**요청 시간 해제 양식 제출자의 보기**

![초기 단계](assets/initialstep.gif)

**양식의 승인자 보기**

![approverview](assets/approversview.gif)

승인자 보기에서 승인자는 제출된 데이터를 편집할 수 없습니다. 승인자만을 위한 새로운 섹션도 있습니다.

시스템에서 이 워크플로우를 테스트하려면 아래에 언급된 단계를 수행하십시오.
* [DevelopingWithServiceUserBundle 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValue Custom OSGI 번들 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [이 문서와 관련된 자산을 AEM에 가져옵니다.](assets/helpxworkflow.zip)
* [요청 시간 해제 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 엽니다.
* 세부 사항을 입력하고 제출
* [받은 편지함](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)을 엽니다. 새 작업이 할당되었습니다. 양식을 엽니다. 제출자의 데이터는 읽기 전용이어야 하며 새 승인자 섹션이 표시되어야 합니다.
* [워크플로우 모델 탐색](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 프로세스 단계를 탐색합니다. 초기 단계의 값을 아니요로 설정하는 단계입니다.
