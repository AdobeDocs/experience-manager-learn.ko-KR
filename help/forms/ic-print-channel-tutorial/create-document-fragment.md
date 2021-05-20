---
title: 문서 조각 만들기
description: '첫 번째 대화형 통신 문서를 만들기 위한 여러 단계 자습서의 5부분입니다. 이 부분에서는 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
seo-description: '첫 번째 대화형 통신 문서를 만들기 위한 여러 단계 자습서의 5부분입니다. 이 부분에서는 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---


# 문서 조각 만들기

이 부분에서는 수신자 이름과 주소를 저장할 문서 조각을 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

문서 조각은 대화형 통신 문서의 텍스트 컨텐츠를 포함합니다. 이 텍스트 컨텐츠는 정적 텍스트이거나 기본 데이터 모델 요소 값에서 삽입할 수 있습니다. 예를 들어 **Dee _{name}_**입니다. 여기서 Dee는 정적 텍스트이고 이름은 양식 데이터 모델 요소 이름입니다. 런타임 시, 이 옵션은 이름 요소의 값에 따라&#x200B;**Grey Gloria Rios**또는&#x200B;**Deep John Jacobs**로 확인됩니다.

리치 텍스트 편집기는 비즈니스 사용자가 텍스트를 작성하고 양식 데이터 요소를 삽입할 수 있을 만큼 직관적입니다. 문서 조각 편집기에서는 텍스트 서식을 지정하고, 글꼴 유형과 스타일을 지정하고, 특수 문자를 삽입하고 하이퍼링크를 만들 수 있습니다.

문서 조각 편집기에서도 이 [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)에 설명된 대로 텍스트에 인라인 조건을 삽입할 수 있습니다

>[!NOTE]
>
>문서 조각에 삽입하는 양식 데이터 모델 요소가 루트 요소의 하위 요소인지 확인합니다. 예를 들어, 이 사용 사례에서는 사용자가 선택하는 사용자 객체의 요소가 잔액 객체의 하위 항목인지 확인합니다

