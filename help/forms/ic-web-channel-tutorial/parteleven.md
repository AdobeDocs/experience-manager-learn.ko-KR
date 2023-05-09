---
title: 투자 혼합 패널 구성
seo-title: Configuring Investment Mix Panel
description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서 11의 일부입니다.이 부분에서는 현재 및 모델 투자 조합을 표시하기 위해 파이 차트를 추가하겠습니다.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 투자 혼합 패널 구성

이 부분에서는 현재 및 모델 투자 혼합물을 표시할 파이 차트를 추가하겠습니다.

* AEM Forms에 로그인하고 Adobe Experience Manager > Forms > Forms 및 문서로 이동합니다.

* 401KStutement 폴더를 엽니다.

* 편집 모드에서 401KStutement를 엽니다.

* 2개의 파이 차트로 계좌 보유자의 현재 및 모델 투자 조합을 대표할 것입니다.

## 현재 자산 혼합 {#current-asset-mix}

* 오른쪽의 &quot;CurrentAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택하고 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;현재 자산 혼합&quot;으로 변경합니다.

* CurrentAssetMix 패널을 탭하고 &quot;+&quot; 아이콘을 선택하고 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 탭하고 &quot;공구모양&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X축과 Y축에 바인딩된 데이터 모델 개체를 참고하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 해당 요소를 선택해야 합니다.

* ![curtassetmix](assets/currentassetmixchart.png)

## 모델 자산 믹스 {#model-asset-mix}

* 오른쪽의 &quot;RecommendedAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택하고 텍스트 구성 요소를 삽입합니다. 기본 텍스트를 &quot;모델 자산 믹스&quot;로 변경합니다.

* &quot;RecommendedAssetMix&quot; 패널을 탭하고 &quot;+&quot; 아이콘을 선택하고 차트 구성 요소를 삽입합니다. 새로 삽입된 차트 구성 요소를 탭하고 &quot;공구모양&quot; 아이콘을 클릭하여 차트에 대한 구성 속성 시트를 엽니다.

* 아래 이미지에 표시된 대로 속성을 설정합니다. 차트 유형이 파이 차트인지 확인합니다.

* X축과 Y축에 바인딩된 데이터 모델 개체를 참고하십시오. 양식 데이터 모델의 루트 요소를 선택한 다음 드릴다운하여 해당 요소를 선택해야 합니다.

* ![asettype](assets/modelassettypechart.png)

## 다음 단계

[웹 채널 문서 제공 준비](./parttwelve.md)
