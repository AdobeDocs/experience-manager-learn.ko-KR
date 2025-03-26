---
title: AEM Forms과 Marketo(4부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 방법에 대한 자습서입니다.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---

# 통합 테스트

간단한 양식 가져오기를 만들어 Market에서 Lead 객체를 표시하여 통합을 테스트합니다.
>[!NOTE]
>
>이 기능은 기초 구성 요소를 기반으로 양식을 기반으로 테스트되었습니다.

## 적응형 양식 만들기

1. 적응형 양식을 만들고 &quot;빈 양식 템플릿&quot;을 기반으로 이전 단계에서 만든 양식 데이터 모델과 연결합니다.
1. 편집 모드에서 양식을 엽니다.
1. TextField 구성 요소와 패널 구성 요소를 적응형 양식으로 드래그 앤 드롭합니다. TextField 구성 요소의 제목을 &quot;리드 ID 입력&quot;으로 설정하고 이름을 &quot;리드 ID&quot;로 설정합니다.
1. 2개의 TextField 구성 요소를 패널 구성 요소로 끌어다 놓습니다.
1. 두 Textfield 구성 요소의 이름 및 제목을 이름 및 성 로 설정합니다.
1. 최소값을 1로 설정하고 최대값을 -1로 설정하여 패널 구성 요소를 반복 가능한 구성 요소로 구성합니다. Marketo 서비스가 리드 오브젝트 배열을 반환하고 결과를 표시하려면 반복 가능한 구성 요소가 있어야 하므로 이 작업이 필요합니다. 그러나 이 경우 Lead 객체의 ID로 Lead 객체를 검색하므로 Lead 객체는 하나만 다시 가져옵니다.
1. 아래 이미지에 표시된 대로 LeadId 필드에 규칙을 만듭니다.
1. 양식을 미리 보고 LeadID 필드에 유효한 Lead ID를 입력한 다음 Tab out 을 클릭합니다. 이름 및 성 필드는 서비스 호출 결과로 채워집니다.

다음 스크린샷에서는 규칙 편집기 설정에 대해 설명합니다

![ruleeditor](assets/ruleeditor.png)


## 축하합니다.

AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합했습니다.