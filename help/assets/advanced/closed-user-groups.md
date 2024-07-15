---
title: AEM Assets의 폐쇄형 사용자 그룹
description: 폐쇄된 사용자 그룹(CUG)은 콘텐츠에 대한 액세스를 게시된 사이트에서 선택한 사용자 그룹으로 제한하는 데 사용되는 기능입니다. 이 비디오는 폐쇄된 사용자 그룹을 Adobe Experience Manager Assets과 함께 사용하여 어떻게 특정 에셋 폴더에 대한 액세스를 제한할 수 있는지 보여 줍니다.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 폐쇄된 사용자 그룹{#using-closed-user-groups-with-aem-assets}

폐쇄된 사용자 그룹(CUG)은 콘텐츠에 대한 액세스를 게시된 사이트에서 선택한 사용자 그룹으로 제한하는 데 사용되는 기능입니다. 이 비디오는 폐쇄된 사용자 그룹을 Adobe Experience Manager Assets과 함께 사용하여 어떻게 특정 에셋 폴더에 대한 액세스를 제한할 수 있는지 보여 줍니다. AEM 6.4에서 AEM Assets을 사용하는 폐쇄형 사용자 그룹에 대한 지원이 처음 도입되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assets이 있는 폐쇄형 사용자 그룹(CUG)

* AEM Publish 인스턴스의 자산에 대한 액세스를 제한하도록 설계되었습니다.
* 사용자/그룹 집합에 대한 읽기 액세스 권한을 부여합니다.
* CUG는 폴더 수준에서만 구성할 수 있습니다. 개별 자산에 대해 CUG를 설정할 수 없습니다.
* CUG 정책은 하위 폴더 및 적용된 에셋에 의해 자동으로 상속됩니다.
* 새 CUG 정책을 설정하여 하위 폴더에서 CUG 정책을 재정의할 수 있습니다. 이 변수는 제한적으로 사용해야 하며 모범 사례로 간주되지 않습니다.

## 폐쇄된 사용자 그룹과 액세스 제어 목록 {#closed-user-groups-vs-access-control-lists}

CUG(폐쇄형 사용자 그룹)와 ACL(액세스 제어 목록)은 모두 AEM의 콘텐츠에 대한 액세스를 제어하는 데 사용되며 AEM Security 사용자 및 그룹을 기반으로 합니다. 그러나 이러한 기능의 적용 및 구현은 매우 다릅니다. 다음 표에는 두 피쳐의 차이점이 요약되어 있습니다.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 의도한 사용 | **현재** AEM 인스턴스의 콘텐츠에 대한 권한을 구성하고 적용하십시오. | AEM **작성자** 인스턴스의 콘텐츠에 대한 CUG 정책을 구성합니다. AEM **publish** 인스턴스의 콘텐츠에 대한 CUG 정책을 적용합니다. |
| 권한 수준 | 읽기, 수정, 만들기, 삭제, ACL 읽기, ACL 편집, 복제와 같은 모든 수준의 사용자/그룹에 대해 부여되거나 거부된 권한을 정의합니다. | 사용자/그룹 집합에 대한 읽기 액세스 권한을 부여합니다. *다른 모든* 사용자/그룹에 대한 읽기 액세스를 거부합니다. |
| 발행 | ACL이 콘텐츠로 게시된 *not*&#x200B;입니다. | CUG 정책 *은(는)* 콘텐츠로 게시되었습니다. |

## 지원 링크 {#supporting-links}

* [Assets 및 폐쇄된 사용자 그룹 관리](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [폐쇄된 사용자 그룹 만들기](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak 폐쇄 사용자 그룹 설명서](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
