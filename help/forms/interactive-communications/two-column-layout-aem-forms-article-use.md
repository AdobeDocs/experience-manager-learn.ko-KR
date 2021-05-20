---
title: 인쇄 채널 문서를 위한 두 개의 열 레이아웃 만들기
seo-title: 인쇄 채널 문서를 위한 두 개의 열 레이아웃 만들기
description: 인쇄 채널 문서용 2열 레이아웃 만들기
seo-description: 인쇄 채널 문서용 2열 레이아웃 만들기
feature: 대화형 통신
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 2%

---


# 인쇄 채널 문서의 두 열 레이아웃

이 짧은 문서에서는 인쇄 채널에서 2개의 열 레이아웃을 만드는 데 필요한 단계를 강조 표시합니다. 사용 사례는 2개의 열 레이아웃과 2개의 열 레이아웃이 있는 2개의 페이지 1과 표준 1개의 열 레이아웃이 있는 2개의 페이지 문서를 생성하는 것입니다.

다음은 AEM Forms 디자이너를 사용하여 2개의 열 레이아웃을 만드는 데 관련된 높은 수준의 단계입니다.

* 1페이지의 마스터 페이지에서 2개의 컨텐츠 영역 만들기
* 두 콘텐츠 영역의 이름을 &quot;왼쪽 열&quot; 및 &quot;오른쪽 열&quot;로 지정합니다
* 하나의 컨텐츠 영역으로 두 번째 마스터 페이지를 만듭니다(기본값)
* 페이지 매김 탭(제목 없는 하위 양식)(페이지 1) 및 (제목 없는 하위 양식)(page2)을 선택하고 아래 스크린샷에 표시된 대로 속성을 설정합니다.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

페이지 매김 속성이 설정되면 하위 양식이나 대상 영역을 (제목 없는 하위 양식) (페이지 1)에 추가할 수 있습니다.

그런 다음 이러한 하위 양식 또는 대상 영역에 문서 조각을 추가할 수 있습니다. 왼쪽 열이 가득 차면 컨텐츠가 오른쪽 열로 이동합니다.

로컬 서버에서 테스트하려면 이 문서와 관련된 자산을 다운로드하십시오. 이 페이지 아래쪽으로 스크롤합니다.

* [패키지 관리자를 사용하여 샘플 인쇄 채널 문서 다운로드 및 설치](assets/print-channel-with-two-column-layout.zip)
* [인쇄 채널 문서 미리 보기](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
