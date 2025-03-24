---
title: Adobe Analytics을 사용하여 제출된 양식 데이터 필드에 대한 보고서
description: AEM Forms CS와 Adobe Analytics을 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# 규칙 정의

Tags 속성에서 2개의 새 [규칙](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html)&#x200B;(**필드 유효성 검사 오류 및 FormSubmit**)을 만들었습니다.

![적응형 양식](assets/rules.png)


## 필드 유효성 검사 오류

**필드 유효성 검사 오류** 규칙은 적응형 양식 필드에 유효성 검사 오류가 있을 때마다 트리거됩니다. 예를 들어, 전화 번호나 이메일이 예상 형식이 아닌 경우 유효성 검사 오류 메시지가 표시됩니다.

스크린샷과 같이 이벤트를 _**Adobe Experience Manager Forms-Error**_(으)로 설정하여 필드 유효성 검사 오류 규칙을 구성합니다.



![지원자-주-거주](assets/field_validation_error_rule.png)

Adobe Analytics - 변수 설정 은 다음과 같이 구성됩니다

![작업 설정](assets/field_validation_action_rule.png)

## 양식 제출 규칙

적응형 양식이 성공적으로 제출될 때마다 양식 제출 규칙이 트리거됩니다.

양식 제출 규칙이 _**Adobe Experience Manager Forms - 제출**_ 이벤트를 사용하여 구성되었습니다.

![form-submit-rule](assets/form-submit-rule.png)

양식 제출 규칙에서 데이터 요소 _**AppliancesStateOfResidence**_&#x200B;의 값은 prop5에 매핑되고 데이터 요소 FormTitle의 값은 prop8에 매핑됩니다.

Adobe Analytics - 변수 설정은 다음과 같이 구성됩니다.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

태그 코드를 테스트할 준비가 되면 게시 플로우를 사용하여 [태그에 대한 변경 사항을 게시](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html)합니다.

## 다음 단계

[솔루션 테스트](./test.md)