---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색
description: 양식 데이터를 저장하고 검색하는 단계를 단계별로 안내하는 다중 부분 자습서입니다
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# MySQL 데이터베이스에서 적응형 양식 데이터 저장 및 검색

이 튜토리얼에서는 데이터베이스에서 적응형 양식 데이터를 저장 및 검색하는 단계를 설명합니다. 이 자습서에서는 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장했습니다. AEM에서 데이터베이스 특정 드라이버를 배포한 경우 선택한 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 높은 수준에서 사용 사례를 실현하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터에 액세스 권한 얻기

* 서블릿에 POST 호출을 생성합니다. 이 서블릿은 데이터베이스에 데이터를 저장합니다. 저장된 데이터가 GUID와 연결되어 있습니다

* 적응형 양식을 저장된 데이터로 채우려면 GUID와 연결된 데이터를 검색하고 **request.setAttribute** 메서드를 사용하여 적응형 양식을 채웁니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
