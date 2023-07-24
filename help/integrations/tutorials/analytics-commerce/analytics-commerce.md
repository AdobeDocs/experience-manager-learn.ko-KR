---
title: Analytics와 상거래 통합 자습서
description: Analytics를 Commerce와 통합하는 방법에 대해 알아봅니다.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="통합" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# Analytics와 Commerce 통합

* 계정이 Adobe Analytics에 액세스할 수 있는지 확인합니다.

* Adobe Analytics에서 프로젝트를 만듭니다.

* 스키마를 만듭니다.
   * 이 옵션은 이후 단계의 옵션에서 선택해야 합니다. 스키마를 생성하려면 &quot;데이터 관리&quot; 아래의 왼쪽 열을 찾은 다음 스키마를 찾습니다. 이제 왼쪽 상단에서 &quot;스키마 만들기&quot;를 클릭합니다. XDM ExperienceEvent 를 선택합니다.
   * 왼쪽에 있는 필드 찾기 그룹에서 추가를 클릭합니다
      * 검색에서 다음을 입력하여 필터링할 수 있습니다. `ExperienceEvent Commerce`
      * 다음을 찾습니다. `Adobe Analytics ExperienceEvent Commerce` 상자를 선택합니다.
      * 다음을 클릭하십시오. `Add field groups` 저장하고 계속하기 위해 오른쪽 상단
* 데이터 세트를 만듭니다. 다음에 &quot;데이터 스트림&quot;을 설정할 때 필요합니다.
   * 데이터 세트는 왼쪽 열 &quot;데이터 관리&quot; 아래에 있으며 &quot;데이터 세트&quot;를 찾습니다.
   * 그런 다음 오른쪽 상단에 있는 &quot;데이터 세트 만들기&quot;를 클릭합니다. 스키마에서 데이터 세트를 만듭니다.
   * 이전에 생성한 스키마를 검색하고 사용합니다
* 데이터 스트림을 만듭니다. &quot;왼쪽 열의 데이터 수집&quot;을 사용하고 &quot;데이터스트림&quot;을 찾으면 이 기능에 액세스할 수 있습니다.
* 패널 및 세그먼트를 사용하여 표를 만듭니다. 이 자습서는 매우 복잡하므로 숙련된 Analytics 사용자의 도움이 필요합니다.


마지막으로 보고서를 보려면 experience.adobe.com으로 이동하여 작업 공간 프로젝트를 찾은 다음 보려는 프로젝트의 링크를 클릭하면 이 이미지와 같은 항목이 표시됩니다

![일부 상거래 데이터의 Analytics 스크린샷](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)