---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색
description: 양식 데이터 저장 및 검색과 관련된 단계를 안내하는 다중 파트 튜토리얼
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 239
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# MySQL 데이터베이스에서 적응형 양식 데이터 저장 및 검색

이 튜토리얼에서는 데이터베이스에서 적응형 양식 데이터를 저장하고 검색하는 단계를 설명합니다. 이 자습서에서는 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장했습니다. AEM에서 데이터베이스별 드라이버를 배포한 경우 선택한 데이터베이스를 사용하여 데이터를 저장할 수 있습니다. 높은 수준에서 사용 사례를 달성하려면 다음 단계가 필요합니다.

* GuideBridge API를 사용하여 적응형 양식 데이터 액세스

* 서블릿에 대한 POST 호출을 수행합니다. 이 서블릿은 데이터베이스에 데이터를 저장합니다. 저장된 데이터는 GUID와 연결됩니다.

* 적응형 양식을 저장된 데이터로 채우려면 GUID와 연관된 데이터를 검색하고 다음을 사용하여 적응형 양식을 채웁니다. **request.setAttribute** 메서드를 사용합니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


