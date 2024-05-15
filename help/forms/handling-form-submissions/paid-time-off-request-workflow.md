---
title: 단순 유료 휴무 요청 워크플로
description: AEM Workflow에서 적응형 양식 패널 숨기기 및 표시
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 592
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# 단순 유료 휴무 요청 워크플로

이 문서에서는 유급 휴무 요청에 사용되는 간단한 워크플로우를 살펴봅니다. 비즈니스 요구 사항은 다음과 같습니다.

* 사용자 A가 적응형 양식을 채워 휴무를 요청합니다.
* 양식이 AEM 관리자 사용자에게 라우팅됩니다(실제로 제출자의 관리자에게 라우팅됨).
* 관리자가 양식을 엽니다. 책임자는 제출자가 채운 정보를 편집할 수 없어야 합니다.
* 승인자 섹션은 승인자가 볼 수 있어야 합니다(이 경우 AEM 관리자 사용자).

위의 요구 사항을 충족하기 위해 이라는 숨겨진 필드를 사용합니다. **initialstep** 폼에서 기본 값이 예로 설정되어 있습니다.폼이 제출되면 워크플로우의 첫 번째 단계에서 initialstep의 값을 아니요로 설정합니다. 양식에 initialstep 값을 기반으로 적절한 섹션을 숨기고 표시하는 비즈니스 규칙이 있습니다.

**AEM 워크플로우를 트리거할 양식 구성**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**워크플로우 워크스루**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**휴무 요청 양식 제출자 보기**

![initialstep](assets/initialstep.gif)

**양식의 승인자 보기**

![승인자 보기](assets/approversview.gif)

승인자 보기에서 승인자는 제출된 데이터를 편집할 수 없습니다. 승인자만을 위한 새로운 섹션도 있습니다.

시스템에서 이 워크플로우를 테스트하려면 아래 단계를 따르십시오.
* [DevelopingWitheServiceUserBundle 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValue 사용자 지정 OSGI 번들 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [이 문서와 관련된 자산을 AEM에 가져오기](assets/helpxworkflow.zip)
* 를 엽니다. [휴가 요청 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 세부 정보를 입력하고 제출
* 를 엽니다. [받은 편지함](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). 할당된 새 작업이 표시됩니다. 양식을 엽니다. 제출자의 데이터는 읽기 전용이어야 하며 새 승인자 섹션이 표시되어야 합니다.
* 탐색 [워크플로 모델](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 프로세스 단계를 살펴봅니다. initialstep의 값을 No로 설정하는 단계입니다.
