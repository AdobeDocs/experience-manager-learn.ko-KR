---
title: 투자 조합 패널 구성
description: 첫 번째 대화형 커뮤니케이션 문서 작성을 위한 11단계 자습서입니다. 이 부분에서는 원형 차트를 추가하여 현재 및 모델 투자 조합을 표시합니다.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 78
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# 투자 조합 패널 구성

이 부분에서는 파이 차트를 추가하여 현재 및 모델 투자 믹스를 표시하겠습니다.

* AEM Forms에 로그인하고 Adobe Experience Manager > Forms > Forms 및 문서로 이동합니다.

* 401KStation 폴더를 엽니다.

* 편집 모드로 401KStation을 엽니다.

* 계정 보유자의 현재 및 모델 투자 혼합을 나타내는 파이 차트 2개를 추가합니다.

## 현재 자산 조합 {#current-asset-mix}

* 오른쪽의 &quot;CurrentAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택한 다음 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;현재 에셋 혼합&quot;으로 변경합니다.

* &quot;CurrentAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택한 다음 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 탭하고 &quot;렌치&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X축과 Y축에 바인딩된 데이터 모델 개체를 참고하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 적절한 요소를 선택해야 합니다.

* ![currentassetmix](assets/currentassetmixchart.png)

## 모델 자산 조합 {#model-asset-mix}

* 오른쪽의 &quot;RecommendedAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택한 다음 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;모델 자산 혼합&quot;으로 변경합니다.

* &quot;RecommendedAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택한 다음 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 탭하고 &quot;렌치&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X축과 Y축에 바인딩된 데이터 모델 개체를 참고하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 적절한 요소를 선택해야 합니다.

* ![assettype](assets/modelassettypechart.png)

## 다음 단계

[웹 채널 문서 게재 준비](./parttwelve.md)
