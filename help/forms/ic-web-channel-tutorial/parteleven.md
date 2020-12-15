---
title: 투자 혼합 패널 구성
seo-title: 투자 혼합 패널 구성
description: 첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 11부분입니다. 이 부분에서는 현재 및 모델 투자 조합을 표시하는 파이 차트를 추가하겠습니다.
seo-description: 첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 11부분입니다. 이 부분에서는 현재 및 모델 투자 조합을 표시하는 파이 차트를 추가하겠습니다.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---


# 투자 혼합 패널 구성

이 부분에서는 현재 및 모델 투자 조합을 표시하는 파이 차트를 추가할 것입니다.

* AEM Forms에 로그인하고 Adobe Experience Manager > Forms > Forms 및 문서로 이동합니다.

* 401KStutement 폴더를 엽니다.

* 편집 모드에서 401KStatement를 엽니다.

* 계정 소유자의 현재 및 모델 투자 조합을 나타내는 2개의 파이 차트를 추가할 예정입니다.

## 현재 자산 혼합 {#current-asset-mix}

* 오른쪽의 &quot;CurrentAssetMix&quot; 패널을 누르고 &quot;+&quot; 아이콘을 선택하고 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;현재 에셋 혼합&quot;으로 변경합니다.

* &quot;CurrentAssetMix&quot; 패널을 누르고 &quot;+&quot; 아이콘을 선택하고 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 누르고 &quot;렌치&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X 축과 Y 축에 바인딩된 데이터 모델 개체를 주목하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 해당 요소를 선택해야 합니다.

* ![curtassetmix](assets/currentassetmixchart.png)

## 모델 자산 혼합 {#model-asset-mix}

* 오른쪽의 &quot;RecommendedAssetMix&quot; 패널을 누르고 &quot;+&quot; 아이콘을 선택하고 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;모델 에셋 믹스&quot;로 변경합니다.

* &quot;RecommendedAssetMix&quot; 패널을 누르고 &quot;+&quot; 아이콘을 선택하고 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 누르고 &quot;렌치&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X 축과 Y 축에 바인딩된 데이터 모델 개체를 주목하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 해당 요소를 선택해야 합니다.

* ![assettype](assets/modelassettypechart.png)

