---
title: CRXDE Lite
description: CRXDE LiteAEM 는 as a Cloud Service 개발자 환경을 디버깅하기 위한 고전적이면서도 강력한 도구입니다. CRXDE Lite은 디버깅이 모든 리소스 및 속성을 검사하고, JCR의 변경 가능한 부분을 조작하고 권한을 조사하는 것을 돕는 기능 제품군을 제공합니다.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# CRXDE Lite을 사용하여 AEM as a Cloud Service 디버깅

CRXDE Lite: __전용__ AEM as a Cloud Service 개발 환경(및 로컬 AEM SDK)에서 사용할 수 있습니다.

## AEM 작성자의 CRXDE Lite에 액세스

CRXDE Lite: __전용__ AEM as a Cloud Service 개발 환경에서 액세스할 수 있으며 __아님__ 스테이지 또는 프로덕션 환경에서 사용할 수 있습니다.

AEM 작성자의 CRXDE Lite에 액세스하려면:

1. AEM as a Cloud Service AEM Author 서비스에 로그인합니다.
1. 도구 > 일반 > CRXDE Lite 로 이동합니다.

이렇게 하면 AEM 작성자에 로그인하는 데 사용되는 자격 증명 및 권한을 사용하여 CRXDE Lite이 열립니다.

## 콘텐츠 디버깅

CRXDE Lite은 JCR에 직접 액세스할 수 있습니다. CRXDE Lite을 통해 표시되는 컨텐츠는 사용자에게 부여된 권한에 의해 제한됩니다. 즉, 액세스 권한에 따라 JCR의 모든 항목을 보거나 수정할 수 없습니다.

참고: `/apps`, `/libs` 및 `/oak:index` 은 변경할 수 없습니다. 즉, 런타임 시 사용자가 변경할 수 없습니다. JCR에서 이러한 위치는 코드 배포를 통해서만 수정할 수 있습니다.

+ JCR 구조는 왼쪽 탐색 창을 사용하여 탐색 및 조작됩니다
+ 왼쪽 탐색 창에서 노드를 선택하면 하단 창에 노드 속성의 가 표시됩니다.
   + 창에서 속성을 추가, 제거 또는 변경할 수 있습니다.
+ 왼쪽 탐색에서 파일 노드를 두 번 클릭하면 오른쪽 상단 창에 파일 컨텐트가 열립니다
+ 변경 사항을 유지하려면 왼쪽 상단의 모두 저장 버튼을 탭하고 저장하지 않은 변경 사항을 되돌리려면 모두 저장 옆에 있는 아래쪽 화살표를 탭합니다.

![CRXDE Lite - 콘텐츠 디버깅](./assets/crxde-lite/debugging-content.png)

CRXDE Lite을 통해 AEM as a Cloud Service 개발 환경에서 런타임에 변경 가능한 콘텐츠를 변경하는 것은 신중해야 합니다.
CRXDE Lite을 통해 AEM에 직접 수행된 모든 변경 사항은 추적하고 제어하는 것이 어려울 수 있습니다. 적절하게, CRXDE Lite을 통해 변경한 사항이 AEM 프로젝트의 변경 가능한 콘텐츠 패키지(`ui.content`) 및 커밋하여 문제가 해결되도록 Git에 전송합니다. 모든 애플리케이션 콘텐츠 변경 사항은 CRXDE Lite을 통해 AEM에 직접 변경하는 것이 아니라 코드 기반에서 시작하여 배포를 통해 AEM으로 유입되는 것이 이상적입니다.

### 액세스 제어 디버깅

CRXDE Lite은 특정 사용자 또는 그룹(즉, 주체)에 대한 특정 노드의 액세스 제어를 테스트하고 평가하는 방법을 제공합니다.

CRXDE Lite의 Test Access Control 콘솔에 액세스하려면 다음으로 이동합니다.

+ CRXDE Lite > 도구 > 액세스 제어 테스트 ...

![CRXDE Lite - 액세스 제어 테스트](./assets/crxde-lite/permissions__test-access-control.png)

1. 경로 필드를 사용하여 평가할 JCR 경로 선택
1. 주도자 필드를 사용하여 경로를 평가할 사용자 또는 그룹을 선택합니다
1. 테스트 단추 탭

결과는 아래에 표시됩니다.

+ __경로__ 평가된 경로를 반복합니다
+ __사용자__ 경로가 평가된 사용자 또는 그룹을 반복합니다
+ __사용자__ 선택된 주도자가 속한 주도자를 모두 나열합니다.
   + 상속을 통해 권한을 제공할 수 있는 전이적 그룹 멤버십을 이해하는 데 유용합니다
+ __경로의 권한__ 평가된 경로에 대해 선택한 주체가 가진 모든 JCR 권한을 나열합니다.

### 지원되지 않는 디버깅 활동

다음은 다음을 수행할 수 있는 디버깅 활동입니다. __아님__ CRXDE Lite에서 수행됩니다.

### OSGi 구성 디버깅

배포된 OSGi 구성은 CRXDE Lite을 통해 검토할 수 없습니다. OSGi 구성은 AEM 프로젝트의 `ui.apps` 코드 패키지 위치: `/apps/example/config.xxx`그러나 AEM as a Cloud Service 환경에 배포하면 OSGi 구성 리소스가 JCR에 지속되지 않으므로 CRXDE Lite을 통해 표시되지 않습니다.

대신 [Developer Console > 구성](./developer-console.md#configurations) 배포된 OSGi 구성을 검토합니다.
