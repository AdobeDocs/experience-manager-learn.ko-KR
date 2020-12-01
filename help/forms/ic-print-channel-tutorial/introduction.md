---
title: 인쇄 채널을 위한 첫 번째 인터랙티브한 커뮤니케이션 제작
seo-title: 인쇄 채널을 위한 첫 번째 인터랙티브한 커뮤니케이션 제작
description: Interactive Communications는 AEM Forms 6.4에서 처음 도입되었습니다. 이 문서에서는 인쇄 채널용 대화형 통신을 만드는 데 필요한 단계를 안내합니다.
seo-description: Interactive Communications는 AEM Forms 6.4에서 처음 도입되었습니다. 이 문서에서는 인쇄 채널용 대화형 통신을 만드는 데 필요한 단계를 안내합니다.
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 2%

---


# 인쇄 채널을 위한 첫 번째 인터랙티브한 커뮤니케이션 제작

Interactive Communications는 AEM Forms 6.4에서 처음 도입되었습니다. 이 문서에서는 인쇄 채널용 대화형 통신을 만드는 데 필요한 단계를 안내합니다.

이 기능의 실시간 데모를 보려면 [AEM Forms 샘플](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

## 전제 조건 {#prerequistes}

[패키지 관리자를 사용하여 이 튜토리얼과 관련된 에셋을 다운로드하고 AEM으로 가져옵니다.](assets/gettingstartedassets.zip)이 zip 파일에는 자산 패키지의 일부로 이미지, 문서 조각, 감시 폴더 구성 및 레이아웃 파일(xdp)이 들어 있습니다.

[이 파일을 다운로드하고 압축을 해제합니다.](assets/warfileandswaggerfile.zip) 이 파일에는 Tomcat 및 Swagger 파일에 배포해야 하는 SampleRest.war 파일이 포함되어 있으며, 이 파일은 데이터 소스를 구성하는 데 사용해야 합니다.

이 튜토리얼을 완료하면 다음과 같은 사항을 알 수 있습니다.

* 데이터 소스 만들기
* 양식 데이터 모델 작성
* 문서 조각 만들기
* 테이블 및 차트 구성
* 감시 폴더를 사용하여 일괄 처리 모드로 문서 생성

