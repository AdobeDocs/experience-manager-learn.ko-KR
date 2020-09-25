---
title: 웹 채널을 위한 첫 번째 인터랙티브 커뮤니케이션 제작
seo-title: 웹 채널을 위한 첫 번째 인터랙티브 커뮤니케이션 제작
description: 인터랙티브 커뮤니케이션은 AEM Forms 6.4의 새로운 기능입니다. 이 문서에서는 웹 채널에 대한 인터랙티브 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
seo-description: 인터랙티브 커뮤니케이션은 AEM Forms 6.4의 새로운 기능입니다. 이 문서에서는 웹 채널에 대한 인터랙티브 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 2%

---


# 웹 채널을 위한 첫 번째 인터랙티브 커뮤니케이션 제작

Interactive Communications는 AEM Forms 6.4에서 처음 도입되었습니다. 이 문서에서는 인쇄 채널용 대화형 통신을 만드는 데 필요한 단계를 안내합니다.

이 기능의 실시간 데모를 보려면 [AEM Forms 샘플](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 참조하십시오.

## 전제 조건 {#prerequistes}

[패키지 관리자를 사용하여 이 튜토리얼과 관련된 에셋을 다운로드하고 AEM으로 가져옵니다.](assets/gettingstartedassets.zip). 이 zip 파일에는 이 자습서에서 사용될 이미지 및 문서 조각이 포함되어 있습니다.

[이 파일을 다운로드하고 압축을 해제합니다.](assets/warfileandswaggerfile.zip) 이 파일에는 Tomcat 및 Swagger 파일에 배포해야 하는 SampleRest.war 파일이 포함되어 있으며, 이 파일은 데이터 소스를 구성하는 데 사용해야 합니다.

이 튜토리얼을 완료하면 다음과 같은 사항을 알 수 있습니다.

* 데이터 소스 만들기
* 양식 데이터 모델 작성
* 문서 조각 만들기
* 테이블 및 차트 구성
* 웹 채널 문서 전달




