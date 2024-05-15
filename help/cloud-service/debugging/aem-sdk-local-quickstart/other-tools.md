---
title: AEM SDK 디버깅을 위한 기타 도구
description: 다양한 다른 도구는 AEM SDK의 로컬 빠른 시작을 디버깅하는 데 도움이 될 수 있습니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 107
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 1%

---

# AEM SDK 디버깅을 위한 기타 도구

다양한 다른 도구는 AEM SDK의 로컬 빠른 시작에서 애플리케이션을 디버깅하는 데 도움이 될 수 있습니다.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite은 AEM 데이터 저장소인 JCR과 상호 작용하기 위한 웹 기반 인터페이스입니다. CRXDE Lite은 노드, 속성, 속성 값 및 권한을 포함하여 JCR을 완전히 볼 수 있도록 합니다.

CRXDE Lite 위치:

+ 도구 > 일반 > CRXDE Lite
+ 또는에서 바로 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### 콘텐츠 디버깅

CRXDE Lite은 JCR에 직접 액세스할 수 있습니다. CRXDE Lite을 통해 표시되는 컨텐츠는 사용자에게 부여된 권한에 의해 제한됩니다. 즉, 액세스 권한에 따라 JCR의 모든 항목을 보거나 수정할 수 없습니다.

+ JCR 구조는 왼쪽 탐색 창을 사용하여 탐색 및 조작됩니다
+ 왼쪽 탐색 창에서 노드를 선택하면 하단 창에 노드 속성의 가 표시됩니다.
   + 창에서 속성을 추가, 제거 또는 변경할 수 있습니다.
+ 왼쪽 탐색에서 파일 노드를 두 번 클릭하면 오른쪽 상단 창에 파일 컨텐트가 열립니다
+ 변경 사항을 유지하려면 왼쪽 상단의 모두 저장 버튼을 탭하고 저장하지 않은 변경 사항을 되돌리려면 모두 저장 옆에 있는 아래쪽 화살표를 탭합니다.

![CRXDE Lite - 콘텐츠 디버깅](./assets/other-tools/crxde-lite__debugging-content.png)

CRXDE Lite을 통해 AEM SDK에 직접 적용한 변경 사항은 추적하고 제어하는 것이 어려울 수 있습니다. 적절하게, CRXDE Lite을 통해 변경한 사항이 AEM 프로젝트의 변경 가능한 콘텐츠 패키지(`ui.content`) Git에 커밋됩니다. 모든 애플리케이션 콘텐츠 변경 사항은 CRXDE Lite을 통해 AEM SDK에 직접 변경하는 것이 아니라 코드 기반에서 시작하여 배포를 통해 AEM SDK로 유입되는 것이 가장 좋습니다.

### 액세스 제어 디버깅

CRXDE Lite은 특정 사용자 또는 그룹(즉, 주체)에 대한 특정 노드의 액세스 제어를 테스트하고 평가하는 방법을 제공합니다.

CRXDE Lite의 Test Access Control 콘솔에 액세스하려면 다음으로 이동합니다.

+ CRXDE Lite > 도구 > 액세스 제어 테스트 ...

![CRXDE Lite - 액세스 제어 테스트](./assets/other-tools/crxde-lite__test-access-control.png)

1. 경로 필드를 사용하여 평가할 JCR 경로 선택
1. 주도자 필드를 사용하여 경로를 평가할 사용자 또는 그룹을 선택합니다
1. 테스트 단추 탭

결과는 아래에 표시됩니다.

+ __경로__ 평가된 경로를 반복합니다
+ __사용자__ 경로가 평가된 사용자 또는 그룹을 반복합니다
+ __사용자__ 선택된 주도자가 속한 주도자를 모두 나열합니다.
   + 상속을 통해 권한을 제공할 수 있는 전이적 그룹 멤버십을 이해하는 데 유용합니다
+ __경로의 권한__ 평가된 경로에 대해 선택한 주체가 가진 모든 JCR 권한을 나열합니다.

## 쿼리 설명

![쿼리 설명](./assets/other-tools/explain-query.png)

AEM SDK의 로컬 빠른 시작에서 AEM이 쿼리를 해석하고 실행하는 방법에 대한 주요 통찰력을 제공하는 쿼리 웹 기반 도구 및 AEM에서 수행적 방식으로 쿼리를 실행하는 데 유용한 도구를 설명합니다.

쿼리 설명 위치는 다음과 같습니다.

+ 도구 > 진단 > 쿼리 성능 > 쿼리 설명 탭
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > 쿼리 설명 탭

## QueryBuilder 디버거

![QueryBuilder 디버거](./assets/other-tools/query-debugger.png)

QueryBuilder 디버거는 AEM을 사용하여 검색 쿼리를 디버깅하고 이해하는 데 도움이 되는 웹 기반 도구입니다 [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 구문.

QueryBuilder 디버거는 다음 위치에 있습니다.

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
