---
title: Adobe Analytics을 사용하여 제출된 양식 데이터 필드에 대해 보고합니다
description: AEM Forms CS를 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 규칙 정의

Tags 속성에서 두 개를 새로 만들었습니다 [규칙](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**필드 유효성 검사 오류 및 FormSubmit**).

![적응형 양식](assets/rules.png)


## 필드 유효성 검사 오류

다음 **필드 유효성 검사 오류** 적응형 양식 필드에 유효성 검사 오류가 있을 때마다 규칙이 트리거됩니다. 예를 들어, 전화 번호나 이메일이 예상 형식이 아닌 경우, 유효성 검사 오류 메시지가 표시됩니다.

Field Validation Error 규칙은 이벤트를 _**Adobe Experience Manager Forms-오류**_ 스크린샷에 표시된 대로



![신청자](assets/field_validation_error_rule.png)

Adobe Analytics - 변수 설정은 다음과 같이 구성됩니다

![작업 설정](assets/field_validation_action_rule.png)

## 양식 제출 규칙

양식 제출 규칙은 적응형 양식이 성공적으로 제출될 때마다 트리거됩니다.

양식 제출 규칙은 _**Adobe Experience Manager Forms - 제출**_ 이벤트

![form-submit-rule](assets/form-submit-rule.png)

양식 제출 규칙에서 데이터 요소의 값 _**AppersionsStateOfResidence**_ 가 prop5에 매핑되고 데이터 요소 FormTitle 값이 prop8에 매핑됩니다.

Adobe Analytics - 변수 설정은 다음과 같이 구성됩니다.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

태그 코드를 테스트할 준비가 되면[태그에 대한 변경 사항 게시](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 게시 흐름 사용.
