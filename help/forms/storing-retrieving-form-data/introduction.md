---
title: MySQL 데이터베이스 소개에서 양식 데이터 저장 및 검색
description: 양식 데이터를 저장하고 검색하는 단계를 단계별로 안내하는 다중 부분 자습서입니다
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# MySQL 데이터베이스에서 적응형 양식 데이터 저장 및 검색

이 튜토리얼에서는 데이터베이스에서 적응형 양식 데이터를 저장 및 검색하는 단계를 설명합니다. 이 자습서에서는 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장했습니다. AEM에서 데이터베이스 특정 드라이버를 배포한 경우 선택한 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 높은 수준에서 사용 사례를 실현하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터에 액세스 권한 얻기

* 서블릿에 POST 호출을 생성합니다. 이 서블릿은 데이터베이스에 데이터를 저장합니다. 저장된 데이터가 GUID와 연결되어 있습니다

* 저장된 데이터로 적응형 양식을 채우려면 GUID와 연결된 데이터를 검색하고 **request.setAttribute** 메서드를 사용합니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


