---
title: 웹 채널용 첫 번째 대화형 통신 만들기
seo-title: 웹 채널용 첫 번째 대화형 통신 만들기
description: Interactive Communications는 AEM Forms 6.4의 새로운 기능입니다. 이 문서는 웹 채널을 위한 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
seo-description: Interactive Communications는 AEM Forms 6.4의 새로운 기능입니다. 이 문서는 웹 채널을 위한 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

---


# 웹 채널용 첫 번째 대화형 통신 만들기

Interactive Communications는 AEM Forms 6.4의 새로운 기능입니다. 이 문서는 인쇄 채널용 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.

이 기능의 라이브 데모에 대한 링크는 [AEM Forms 샘플](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

## 전제 조건 {#prerequistes}

[패키지 관리자를 사용하여 이 자습서와 관련된 자산을 AEM으로 다운로드하여 가져옵니다.](assets/gettingstartedassets.zip). 이 zip 파일에는 이 자습서에서 사용할 이미지 및 문서 조각이 포함되어 있습니다

[이 파일을 다운로드하여 압축을 해제합니다.](assets/warfileandswaggerfile.zip) 이 파일에는 데이터 소스를 구성하는 데 사용해야 하는 Tomcat 및 swagger 파일에 배포해야 하는 SampleRest.war 파일이 포함되어 있습니다.

이 자습서를 완료하면 다음 사항을 알 수 있습니다.

* 데이터 소스 만들기
* 양식 데이터 모델 작성
* 문서 조각 만들기
* 테이블 및 차트 구성
* 웹 채널 문서 제공




