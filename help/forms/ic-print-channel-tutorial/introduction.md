---
title: 인쇄 채널용 첫 번째 대화형 통신 만들기
seo-title: Creating your first interactive communication for the print channel
description: Interactive Communications는 AEM Forms 6.4의 새로운 기능입니다. 이 문서는 인쇄 채널용 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.
seo-description: Interactive Communications is new to AEM Forms 6.4. This document will walk you through the steps needed to create an interactive communication for the print channel.
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1949aeff-ae56-4abd-8e63-23c2fb4859f2
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 인쇄 채널용 첫 번째 대화형 통신 만들기

Interactive Communications는 AEM Forms 6.4의 새로운 기능입니다. 이 문서는 인쇄 채널용 대화형 커뮤니케이션을 만드는 데 필요한 단계를 안내합니다.

## 전제 조건 {#prerequistes}

[패키지 관리자를 사용하여 이 자습서와 관련된 자산을 AEM으로 다운로드하여 가져옵니다.](assets/gettingstartedassets.zip)이 zip 파일에는 자산 패키지의 일부로 이미지, 문서 조각, 감시 폴더 구성 및 레이아웃 파일(xdp)이 포함되어 있습니다

[이 파일을 다운로드하여 압축을 해제합니다.](assets/warfileandswaggerfile.zip) 이 파일에는 데이터 소스를 구성하는 데 사용해야 하는 Tomcat 및 swagger 파일에 배포해야 하는 SampleRest.war 파일이 포함되어 있습니다.

이 자습서를 완료하면 다음 사항을 알 수 있습니다.

* 데이터 소스 만들기
* 양식 데이터 모델 작성
* 문서 조각 만들기
* 테이블 및 차트 구성
* 감시 폴더를 사용하여 일괄 처리 모드에서 문서를 생성합니다.
