---
title: MySQL 데이터베이스의 첨부 파일이 있는 양식 데이터 저장 및 검색
description: 첨부 파일이 있는 양식 데이터를 저장 및 검색하는 단계를 단계별로 안내하는 다중 부분 자습서입니다
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---

# 2FA를 사용하여 적응형 양식 데이터 저장 및 검색

이 튜토리얼에서는 2FA를 사용하여 첨부 파일이 있는 적응형 양식 데이터를 저장 및 검색하는 단계를 설명합니다. 이 자습서에서는 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장했습니다. AEM에서 데이터베이스 특정 드라이버를 배포한 경우 선택한 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 높은 수준에서 사용 사례를 실현하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터에 액세스 권한 얻기

* 서블릿에 POST 호출을 생성합니다. 이 서블릿은 데이터베이스의 데이터와 CRX 저장소의 양식 첨부 파일을 저장합니다. 데이터베이스에 저장된 데이터가 GUID와 연결되어 있습니다.

* 저장된 데이터로 적응형 양식을 채우려면 GUID와 연결된 데이터를 검색하고 **request.setAttribute** 메서드를 사용합니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 사전 요구 사항

이 컨텐츠의 대상자에게는 다음 영역에서 경험이 필요합니다.

* 적응형 양식
* 양식 데이터 모델
* OSGi 서비스/구성 요소
* AEM 클라이언트 라이브러리
