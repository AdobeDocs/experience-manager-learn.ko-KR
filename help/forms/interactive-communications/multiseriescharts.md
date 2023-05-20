---
title: AEM Forms의 다중 시리즈 차트
description: 적절한 양식 데이터 모델을 만들어 인쇄 및 웹 채널 문서에 다중 시리즈 차트를 만듭니다.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 다중 시리즈 차트

AEM Forms 6.5에는 여러 시리즈 차트를 만들고 구성하는 기능이 도입되었습니다. 여러 계열 차트는 일반적으로 라인, 막대, 열 차트 유형과 관련하여 사용됩니다. 다음 차트는 다중 계열 차트의 좋은 예입니다. 이 차트는 일정 기간 동안 3개의 다른 뮤추얼 펀드에서 미화 10,000달러의 성장을 보여줍니다. AEM Forms에서 이러한 종류의 차트를 만들고 사용하려면 적절한 양식 데이터 모델을 만들어야 합니다.

![다중 계열 차트](assets/seriescharts.jfif)

AEM Forms에서 다중 시리즈 차트를 만들려면 필요한 엔터티와 엔터티 간의 연결을 사용하여 적절한 양식 데이터 모델을 만들어야 합니다. 다음 스크린샷에서는 엔티티와 세 엔티티 간의 연관을 강조 표시합니다. 최상층에는 &#39;조직&#39;이라는 조직이 있는데, 이 조직은 펀드실체와 일대다 연계를 하고 있다. 펀드 엔티티는 결과적으로 퍼포먼스 엔티티와 일대다 관계를 맺고 있습니다.

![양식 데이터 모델](assets/formdatamodel.jfif)

## 다중 계열 차트에 대한 양식 데이터 모델 만들기

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### Line 시리즈 차트 구성

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

시스템에서 테스트하려면 다음 단계를 따르십시오

* [AEM Package Manager를 사용하여 MutualFundFactSheet.zip을 다운로드하고 가져옵니다.](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.json을 하드 드라이브에 다운로드합니다.](assets/serieschartsampledata.json) 차트를 채우는 데 사용되는 샘플 데이터입니다.
* [Forms 및 문서로 이동합니다.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* &quot;MutualFundGrowthFactSheet&quot; 대화형 커뮤니케이션 템플릿을 부드럽게 선택하십시오.
* 미리보기 클릭 | 인쇄 채널 | 샘플 데이터 업로드
* 이 문서의 일부로 제공된 샘플 데이터 파일을 찾아봅니다.
* 이전 단계에서 다운로드한 샘플 데이터로 &quot;MutualFundGrowthFactSheet&quot; 대화형 통신의 인쇄 채널을 미리 봅니다.
