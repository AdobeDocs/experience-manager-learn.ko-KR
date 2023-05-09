---
title: 프로세스를 트리거할 초기 양식 만들기
description: 전자 메일 알림을 트리거하여 서명 프로세스를 시작하는 초기 양식을 만듭니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# 초기 양식 만들기

초기 양식(Refinance 양식)은 **여러 Forms 서명** AEM 워크플로우입니다. 선택한 값을 입력할 수 있지만 다음 필드가 양식에 추가되어 있는지 확인하십시오.

| 필드 유형 | 이름 | 용도 | 숨김 | 기본 값 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| 텍스트 필드 | 서명 | 서명 상태를 표시하려면 | Y | N |
| 텍스트 필드 | guid | 양식을 고유하게 식별하려면 | Y | 3889 |
| 텍스트 필드 | customerName | 고객 이름을 캡처하려면 | N |
| 텍스트 필드 | customerEmail | 알림을 전송할 고객 이메일 | N |
| CheckBox | formsToSign | 이 항목은 패키지의 양식을 식별합니다 | N |

이라는 AEM 워크플로우를 트리거하려면 초기 양식을 구성해야 합니다. **여러 양식**
데이터 파일 경로가 로 설정되어 있는지 확인합니다. **Data.xml**. 이 작업은 샘플 코드가 양식을 제출하는 프로세스에서 페이로드에서 Data.xml이라는 파일을 찾으므로 매우 중요합니다.

## Assets

초기 양식(재재무 양식)은 [여기에서 다운로드](assets/refinance-form.zip)

## 다음 단계

[서명에 사용할 양식 만들기](./create-forms-for-signing.md)
