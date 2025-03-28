---
title: 프로세스를 트리거할 초기 양식 만들기
description: 서명 프로세스를 시작하기 위해 전자 메일 알림을 트리거할 초기 양식을 만듭니다.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 9%

---

# 초기 양식 만들기

초기 양식(리파이낸스 양식)은 **여러 Forms 서명** AEM 워크플로우를 트리거하여 여러 양식에 서명하는 데 사용됩니다. 선택한 값을 입력할 수 있지만 양식에 다음 필드가 추가되었는지 확인합니다.

| 필드 유형 | 이름 | 용도 | 숨김 | 기본 값 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| 텍스트 필드 | 서명됨 | 서명 상태를 표시하려면 | Y | N |
| 텍스트 필드 | guid | 양식을 고유하게 식별하려면 | Y | 3889 |
| 텍스트 필드 | customerName | 고객 이름을 캡처하려면 | N |
| 텍스트 필드 | customerEmail | 알림을 보낼 고객 이메일 | N |
| 확인란 | formsTosign | 항목은 패키지의 양식을 식별합니다 | N |

**signmultipleforms**(이)라는 AEM 워크플로우를 트리거하려면 초기 양식을 구성해야 합니다.
데이터 파일 경로가 **Data.xml**(으)로 설정되어 있는지 확인하십시오. 이는 샘플 코드가 양식 제출을 처리하는 페이로드에서 Data.xml이라는 파일을 검색하기 때문에 매우 중요합니다.

## 자산

초기 양식(리파이낸스 양식)은 [여기에서 다운로드](assets/refinance-form.zip)할 수 있습니다.

## 다음 단계

[서명에 사용할 양식 만들기](./create-forms-for-signing.md)
