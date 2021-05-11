---
title: AEM SDK 디버깅을 위한 기타 도구
description: AEM SDK의 로컬 빠른 시작을 디버깅하는 데 도움이 되는 다양한 도구를 사용할 수 있습니다.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: 개발
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: 5fcc7eec120debf1a8ac08716154599467e66759
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 4%

---


# AEM SDK 디버깅을 위한 기타 도구

AEM SDK의 로컬 quickstart에서 애플리케이션을 디버깅하는 데 도움이 되는 다양한 도구를 사용할 수 있습니다.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite은 JCR, AEM 데이터 저장소와의 상호 작용을 위한 웹 기반 인터페이스입니다. CRXDE Lite은 노드, 속성, 속성 값 및 권한을 포함하여 JCR을 완벽하게 표시합니다.

CRXDE Lite은 다음 위치에 있습니다.

+ 도구 > 일반 > CRXDE Lite
+ 또는 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## 쿼리 설명

![쿼리 설명](./assets/other-tools/explain-query.png)

AEM SDK의 로컬 quickstart에서 쿼리를 해석하고 실행하는 방법에 대한 주요 통찰력과 AEM에서 쿼리를 효율적으로 실행할 수 있는 매우 귀중한 도구를 제공하는 쿼리 웹 기반 도구를 설명합니다.

쿼리 설명은 다음 위치에 있습니다.

+ 도구 > 진단 > 쿼리 성능 > 쿼리 탭 설명
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > 쿼리 설명 탭

## QueryBuilder 디버거

![QueryBuilder 디버거](./assets/other-tools/query-debugger.png)

QueryBuilder 디버거는 AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 구문을 사용하여 검색 쿼리를 디버깅하고 이해하는 데 도움이 되는 웹 기반 도구입니다.

QueryBuilder Debugger는 다음 위치에 있습니다.

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

