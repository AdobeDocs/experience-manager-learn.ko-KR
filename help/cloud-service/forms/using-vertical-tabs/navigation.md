---
title: AEM Forms as a Cloud Service에서 세로 탭 사용
description: 세로 탭을 사용하여 적응형 양식 만들기
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 0%

---

# 탭 간 탐색

개별 탭을 클릭하거나 양식의 이전 및 다음 버튼을 사용하여 탭 사이를 탐색할 수 있습니다.
버튼을 사용하여 탐색하려면 양식에 두 개의 버튼을 추가하고 이름을 이전 및 다음 로 지정합니다. 다음 사용자 지정 함수를 단추의 클릭 이벤트와 연결하여 탭 사이를 탐색합니다.

다음은 탭 사이를 이동하는 사용자 지정 함수입니다.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

다음은 다음 및 이전 단추에 대한 규칙 편집기입니다

**다음 단추**

![다음 단추](assets/next-button.png)

**이전 단추**

![이전 단추](assets/prev-button.png)
