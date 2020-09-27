---
title: 문서 조각 만들기
description: '첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 5부분입니다. 이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
seo-description: '첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 5부분입니다. 이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# 문서 조각 만들기

이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

문서 조각에는 인터랙티브한 커뮤니케이션 문서의 텍스트 컨텐츠가 들어 있습니다. 이 텍스트 컨텐츠는 정적 텍스트이거나 기본 데이터 모델 요소 값에서 삽입될 수 있습니다. 예를 들어 **Addy _{name}_**(으)로 설정되며 Deer는 정적 텍스트이고 name은 양식 데이터 모델 요소 이름입니다. 런타임 시 이름 요소의 값에 따라&#x200B;**Gloria Rios**또는&#x200B;**Dear John Jacobs**에게 해결됩니다.

리치 텍스트 편집기는 비즈니스 사용자가 텍스트를 작성하고 양식 데이터 요소를 삽입할 수 있을 정도로 직관적입니다. 문서 조각 편집기에는 텍스트 서식 지정, 글꼴 유형 및 스타일 지정, 특수 문자 삽입 및 하이퍼링크 생성 기능이 있습니다.

문서 조각 편집기에서는 이 [비디오에서 설명한 대로 텍스트에 인라인 조건을 삽입할 수도 있습니다](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>문서 조각에 삽입하는 양식 데이터 모델 요소가 루트 요소의 하위 요소인지 확인합니다. 예를 들어 이 사용 사례에서는 사용자가 선택하는 사용자 객체의 요소가 잔액 객체의 자인지 확인합니다

