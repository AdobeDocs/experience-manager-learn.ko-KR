---
title: 재사용 가능한 AEM Forms 워크플로우 모델 제작
seo-title: 재사용 가능한 AEM Forms 워크플로우 모델 제작
description: 적응형 Forms에 영향을 받지 않는 워크플로우 모델
seo-description: 적응형 Forms에 영향을 받지 않는 워크플로우 모델
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---


# 재사용 가능한 AEM Forms 워크플로우 모델 만들기{#create-re-usable-aem-forms-workflow-models}

이제 AEM Forms 6.5 릴리스부터 특정 적응형 양식과 연결되지 않은 워크플로우 모델을 만들 수 있습니다. 이제 이 기능을 사용하여 다양한 적응형 양식 제출 시 호출할 수 있는 하나의 워크플로우 모델을 만들 수 있습니다. 이 기능을 사용하면 검토 및 승인을 위해 모든 적응형 양식 제출을 처리하는 일반적인 워크플로우가 있을 수 있습니다.

이러한 워크플로우를 디자인하려면 다음 단계를 수행하십시오

1. AEM에 로그인
1. 브라우저를 [워크플로우 모델](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)으로 지정합니다.
1. 만들기를 클릭합니다 | 모델 만들어 워크플로우 모델 추가
1. 워크플로우 모델에 적절한 이름 및 제목을 입력한 다음 완료를 클릭합니다
1. 편집 모드에서 새로 만든 모델 열기
1. 작업 지정 구성 요소를 워크플로우 모델에 드래그 앤 드롭
1. 작업 구성 요소의 구성 속성을 엽니다.
1. 탭 - Forms 및 문서 탭
1. 유형 - 응용 양식 또는 읽기 전용 응용 양식을 선택합니다.

양식 경로를 지정하는 방법에는 3가지가 있습니다

1. 절대 경로에서 사용 가능 - 즉, 워크플로우는 적응형 양식과 밀접하게 결합됩니다. 이건 우리가 원하는 게 아니야
1. **워크플로우에 제출됨**  - 응용 양식이 제출되면 워크플로우 엔진은 제출된 데이터에서 양식 이름을 추출합니다. 이 옵션을 선택해야 합니다.
1. 변수의 경로에서 사용 가능 - 이것은 적응형 양식이 워크플로우 변수에서 선택됨을 의미합니다
다음 스크린샷은 적응형 양식의 연결 해제 워크플로우를 선택해야 하는 올바른 옵션을 보여줍니다

![워크플로모델](assets/workflomodel.PNG)