---
title: AEM SDK를 디버깅하는 기타 도구
description: 다양한 다른 도구를 사용하여 AEM SDK의 로컬 빠른 시작을 디버깅하는 데 도움이 될 수 있습니다.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 1%

---

# AEM SDK를 디버깅하는 기타 도구

다양한 다른 도구를 사용하여 AEM SDK의 로컬 빠른 시작에서 애플리케이션을 디버깅하는 데 도움이 될 수 있습니다.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite은 JCR, AEM 데이터 리포지토리와 상호 작용할 수 있는 웹 기반 인터페이스입니다. CRXDE Lite은 노드, 속성, 속성 값 및 권한을 포함하여 JCR을 완전히 볼 수 있도록 합니다.

CRXDE Lite은 다음 위치에 있습니다.

+ 도구 > 일반 > CRXDE Lite
+ 또는 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### 콘텐츠 디버깅

CRXDE Lite은 JCR에 직접 액세스할 수 있습니다. CRXDE Lite을 통해 표시되는 콘텐츠는 사용자에게 부여된 권한에 의해 제한됩니다. 즉, 액세스 권한에 따라 JCR에서 모든 항목을 보거나 수정할 수 없을 수 있습니다.

+ JCR 구조는 왼쪽 탐색 창을 사용하여 탐색 및 조작됩니다
+ 왼쪽 탐색 창에서 노드를 선택하면 노드 속성의 맨 아래 창이 표시됩니다.
   + 창에서 속성을 추가, 제거 또는 변경할 수 있습니다
+ 왼쪽 탐색에서 파일 노드를 두 번 클릭하면 오른쪽 상단 창에 파일의 컨텐츠가 열립니다
+ 왼쪽 상단에 있는 모두 저장 단추를 탭하여 변경 내용을 유지하거나 저장하지 않은 변경 사항을 되돌리기 위해 모두 저장 옆에 있는 아래쪽 화살표를 누릅니다.

![CRXDE Lite - 컨텐츠 디버깅](./assets/other-tools/crxde-lite__debugging-content.png)

CRXDE Lite을 통해 AEM SDK에 직접 변경한 내용은 추적 및 제어하기 어려울 수 있습니다. 적절하게, CRXDE Lite을 통해 변경한 내용이 AEM 프로젝트의 변경 가능한 컨텐츠 패키지(`ui.content`) 및 Git에 커밋되었습니다. 이상적으로는 모든 애플리케이션 컨텐츠 변경 사항은 CRXDE Lite을 통해 AEM SDK를 직접 변경하는 것이 아니라 배포를 통해 코드 기반에서 AEM SDK로 유입되는 것이 좋습니다.

### 액세스 제어 디버깅

CRXDE Lite은 특정 사용자 또는 그룹(주도자)에 대한 특정 노드의 액세스 제어를 테스트하고 평가하는 방법을 제공합니다.

CRXDE Lite의 테스트 액세스 제어 콘솔에 액세스하려면 다음 위치로 이동하십시오.

+ CRXDE Lite > 도구 > 테스트 액세스 제어 ...

![CRXDE Lite - 테스트 액세스 제어](./assets/other-tools/crxde-lite__test-access-control.png)

1. 경로 필드를 사용하여 평가할 JCR 경로를 선택합니다
1. 주도자 필드를 사용하여 경로를 평가할 사용자 또는 그룹을 선택합니다
1. Test 단추를 누릅니다

결과는 다음과 같습니다.

+ __경로__ 평가된 경로를 반복합니다.
+ __주체__ 경로가 평가된 사용자 또는 그룹을 재지정합니다.
+ __주도자__ 선택한 주도자가 속한 모든 주도자를 나열합니다.
   + 상속을 통해 권한을 제공할 수 있는 전환 그룹 멤버십을 이해하는 데 유용합니다
+ __경로의 권한__ 선택된 주체가 평가 경로에 가지고 있는 모든 JCR 권한을 나열합니다

## 쿼리 설명

![쿼리 설명](./assets/other-tools/explain-query.png)

AEM SDK의 로컬 빠른 시작에서 쿼리 웹 기반 도구를 설명하고, AEM이 쿼리를 해석 및 실행하는 방법에 대한 주요 인사이트와 AEM에서 쿼리를 실행 가능하게 하는 중요한 도구를 제공합니다.

설명 쿼리는 다음 위치에 있습니다.

+ 도구 > 진단 > 쿼리 성능 > 쿼리 탭 설명
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > 쿼리 설명 탭

## QueryBuilder 디버거

![QueryBuilder 디버거](./assets/other-tools/query-debugger.png)

QueryBuilder 디버거는 AEM을 사용하여 검색 쿼리를 디버깅하고 이해하는 데 도움이 되는 웹 기반 도구입니다 [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 구문

QueryBuilder 디버거는 다음 위치에 있습니다.

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
