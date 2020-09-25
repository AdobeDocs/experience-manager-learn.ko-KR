---
title: AEM Forms(Markto 포함)(4부)
seo-title: AEM Forms(Markto 포함)(4부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketing과 통합하는 자습서입니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketing과 통합하는 자습서입니다.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# 양식 데이터 모델을 사용하여 응용 양식 만들기

다음 단계는 응용 양식을 만들고 이전 단계에서 만든 양식 데이터 모델을 기반으로 하는 것입니다.
사용자는 리드 ID를 입력하고 Marketing To 서비스를 탭하여 id로 리드를 가져옵니다. 그런 다음 서비스 작업 결과가 응용 Forms의 해당 필드에 매핑됩니다.

1. 적응형 양식을 만들고 &quot;빈 양식 템플릿&quot;을 기반으로 하여 이전 단계에서 만든 양식 데이터 모델과 연결합니다.
1. 편집 모드에서 양식 열기
1. TextField 구성 요소와 패널 구성 요소를 응용 양식으로 드래그하여 놓습니다. TextField 구성 요소 &quot;리드 ID 입력&quot;의 제목을 설정하고 해당 이름을 &quot;LeadId&quot;로 설정합니다
1. 2개의 TextField 구성 요소를 패널 구성 요소로 드래그하여 놓기
1. 2개의 Textfield 구성 요소의 이름과 제목을 FirstName 및 LastName으로 설정합니다.
1. 최소값을 1로, 최대값을 -1로 설정하여 패널 구성 요소를 반복 가능한 구성 요소로 구성합니다. 이는 Marketing To 서비스가 리드 개체 배열을 반환하고 결과를 표시하려면 반복 가능한 구성 요소가 있어야 하기 때문에 필요합니다. 하지만 이 경우 리드 개체를 ID로 검색하고 있으므로 하나의 리드 개체만 다시 가져올 수 있습니다.
1. 아래 이미지에 표시된 대로 리드 ID 필드에 규칙을 만듭니다.
1. 양식을 미리 보고 리드 ID 필드에 유효한 리드 ID를 입력하고 탭을 탭합니다. 이름 및 성 필드는 서비스 호출 결과로 채워져야 합니다.

다음 스크린샷은 규칙 편집기 설정에 대해 설명합니다

![규칙 편집기](assets/ruleeditor.jfif)
