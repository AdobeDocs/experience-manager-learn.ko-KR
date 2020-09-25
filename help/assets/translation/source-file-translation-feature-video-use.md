---
title: AEM Assets에서 소스 파일 변환 사용
seo-title: AEM Assets에서 소스 파일 변환 사용
description: Adobe Experience Manager(AEM) 자산을 사용하면 공통 속성을 공유하는 자산을 식별하고 새로운 관련 자산 기능을 사용하여 관련 자산으로 표시할 수 있습니다. 또한 사용자가 자산의 출처를 쉽게 식별할 수 있도록 자산 간의 소스/파생 관계를 정의할 수도 있습니다. 파생 자산에서 번역 작업 과정을 실행하면 소스 파일이 참조하고 번역을 위해 포함하는 모든 자산을 가져올 수 있으므로 다중 사이트를 유지 관리하는 노력을 줄일 수 있습니다.
seo-description: Adobe Experience Manager(AEM) 자산을 사용하면 공통 속성을 공유하는 자산을 식별하고 새로운 관련 자산 기능을 사용하여 관련 자산으로 표시할 수 있습니다. 또한 사용자가 자산의 출처를 쉽게 식별할 수 있도록 자산 간의 소스/파생 관계를 정의할 수도 있습니다. 파생 자산에서 번역 작업 과정을 실행하면 소스 파일이 참조하고 번역을 위해 포함하는 모든 자산을 가져올 수 있으므로 다중 사이트를 유지 관리하는 노력을 줄일 수 있습니다.
uuid: 58f70535-909b-464a-b265-ddddb8ab2908
discoiquuid: a50eb7f2-744b-46ea-8daf-212d833a0333
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---


# AEM Assets에서 소스 파일 변환 사용 {#using-source-file-translation-with-aem-assets}

Adobe Experience Manager(AEM) 자산을 사용하면 공통 속성을 공유하는 자산을 식별하고 새로운 관련 자산 기능을 사용하여 관련 자산으로 표시할 수 있습니다. 또한 사용자가 자산의 출처를 쉽게 식별할 수 있도록 자산 간의 소스/파생 관계를 정의할 수도 있습니다. 파생 자산에서 번역 작업 과정을 실행하면 소스 파일이 참조하고 번역을 위해 포함하는 모든 자산을 가져올 수 있으므로 다중 사이트를 유지 관리하는 노력을 줄일 수 있습니다.

## 멀티사이트 에셋 소스 파일 관리 {#multisite-asset-source-file-management}

>[!VIDEO](https://video.tv.adobe.com/v/18331/?quality=9&learn=on)

관련 자산은 사용자가 공유 트레이트, 속성 및 워크플로우를 통해 보다 효과적인 크로스 링크 에셋을 관리하고,

* 유사한 특성이나 동일한 캠페인 또는 프로젝트에 속하는 자산을 수동으로 연결하는 새로운 관련 자산 기능
* 사용자는 속성 보기에서 자산의 관련 파일을 볼 수 있습니다. 사용자는 속성 보기 창에서 관련 파일을 탐색할 수 있습니다.
* 두 개의 관련 자산의 속성이 변경된 경우, 사용자는 관계 없음 옵션을 사용하여 이러한 자산과 관련되지 않을 수 있습니다.
* 관련 자산을 삭제하려고 하면 관련 자산이 있는 경우 경고 메시지가 표시됩니다.
* 파생된 관련 자산에서 번역 워크플로우를 실행하면 관련 소스 파일이 번역 워크플로우에 추가되므로 다중 사이트 관리가 용이합니다.