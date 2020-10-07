---
title: AEM Assets의 폐쇄된 사용자 그룹
description: '폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 특정 사용자 그룹에 대한 컨텐츠 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오에서는 닫힌 사용자 그룹을 Adobe Experience Manager 자산과 함께 사용하여 특정 자산 폴더에 대한 액세스를 제한하는 방법을 보여줍니다. AEM Assets에서 폐쇄된 사용자 그룹에 대한 지원은 AEM 6.4에서 처음 도입되었습니다. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# 닫힌 사용자 그룹{#using-closed-user-groups-with-aem-assets}

폐쇄된 사용자 그룹(CUG)은 게시된 사이트의 특정 사용자 그룹에 대한 컨텐츠 액세스를 제한하는 데 사용되는 기능입니다. 이 비디오에서는 닫힌 사용자 그룹을 Adobe Experience Manager 자산과 함께 사용하여 특정 자산 폴더에 대한 액세스를 제한하는 방법을 보여줍니다. AEM Assets에서 폐쇄된 사용자 그룹에 대한 지원은 AEM 6.4에서 처음 도입되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## AEM Assets이 있는 폐쇄된 사용자 그룹(CUG)

* AEM 게시 인스턴스의 자산에 대한 액세스를 제한하도록 설계되었습니다.
* 사용자/그룹 세트에 대한 읽기 액세스 권한을 부여합니다.
* CUG는 폴더 수준에서만 구성할 수 있습니다. 개별 자산에 대해 CUG를 설정할 수 없습니다.
* CUG 정책은 하위 폴더 및 적용된 자산에 의해 자동으로 상속됩니다.
* 새 CUG 정책을 설정하여 하위 폴더로 CUG 정책을 재정의할 수 있습니다. 이 기능은 제한적으로 사용해야 하며 우수 사례로 간주되지 않습니다.

## JCR의 CUG 표현 {#cug-representation-in-the-jcr}

![JCR의 CUG 표현](assets/closed-user-groups/folder-properties-closed-user-groups.png)

We.Retail 구성원 그룹이 폐쇄된 사용자 그룹으로 폴더에 추가되었습니다./content/dam/we-retail/en/beta-products

rep:CugMixin **이** /content/dam/we-retail/en/beta-products **** 폴더에 적용됩니다. rep: **cugPolicy** 노드가 폴더 아래에 추가되고 we-retail-members가 주체로 지정됩니다. granite:AuthenticationRequired **의 또 다른 혼합이 베타 제품 폴더에 적용되며* granite:loginPath* 속성은 사용자가 인증되지 않은 경우 사용할 로그인 페이지를 지정하고** beta-products **** 폴더 아래에 자산을 요청하려고 합니다.

아래의 JCR 설명:

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## 닫힌 사용자 그룹과 액세스 제어 목록 비교 {#closed-user-groups-vs-access-control-lists}

CUG(폐쇄된 사용자 그룹)와 ACL(액세스 제어 목록)은 모두 AEM의 컨텐츠에 대한 액세스를 제어하는 데 사용되며 AEM 보안 사용자 및 그룹을 기반으로 합니다. 그러나 이러한 기능의 적용 및 구현은 매우 다릅니다. 다음 표에는 두 기능 간의 차이점이 요약되어 있습니다.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 사용 목적 | 현재 **AEM 인스턴스의 콘텐츠에 대한 권한을** 구성하고 적용합니다. | AEM **작성자** 인스턴스의 컨텐츠에 대한 CUG 정책을 구성합니다. AEM **게시** 인스턴스의 컨텐츠에 CUG 정책을 적용합니다. |
| 권한 수준 | 모든 수준의 사용자/그룹에 대해 허용된/거부된 권한을 정의합니다.읽기, 수정, 작성, 삭제, ACL 읽기, ACL 편집, 복제 | 사용자/그룹 세트에 대한 읽기 액세스 권한을 부여합니다. 다른 모든 사용자/그룹에 대한 읽기 액세스를 거부합니다. |
| 복제 | ACL은 컨텐츠와 함께 복제되지 않습니다. | CUG 정책은 컨텐츠와 함께 복제됩니다. |

## 지원 링크 {#supporting-links}

* [자산 및 닫힌 사용자 그룹 관리](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [닫힌 사용자 그룹 만들기](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Oak 폐쇄된 사용자 그룹 설명서](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
