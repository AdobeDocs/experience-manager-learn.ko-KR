---
title: 재사용 가능한 AEM Forms 워크플로 모델
description: 적응형 Forms과 독립적인 워크플로우 모델을 만드는 방법을 알아봅니다.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 재사용 가능한 AEM Forms 워크플로 모델 만들기{#create-re-usable-aem-forms-workflow-models}

AEM Forms 6.5 릴리스부터 특정 적응형 양식에 연결되지 않은 워크플로 모델을 만들 수 있습니다. 이제 이 기능을 사용하여 다른 적응형 양식 제출 시 호출할 수 있는 워크플로우 모델을 한 개 만들 수 있습니다. 이 기능을 사용하면 검토 및 승인을 위한 모든 적응형 양식 제출을 처리하는 일반 워크플로우를 가질 수 있습니다.

이러한 워크플로우를 디자인하려면 다음 단계를 수행하십시오

1. AEM에 로그인
1. 브라우저를 [워크플로 모델](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)(으)로 지정
1. 워크플로 모델을 추가하려면 __만들기 > 모델 만들기__&#x200B;를 클릭하십시오.
1. 워크플로우 모델에 적절한 이름 및 제목 을 입력한 다음 완료 를 클릭합니다
1. 새로 만든 모델을 편집 모드로 엽니다.
1. 작업 할당 구성 요소를 드래그하여 워크플로 모델에 놓기
1. 작업 할당 구성 요소의 구성 속성을 엽니다.
1. Forms 및 문서 탭
1. 유형 - 적응형 양식 또는 읽기 전용 적응형 양식을 선택합니다.

양식 경로를 지정하는 방법에는 세 가지가 있습니다

1. 절대 경로에서 사용 가능 - 워크플로우가 적응형 양식과 밀접하게 연결되어 있음을 의미합니다. 이건 우리가 원하는 게 아니에요
1. **워크플로우에 제출됨** - 적응형 양식이 제출되면 워크플로우 엔진이 제출된 데이터에서 양식 이름을 추출함을 의미합니다. 선택해야 하는 옵션입니다.
1. 변수의 경로에서 사용 가능 - 적응형 양식이 워크플로우 변수에서 선택됨을 의미합니다
다음 스크린샷은 적응형 양식에서 워크플로 결합 해제를 위해 선택해야 하는 올바른 옵션을 보여 줍니다

![다시 사용할 수 있는 AEM Forms 워크플로 모델](assets/workflomodel.PNG)
