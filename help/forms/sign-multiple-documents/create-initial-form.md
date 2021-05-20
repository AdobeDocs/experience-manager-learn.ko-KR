---
title: 프로세스를 트리거할 초기 양식 만들기
description: 전자 메일 알림을 트리거하여 서명 프로세스를 시작하는 초기 양식을 만듭니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: 개발
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 5%

---


# 초기 양식 만들기

초기 양식(Refinance 양식)은 **여러 Forms 서명** AEM 워크플로우를 트리거하여 여러 양식에 서명하는 데 사용됩니다. 선택한 값을 입력할 수 있지만 다음 필드가 양식에 추가되어 있는지 확인하십시오.



| 필드 유형 | 이름 | 목적 | 숨김 | 기본 값 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| 텍스트 필드 | 서명 | 서명 상태를 표시하려면 | Y | N |
| 텍스트 필드 | guid | 양식을 고유하게 식별하려면 | Y | 3889년 |
| 텍스트 필드 | customerName | 고객 이름을 캡처하려면 | N |
| 텍스트 필드 | customerEmail | 알림을 전송할 고객 이메일 | N |
| CheckBox | formsToSign | 이 항목은 패키지의 양식을 식별합니다 | N |



초기 양식은 **signmultiforms**라는 AEM 워크플로우를 트리거하도록 구성해야 합니다.
데이터 파일 경로가 **Data.xml**&#x200B;로 설정되어 있는지 확인합니다. 이 작업은 샘플 코드가 양식을 제출하는 프로세스에서 페이로드에서 Data.xml이라는 파일을 찾으므로 매우 중요합니다.

## 자산

초기 양식(재정렬 양식)은 여기에서 [다운로드할 수 있습니다.](assets/refinance-form.zip)





