---
title: 인쇄 채널 문서를 위한 두 개의 열 레이아웃 만들기
seo-title: 인쇄 채널 문서를 위한 두 개의 열 레이아웃 만들기
description: 인쇄 채널 문서를 위한 2개의 열 레이아웃 만들기
seo-description: 인쇄 채널 문서를 위한 2개의 열 레이아웃 만들기
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 314f798f7a80f9c554e5bea052f8a64ae397d0de
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# 인쇄 채널 문서의 두 열 레이아웃

이 짧은 문서에서는 인쇄 채널에서 2개의 열 레이아웃을 만드는 데 필요한 단계를 강조 표시합니다. 사용 사례는 2개의 열 레이아웃과 2페이지의 표준 1개 열 레이아웃이 있는 2개의 페이지 문서를 생성하는 것입니다.

다음은 AEM Forms 디자이너를 사용하여 2개의 열 레이아웃을 만드는 것과 관련된 높은 수준의 단계입니다.

* 1페이지 마스터 페이지에서 2개의 컨텐츠 영역 만들기
* 2개의 컨텐츠 영역의 이름을 &quot;왼쪽 열&quot; 및 &quot;오른쪽 열&quot;로 지정합니다.
* 하나의 컨텐츠 영역을 사용하여 두 번째 마스터 페이지 만들기(기본값)
* 페이지 매김 탭(제목 없는 하위 폼 페이지)(페이지 1) 및(제목이 없는 하위 폼)(페이지2)을 선택하고 아래 스크린샷에 표시된 대로 속성을 설정합니다.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

페이지 매김 속성이 설정되면 하위 폼 또는 대상 영역을 (제목이 없는 하위 폼)(페이지 1)에 추가할 수 있습니다.

그런 다음 이러한 하위 양식 또는 대상 영역에 문서 조각을 추가할 수 있습니다. 왼쪽 열이 가득 차면 컨텐츠가 오른쪽 열로 이동합니다.

로컬 서버에서 테스트하려면 이 아티클과 관련된 에셋을 다운로드하십시오. 이 페이지 아래쪽으로 스크롤

* [패키지 관리자를 사용한 샘플 인쇄 채널 문서 다운로드 및 설치](assets/print-channel-with-two-column-layout.zip)
* [인쇄 채널 문서 미리 보기](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
