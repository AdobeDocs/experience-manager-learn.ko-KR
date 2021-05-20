---
title: AEM Forms의 다중 시리즈 차트
seo-title: AEM Forms의 다중 시리즈 차트
description: 적절한 양식 데이터 모델을 만들어 인쇄 및 웹 채널 문서에서 다중 시리즈 차트를 만듭니다.
seo-description: 적절한 양식 데이터 모델을 만들어 인쇄 및 웹 채널 문서에서 다중 시리즈 차트를 만듭니다.
feature: 대화형 통신
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---


# 다중 시리즈 차트

AEM Forms 6.5에서는 여러 시리즈 차트를 만들고 구성하는 기능을 도입했습니다. 여러 시리즈 차트는 일반적으로 Line,Bar,Column 차트 유형과 함께 사용됩니다. 다음 차트는 다중 시리즈 차트의 좋은 예입니다. 이 도표는 일정 기간 동안 3개의 서로 다른 뮤추얼 펀드에서 1만 달러의 성장을 보여 줍니다. AEM Forms에서 이러한 종류의 차트를 만들고 사용하려면 적절한 양식 데이터 모델을 만들어야 합니다.

![멀티시리즈](assets/seriescharts.jfif)

AEM Forms에서 다중 시리즈 차트를 만들려면 필요한 엔티티와 엔티티 간의 연결로 적절한 양식 데이터 모델을 만들어야 합니다. 다음 스크린샷에서는 엔티티와 3개 엔티티 간의 연결을 강조 표시합니다. 최상위 수준에서, Adobe에는 Fund 엔티티와의 일대다 관계를 가지는 &quot;조직&quot;이라는 엔티티가 있습니다. Fund 엔티티는 Performance 엔티티와 일대다 연관성이 있습니다.

![양식 데이터 모델](assets/formdatamodel.jfif)


## 다중 시리즈 차트에 대한 양식 데이터 모델 생성

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 라인 시리즈 차트 구성

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


시스템에서 테스트하려면 다음 단계를 수행하십시오

* [AEM Package Manager를 사용하여 MutualFundFactSheet.zip을 다운로드하여 가져옵니다.](assets/mutualfundfactsheet.zip)
* [하드 드라이브에 SeriesChartSampleData.json을 다운로드합니다.](assets/serieschartsampledata.json) 차트를 채우는 데 사용할 샘플 데이터입니다.
* [Forms 및 문서로 이동합니다.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* &quot;MutualFundGrowthFactSheet&quot; 대화형 통신 템플릿을 부드럽게 선택합니다.
* 미리 보기 클릭 | 샘플 데이터를 업로드합니다.
* 이 문서의 일부로 제공된 샘플 데이터 파일을 찾습니다.
* 이전 단계에서 다운로드한 샘플 데이터와 &quot;MutualFundGrowthFactSheet&quot; 대화형 커뮤니케이션의 인쇄 채널을 미리 봅니다.
