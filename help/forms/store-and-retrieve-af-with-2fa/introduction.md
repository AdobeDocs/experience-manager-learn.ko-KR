---
title: MySQL 데이터베이스의 첨부 파일을 사용하여 양식 데이터 저장 및 검색
description: 여러 부분으로 구성된 튜토리얼을 통해 첨부 파일이 있는 양식 데이터를 저장하고 검색하는 절차를 단계별로 살펴봅니다
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---


# 2FA를 사용하여 응용 양식 데이터 저장 및 검색

이 자습서에서는 2FA를 사용하여 첨부 파일이 있는 적응형 양식 데이터를 저장하고 검색하는 것과 관련된 단계를 안내합니다. 이 자습서는 MySQL 데이터베이스를 사용하여 응용 양식 데이터를 저장했습니다. AEM에서 데이터베이스 특정 드라이버를 배포한 경우 원하는 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 사용 사례를 성취하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터에 액세스

* 서블릿으로 POST 호출을 만듭니다. 이 서블릿은 데이터베이스의 데이터와 CRX 저장소의 양식 첨부 파일을 저장합니다. 데이터베이스에 저장된 데이터가 GUID와 연결되어 있습니다.

* 저장된 데이터로 적응형 양식을 채우려면 GUID와 연결된 데이터를 검색하고 **request.setAttribute** 메서드를 사용하여 적응형 양식을 채웁니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 전제 조건

이 콘텐츠의 대상은 다음 영역에서 몇 가지 경험이 있을 것으로 예상됩니다.

* 적응형 양식
* 양식 데이터 모델
* OSGi 서비스/구성 요소
* AEM 클라이언트 라이브러리
