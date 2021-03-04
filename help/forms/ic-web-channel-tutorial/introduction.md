---
title: 웹 채널을 위한 첫 번째 대화형 통신 만들기
seo-title: 웹 채널을 위한 첫 번째 대화형 통신 만들기
description: 인터랙티브 커뮤니케이션은 AEM Forms 6.4의 새로운 기능입니다. 이 문서에서는 웹 채널에 대한 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
seo-description: 인터랙티브 커뮤니케이션은 AEM Forms 6.4의 새로운 기능입니다. 이 문서에서는 웹 채널에 대한 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 개발
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 4%

---


# 웹 채널을 위한 첫 번째 대화형 통신 만들기

인터랙티브 커뮤니케이션은 AEM Forms 6.4의 새로운 기능입니다. 이 문서에서는 인쇄 채널을 위한 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.

이 기능의 라이브 데모를 연결하는 링크는 [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

## 전제 조건 {#prerequistes}

[패키지 관리자를 사용하여 이 튜토리얼과 관련된 에셋을 다운로드하고 AEM으로 가져옵니다.](assets/gettingstartedassets.zip). 이 zip 파일에는 이 튜토리얼에서 사용할 이미지 및 문서 조각이 포함되어 있습니다.

[이 파일을 다운로드하고 압축 해제합니다.](assets/warfileandswaggerfile.zip) 이 파일에는 데이터 소스를 구성하는 데 사용해야 하는 Tomcat 및 Swagger 파일에 배포해야 하는 SampleRest.war 파일이 포함되어 있습니다.

이 튜토리얼을 완료하면 다음 사항을 알 수 있습니다.

* 데이터 소스 만들기
* 양식 데이터 모델 작성
* 문서 조각 만들기
* 테이블 및 차트 구성
* 웹 채널 문서 전달




