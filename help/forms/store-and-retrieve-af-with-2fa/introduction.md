---
title: MySQL 데이터베이스에서 첨부 파일이 있는 양식 데이터 저장 및 검색
description: 여러 부분으로 구성된 튜토리얼을 통해 첨부 파일이 있는 양식 데이터 저장 및 검색 단계를 안내합니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 159
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 2FA를 사용하여 적응형 양식 데이터 저장 및 검색

이 튜토리얼에서는 2FA를 사용하여 첨부 파일이 있는 적응형 양식 데이터를 저장하고 검색하는 단계를 설명합니다. 이 자습서에서는 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장했습니다. AEM에서 데이터베이스별 드라이버를 배포한 경우 선택한 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 높은 수준에서 사용 사례를 달성하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터 액세스

* 서블릿에 대한 POST 호출을 수행합니다. 이 서블릿은 데이터베이스에 데이터를 저장하고 양식 첨부 파일은 CRX 저장소에 저장합니다. 데이터베이스에 저장된 데이터는 GUID와 연결됩니다.

* 적응형 양식을 저장된 데이터로 채우려면 GUID와 연관된 데이터를 검색하고 다음을 사용하여 적응형 양식을 채웁니다. **request.setAttribute** 메서드를 사용합니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 사전 요구 사항

이 콘텐츠의 대상자는 다음 영역에서 몇 가지 경험을 가질 것으로 예상됩니다.

* 적응형 양식
* 양식 데이터 모델
* OSGi 서비스/구성 요소
* AEM 클라이언트 라이브러리


## 다음 단계

[데이터 소스 구성](./configure-data-source.md)