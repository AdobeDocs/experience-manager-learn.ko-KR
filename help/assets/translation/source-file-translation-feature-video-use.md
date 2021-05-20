---
title: AEM Assets에서 소스 파일 변환 사용
description: Adobe Experience Manager (AEM) Assets를 사용하면 공통 속성을 공유하는 자산을 식별하고 새로운 관련 자산 기능을 사용하여 관련 자산으로 표시할 수 있습니다. 또한 사용자가 자산 간의 소스/파생 관계를 정의할 수 있으므로 사용자가 자산의 출처를 쉽게 식별할 수 있습니다. 파생 자산에서 번역 워크플로우를 실행하면 소스 파일이 참조하는 모든 자산을 가져와서 번역을 위해 포함하므로 다중 사이트 유지 노력이 줄어듭니다.
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---


# AEM Assets에서 소스 파일 변환 사용 {#using-source-file-translation-with-aem-assets}

Adobe Experience Manager (AEM) Assets를 사용하면 공통 속성을 공유하는 자산을 식별하고 새로운 관련 자산 기능을 사용하여 관련 자산으로 표시할 수 있습니다. 또한 사용자가 자산 간의 소스/파생 관계를 정의할 수 있으므로 사용자가 자산의 출처를 쉽게 식별할 수 있습니다. 파생 자산에서 번역 워크플로우를 실행하면 소스 파일이 참조하는 모든 자산을 가져와서 번역을 위해 포함하므로 다중 사이트 유지 노력이 줄어듭니다.

## 다중 사이트 자산 소스 파일 관리 {#multisite-asset-source-file-management}

>[!VIDEO](https://video.tv.adobe.com/v/18331/?quality=9&learn=on)

관련 자산은 사용자가 공유 트레이트, 속성을 사용하여 보다 나은 교차 링크 자산을 관리하고 워크플로우를 간소화하는 데 도움이 됩니다.

* 유사한 특성을 사용하거나 동일한 캠페인 또는 프로젝트에 속하는 자산을 수동으로 연결하는 새로운 관련 자산 기능
* 사용자는 보기 속성에서 자산에 대한 관련 파일을 볼 수 있습니다. 사용자는 속성 보기 창에서 관련 파일로 이동할 수 있습니다.
* 두 관련 자산의 속성이 변경된 경우 사용자는 관계 해제 옵션을 사용하여 이러한 자산의 관계를 해제할 수 있습니다.
* 관련 자산을 삭제하려고 하면 다른 관련 자산이 있는 경우 경고 메시지가 표시됩니다.
* 파생된 관련 자산에서 번역 워크플로우를 실행하고 관련 소스 파일을 번역 워크플로우에 추가하여 다중 사이트 관리를 쉽게 할 수 있습니다.