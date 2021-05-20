---
title: AEM Assets에서 닫힌 사용자 그룹
description: 폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 사용자 선택 그룹으로 컨텐츠에 대한 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오에서는 Adobe Experience Manager Assets와 함께 닫힌 사용자 그룹을 사용하여 자산의 특정 폴더에 대한 액세스를 제한하는 방법을 보여줍니다.
version: 6.3, 6.4, 6.5, cloud-service
topic: 관리, 보안
feature: 사용자 및 그룹
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# 닫힌 사용자 그룹{#using-closed-user-groups-with-aem-assets}

폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 사용자 선택 그룹으로 컨텐츠에 대한 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오에서는 Adobe Experience Manager Assets와 함께 닫힌 사용자 그룹을 사용하여 자산의 특정 폴더에 대한 액세스를 제한하는 방법을 보여줍니다. AEM Assets을 사용한 닫힌 사용자 그룹에 대한 지원이 AEM 6.4에서 처음 도입되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assets을 사용한 CUG(폐쇄된 사용자 그룹)

* AEM 게시 인스턴스의 자산에 대한 액세스를 제한하도록 설계되었습니다.
* 사용자/그룹 집합에 대한 읽기 권한을 부여합니다.
* CUG는 폴더 수준에서만 구성할 수 있습니다. 개별 자산에 대해 CUG를 설정할 수 없습니다.
* CUG 정책은 하위 폴더 및 적용된 자산에 의해 자동으로 상속됩니다.
* 새 CUG 정책을 설정하여 하위 폴더에 의해 CUG 정책을 대체할 수 있습니다. 이 기능은 드물게 사용해야 하며 우수 사례로 간주되지 않습니다.

## 폐쇄된 사용자 그룹과 액세스 제어 목록 {#closed-user-groups-vs-access-control-lists}

CUG(폐쇄된 사용자 그룹)와 ACL(액세스 제어 목록)을 모두 사용하여 AEM의 컨텐츠와 AEM 보안 사용자 및 그룹을 기반으로 콘텐츠에 대한 액세스를 제어합니다. 그러나 이러한 기능의 애플리케이션과 구현은 매우 다릅니다. 다음 표에는 두 기능 간의 차이점이 요약되어 있습니다.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 목적 | **현재** AEM 인스턴스에서 컨텐츠에 대한 권한을 구성하고 적용합니다. | AEM **author** 인스턴스의 컨텐츠에 대한 CUG 정책을 구성합니다. AEM **publish** 인스턴스의 컨텐츠에 대한 CUG 정책을 적용합니다. |
| 권한 수준 | 모든 수준에 대해 사용자/그룹에 대해 부여된/거부된 권한을 정의합니다.읽기, 수정, 생성, 삭제, ACL 읽기, ACL 편집, 복제 | 사용자/그룹 집합에 대한 읽기 권한을 부여합니다. *다른 모든* 사용자/그룹에 대한 읽기 권한을 거부합니다. |
| 발행 | ACL이 컨텐츠와 함께 게시되지 않은 *입니다.* | CUG 정책 *은(는) 컨텐츠와 함께 게시됩니다.* |

## 지원 링크 {#supporting-links}

* [자산 및 닫힌 사용자 그룹 관리](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [폐쇄된 사용자 그룹 생성](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak 닫힌 사용자 그룹 설명서](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
