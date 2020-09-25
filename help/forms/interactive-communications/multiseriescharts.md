---
title: AEM Forms의 멀티시리즈 차트
seo-title: AEM Forms의 멀티시리즈 차트
description: 적절한 양식 데이터 모델을 작성하여 인쇄 및 웹 채널 문서에서 다양한 시리즈 차트를 만들 수 있습니다.
seo-description: 적절한 양식 데이터 모델을 작성하여 인쇄 및 웹 채널 문서에서 다양한 시리즈 차트를 만들 수 있습니다.
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# 다중 시리즈 차트

AEM Forms 6.5는 여러 시리즈 차트를 만들고 구성하는 기능을 도입했습니다. 여러 시리즈 차트는 일반적으로 라인, 막대, 열 차트 유형과 관련되어 사용됩니다. 다음 차트는 다중 시리즈 차트의 좋은 예입니다. 이 도표는 일정 기간 동안 3개의 서로 다른 뮤추얼 펀드에 1만 달러의 성장률을 보여준다. AEM Forms에서 이러한 종류의 차트를 만들고 사용하려면 적절한 양식 데이터 모델을 만들어야 합니다.

![다중 시리즈](assets/seriescharts.jfif)

AEM Forms에서 다중 시리즈 차트를 만들려면 개체 간의 필수 개체 및 연결을 사용하여 적절한 양식 데이터 모델을 만들어야 합니다. 다음 스크린샷에서는 3개 엔티티 간의 엔티티 및 연결을 강조 표시합니다. 최상위 수준에는 &quot;조직&quot;이라는 개체가 있으며, 이 조직은 기금 실체와의 일대다 관계를 갖습니다. 기금 실체는 차례로 성과 실체와 일대다 관계를 가집니다.

![양식 데이터 모델](assets/formdatamodel.jfif)


## 다중 시리즈 차트에 대한 양식 데이터 모델 생성

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 라인 시리즈 차트 구성

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


시스템에서 테스트하려면 다음 단계를 따르십시오

* [AEM Package Manager를 사용하여 MutualFundFactSheet.zip을 다운로드하고 가져옵니다.](assets/mutualfundfactsheet.zip)
* [하드 드라이브에 SeriesChartSampleData.json을 다운로드합니다.](assets/serieschartsampledata.json) 차트를 채우는 데 사용할 샘플 데이터입니다.
* [Forms 및 문서로 이동합니다.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* &quot;MutualFundGrowthFactSheet&quot; 대화형 통신 템플릿을 부드럽게 선택합니다.
* 미리 보기 클릭 | 샘플 데이터 업로드를 참조하십시오.
* 이 문서의 일부로 제공된 샘플 데이터 파일을 찾습니다.
* 이전 단계에서 다운로드한 샘플 데이터와 함께 &quot;MutualFundGrowthFactSheet&quot; 인터랙티브 커뮤니케이션의 인쇄 채널을 미리 봅니다.
