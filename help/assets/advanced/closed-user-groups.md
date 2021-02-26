---
title: AEM Assets에서 닫힌 사용자 그룹
description: 폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 사용자 선택 그룹에 대한 컨텐츠 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오는 닫힌 사용자 그룹을 Adobe Experience Manager 자산과 함께 사용하여 특정 자산 폴더에 대한 액세스를 제한하는 방법을 보여줍니다.
version: 6.3, 6.4, 6.5, cloud-service
topic: 관리, 보안
feature: 사용자 및 그룹
role: 관리자
level: 중간
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# 닫힌 사용자 그룹{#using-closed-user-groups-with-aem-assets}

폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 사용자 선택 그룹에 대한 컨텐츠 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오는 닫힌 사용자 그룹을 Adobe Experience Manager 자산과 함께 사용하여 특정 자산 폴더에 대한 액세스를 제한하는 방법을 보여줍니다. AEM Assets에서 닫힌 사용자 그룹에 대한 지원은 AEM 6.4에서 처음 도입되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assets이 있는 폐쇄된 사용자 그룹(CUG)

* AEM 게시 인스턴스의 자산에 대한 액세스를 제한하도록 설계되었습니다.
* 사용자/그룹 집합에 대한 읽기 액세스 권한을 부여합니다.
* CUG는 폴더 수준에서만 구성할 수 있습니다. 개별 자산에 대해 CUG를 설정할 수 없습니다.
* CUG 정책은 하위 폴더 및 적용된 자산에 의해 자동으로 상속됩니다.
* 새 CUG 정책을 설정하여 하위 폴더로 CUG 정책을 재정의할 수 있습니다. 이 작업은 제한적으로 사용해야 하며 우수 사례로 간주되지 않습니다.

## 닫힌 사용자 그룹과 액세스 제어 목록 {#closed-user-groups-vs-access-control-lists} 비교

CUG(폐쇄된 사용자 그룹) 및 ACL(액세스 제어 목록)은 모두 AEM의 콘텐츠에 대한 액세스를 제어하는 데 사용되며 AEM Security 사용자 및 그룹을 기반으로 합니다. 그러나 이러한 기능의 응용 및 구현은 매우 다릅니다. 다음 표에 두 기능 사이의 구분이 요약되어 있습니다.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 사용 목적 | **현재** AEM 인스턴스의 콘텐츠에 대한 권한을 구성하고 적용합니다. | AEM **author** 인스턴스의 내용에 대한 CUG 정책을 구성합니다. AEM **게시** 인스턴스의 내용에 CUG 정책을 적용합니다. |
| 권한 수준 | 모든 수준의 사용자/그룹에 대해 허용된/거부된 권한을 정의합니다.읽기, 수정, 만들기, 삭제, ACL 읽기, ACL 편집, 복제 | 사용자/그룹 집합에 대한 읽기 액세스 권한을 부여합니다. *다른 모든* 사용자/그룹에 대한 읽기 액세스를 거부합니다. |
| 발행 | ACL은 *컨텐츠와 함께 게시되지 않음*&#x200B;입니다. | CUG 정책 *은(는) 컨텐트와 함께 게시됩니다.* |

## 지원 링크 {#supporting-links}

* [자산 및 닫힌 사용자 그룹 관리](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [폐쇄된 사용자 그룹 만들기](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak 닫힌 사용자 그룹 설명서](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
