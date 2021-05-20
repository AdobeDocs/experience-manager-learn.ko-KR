---
title: CRXDE Lite
description: 'CRXDE Lite은 AEM as a Cloud Service 개발자 환경을 디버깅하기 위한 고전적이지만 강력한 도구입니다. CRXDE Lite은 디버깅이 모든 리소스 및 속성을 검사하거나 JCR의 가변 부분을 조작하고 권한을 조사하는 데 도움이 되는 기능을 제공합니다. '
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# CRXDE Lite을 사용하여 AEM as a Cloud Service 디버깅

CRXDE Lite은 __AEM에서 Cloud Service 개발 환경(및 로컬 AEM SDK)으로 사용할 수 있는__&#x200B;만 있습니다.

## AEM 작성자의 CRXDE Lite 액세스

CRXDE Lite은 __AEM에서 Cloud Service 개발 환경으로 액세스할 수 있는__&#x200B;이고, 스테이지 또는 프로덕션 환경에서 사용할 수 없는 __입니다.__

AEM 작성자의 CRXDE Lite에 액세스하려면:

1. AEM as a Cloud Service AEM 작성자 서비스로 로그인합니다.
1. 도구 > 일반 > CRXDE Lite으로 이동합니다.

이렇게 하면 AEM 작성자에 로그인하는 데 사용되는 자격 증명과 권한을 사용하여 CRXDE Lite이 열립니다.

## 콘텐츠 디버깅

CRXDE Lite은 JCR에 직접 액세스할 수 있습니다. CRXDE Lite을 통해 표시되는 콘텐츠는 사용자에게 부여된 권한에 의해 제한됩니다. 즉, 액세스 권한에 따라 JCR에서 모든 항목을 보거나 수정할 수 없을 수 있습니다.

`/apps`, `/libs` 및 `/oak:index`는 변경할 수 없습니다. 즉, 사용자가 런타임 시 변경할 수 없습니다. JCR에서 이러한 위치는 코드 배포를 통해서만 수정할 수 있습니다.

+ JCR 구조는 왼쪽 탐색 창을 사용하여 탐색 및 조작됩니다
+ 왼쪽 탐색 창에서 노드를 선택하면 노드 속성의 맨 아래 창이 표시됩니다.
   + 창에서 속성을 추가, 제거 또는 변경할 수 있습니다
+ 왼쪽 탐색에서 파일 노드를 두 번 클릭하면 오른쪽 상단 창에 파일의 컨텐츠가 열립니다
+ 왼쪽 상단에 있는 모두 저장 단추를 탭하여 변경 내용을 유지하거나 저장하지 않은 변경 사항을 되돌리기 위해 모두 저장 옆에 있는 아래쪽 화살표를 누릅니다.

![CRXDE Lite - 컨텐츠 디버깅](./assets/crxde-lite/debugging-content.png)

런타임 시 AEM에서 CRXDE Lite을 통해 Cloud Service 개발 환경으로 변경할 수 있는 컨텐츠를 변경해야 하는 경우 주의해야 합니다.
CRXDE Lite을 통해 AEM에 직접 변경한 사항은 추적하고 제어하기 어려울 수 있습니다. 적절하게, CRXDE Lite을 통해 변경한 내용이 AEM 프로젝트의 변경 가능한 컨텐츠 패키지(`ui.content`)로 돌아가서 문제가 해결되도록 Git에 커밋되도록 하십시오. 이상적으로는 모든 애플리케이션 컨텐츠 변경 사항은 CRXDE Lite을 통해 AEM에 직접 변경하는 것보다 코드 베이스에서 발생하고 배포를 통해 AEM으로 유입되는 것이 좋습니다.

### 액세스 제어 디버깅

CRXDE Lite은 특정 사용자 또는 그룹(주도자)에 대한 특정 노드의 액세스 제어를 테스트하고 평가하는 방법을 제공합니다.

CRXDE Lite의 테스트 액세스 제어 콘솔에 액세스하려면 다음 위치로 이동하십시오.

+ CRXDE Lite > 도구 > 테스트 액세스 제어 ...

![CRXDE Lite - 테스트 액세스 제어](./assets/crxde-lite/permissions__test-access-control.png)

1. 경로 필드를 사용하여 평가할 JCR 경로를 선택합니다
1. 주도자 필드를 사용하여 경로를 평가할 사용자 또는 그룹을 선택합니다
1. Test 단추를 누릅니다

결과는 다음과 같습니다.

+ ____ 평가된 경로를 반복합니다
+ ____ 원칙 은 경로가 평가된 사용자 또는 그룹을 재지정합니다
+ ____ 원칙 은 선택한 주도자가 속하는 모든 주도자를 나열합니다.
   + 상속을 통해 권한을 제공할 수 있는 전환 그룹 멤버십을 이해하는 데 유용합니다
+ __경로__ 의 권한은 선택된 주체가 평가 경로에 가지고 있는 모든 JCR 권한을 나열합니다

### 지원되지 않는 디버깅 활동

다음은 CRXDE Lite에서&#x200B;__을 수행할 수 없는 디버깅 활동입니다.__

### OSGi 구성 디버깅

배포된 OSGi 구성은 CRXDE Lite을 통해 검토할 수 없습니다. OSGi 구성은 `/apps/example/config.xxx`에 AEM Project의 `ui.apps` 코드 패키지에서 유지되지만, Cloud Service 환경으로 AEM에 배포할 때 OSGi 구성 리소스는 JCR에 지속되지 않으므로 CRXDE Lite을 통해 표시되지 않습니다.

대신 [개발자 콘솔 > 구성](./developer-console.md#configurations)을 사용하여 배포된 OSGi 구성을 검토하십시오.
