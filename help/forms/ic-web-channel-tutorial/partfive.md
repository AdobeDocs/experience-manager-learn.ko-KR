---
title: 받는 사람 이름과 주소를 저장할 문서 조각 만들기
seo-title: 받는 사람 이름과 주소를 저장할 문서 조각 만들기
description: '첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 5부분입니다. 이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
seo-description: '첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 5부분입니다. 이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---


# 받는 사람 이름 및 주소 {#creating-document-fragments-to-hold-the-recipient-name-and-address}를 포함하는 문서 조각 만들기

이 부분에서 수신자 이름과 주소를 저장할 문서 조각을 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

문서 조각에는 인터랙티브한 커뮤니케이션 문서의 텍스트 컨텐츠가 들어 있습니다. 이 텍스트 컨텐츠는 정적 텍스트이거나 기본 데이터 모델 요소 값에서 삽입될 수 있습니다. 예: Addy {name}님. 여기서 Deer는 정적 텍스트이고 {name}은(는) 양식 데이터 요소 이름입니다. 런타임 시 이름 요소의 값에 따라 Gloria Rios 또는 Dear John Jacobs에게 해결됩니다.

리치 텍스트 편집기는 비즈니스 사용자가 텍스트를 작성하고 양식 데이터 요소를 삽입할 수 있을 정도로 직관적입니다. 문서 조각 편집기에는 텍스트 서식 지정, 글꼴 유형 및 스타일 지정, 특수 문자 삽입 및 하이퍼링크 생성 기능이 있습니다.

문서 조각 편집기에는 이 [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)에서 설명한 대로 텍스트에 인라인 조건을 삽입할 수도 있습니다

>[!NOTE]
>
>문서 조각에 삽입하는 양식 데이터 모델 요소가 루트 요소의 하위 요소인지 확인합니다. 예를 들어 이 사용 사례에서는 사용자가 선택하는 사용자 객체의 요소가 잔액 객체의 자인지 확인합니다

