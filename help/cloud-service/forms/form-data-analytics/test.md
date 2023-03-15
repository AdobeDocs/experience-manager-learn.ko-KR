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
source-wordcount: '180'
ht-degree: 1%

---

# 솔루션 테스트

양식 값의 조합을 사용하여 양식을 미리 보고 제출합니다. Adobe Analytics 보고서에서 데이터를 보려면 몇 분에서 30분이 걸립니다. prop으로 설정된 데이터 세트는 데이터 세트가 eVar로 설정된 것보다 빨리 보고에 표시됩니다.

## 보고서 세트

Adobe Analytics으로 캡처된 양식 데이터는 도넛 형식으로 표시됩니다

**상태별 제출**

![applicationBystate](assets/donut.png)

필드 유효성 검사 오류

![field-validation-error](assets/donut-field-validation.png)

## 디버깅

적응형 양식이 Adobe Launch 구성을 포함하는 동일한 구성 컨테이너를 사용하고 있는지 확인합니다.

양식에서 Adobe Analytics으로 데이터를 보내고 있는지 확인하려면 다음을 수행하십시오

* 브라우저에서 개발자 도구를 엽니다.
* [콘솔] 패널에서 다음 텍스트를 입력합니다.

```javascript
_satellite.setDebug(true)
```

콘솔 창을 열어 둔 채로 양식과 상호 작용합니다. 이런 걸 보시면 됩니다

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger 사용

추가 [AEP 디버거 확장](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) 자세한 디버깅 정보를 얻기 위해 브라우저에(로그인해야 함)

![platform debugger](assets/platform-debugger.png)





