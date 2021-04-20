---
title: 요약 구성 요소 사용자 지정
description: 패키지에서 다음 양식으로 이동하는 기능을 포함하도록 요약 단계 구성 요소를 확장합니다.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 2%

---


# 요약 단계 사용자 정의

요약 단계 구성 요소는 서명한 양식을 다운로드할 수 있는 링크와 함께 양식 제출 요약을 표시하는 데 사용됩니다. 요약 단계는 일반적으로 양식의 마지막 패널에 배치됩니다.
이 사용 목적을 위해 Adobe는 즉시 사용 가능한 요약 구성 요소를 기반으로 새 구성 요소를 만들고 사용자 지정 clientlib을 포함하는 기능을 확장했습니다.

이 구성 요소는 여러 양식에 서명 레이블로 식별됩니다.

다음 스크린샷은 서명식 완료 시 메시지를 표시하기 위해 만들어진 새 구성 요소를 보여줍니다

![요약 구성 요소](assets/summary.PNG)

새 구성 요소는 즉시 사용 가능한 요약 구성 요소를 기반으로 합니다.
![component-prop](assets/componentprop.PNG)

서명을 위해 다음 양식으로 이동하는 단추를 추가했습니다.
![template-code](assets/template-code.PNG)

summary.jsp에는 다음 코드가 있습니다. 범주 id **getnextform**&#x200B;으로 식별되는 클라이언트 라이브러리에 대한 참조가 있습니다.

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## 자산

사용자 지정 요약 구성 요소는 여기에서 [다운로드할 수 있습니다](assets/custom-summary-step.zip)


