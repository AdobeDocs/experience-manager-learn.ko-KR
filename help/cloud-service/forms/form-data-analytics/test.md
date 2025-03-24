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
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# 솔루션 테스트

몇 가지 양식 값 조합을 사용하여 양식을 미리 보고 제출합니다. Adobe Analytics 보고서에서 데이터를 볼 수 있는 데는 몇 분에서 30분까지 걸릴 수 있습니다. prop으로 설정된 데이터는 eVar로 설정된 데이터보다 더 빨리 보고에 표시됩니다.

## 보고서 세트

Adobe Analytics에서 캡처한 양식 데이터는 도넛 형식으로 표시됩니다

**상태별 제출**

![applicantsbystate](assets/donut.png)

필드 유효성 검사 오류

![필드 유효성 검사 오류](assets/donut-field-validation.png)

## 디버깅

적응형 양식이 Adobe Launch 구성이 포함된 것과 동일한 구성 컨테이너를 사용하고 있는지 확인합니다.

양식이 Adobe Analytics에 데이터를 보내고 있는지 확인하려면 다음을 수행하십시오

* 브라우저에서 개발자 도구를 엽니다.
* [콘솔] 패널에서 다음 텍스트를 입력합니다.

```javascript
_satellite.setDebug(true)
```

콘솔 창을 열어 놓고 양식과 상호 작용합니다. 이런 걸 보셔야겠네요

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger 사용

[AEP 디버거 확장](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html)을(를) 브라우저에 추가하여 자세한 디버깅 정보를 확인하십시오

![platform-debugger](assets/platform-debugger.png)

## 축하합니다.

AEM Forms as a Cloud Service과 Adobe Analytics을 통합하여 양식 데이터 필드에 대해 보고했습니다.