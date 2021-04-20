---
title: AEM에서 Project Master를 사용하는 방법
description: Project Masters를 사용하면 AEM Projects를 통해 사용자 및 팀 관리를 크게 간소화할 수 있습니다.
version: 6.4, 6.5, cloud-service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: Business Practitioner
kt: 256
thumbnail: 17740.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---


# 프로젝트 마스터 사용

프로젝트 마스터는 [!DNL AEM Projects]을(를) 사용하여 사용자 및 팀 관리를 크게 간소화합니다.

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

관리자는 이제 **[!DNL Master Project]**&#x200B;을(를) 만들고 프로젝트 팀의 일부로서 역할/권한에 사용자를 할당할 수 있습니다. 프로젝트는 프로젝트에서 기본 제작할 수 있으며 자동으로 팀 멤버십을 상속할 수 있습니다. 다음과 같은 이점이 있습니다.

* 여러 프로젝트에서 기존 팀 재사용
* 수동으로 다시 만들지 않아도 되므로 프로젝트 제작 시간 단축
* 중앙에서 팀 멤버십을 관리하고 모든 팀 업데이트는 프로젝트에 의해 자동으로 상속됩니다.
* 성능 문제를 야기할 수 있는 중복 ACL 생성 방지

[!DNL Master Projects] AEM 프로젝트 아래의   Mastersfolder 아래에  [!UICONTROL 만들 수 있습니다]. 프로젝트를 기본 만들면 새 프로젝트를 만들 때 마법사에서 사용 가능한 템플릿과 함께 옵션으로 표시됩니다.

[!DNL Project Masters] URL(로컬 AEM 작성자 인스턴스): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 삭제 [!DNL Project Masters]

마스터 프로젝트를 삭제하면 파생된 프로젝트를 사용할 수 없게 됩니다.

마스터 프로젝트를 삭제하기 전에 파생된 모든 프로젝트가 완료되어 AEM에서 제거되었는지 확인합니다. 파생된 프로젝트를 제거하기 전에 필요한 프로젝트 데이터를 저장해야 합니다. 파생된 모든 프로젝트가 AEM에서 제거되면 마스터 프로젝트를 안전하게 삭제할 수 있습니다.

## [!DNL Project Masters]을(를) 비활성 상태로 표시

마스터 프로젝트의 상태를 프로젝트의 속성에서 비활성 상태로 변경하면 비활성 마스터 프로젝트가 마스터 프로젝트 목록에서 사라집니다.

비활성 마스터 프로젝트를 표시하려면 상단 막대의 &quot;활성 표시&quot; 필터 단추(목록 표시 전환 옆)를 전환합니다. 비활성 프로젝트를 다시 활성화하려면 비활성 마스터 프로젝트를 선택하고 프로젝트 속성을 편집한 다음 다시 한 번 활성화되도록 설정하기만 하면 됩니다.

## [!DNL Project Masters] 이해

![프로젝트 마스터 기술 보기](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] AEM 사용자 그룹(소유자, 편집기 및 관찰자) 세트를 정의하고 파생된 프로젝트에서 중앙에 정의된 사용자 그룹을 참조하고 재사용할 수 있도록 함으로써 작업합니다.

이렇게 하면 AEM에 필요한 전체 사용자 그룹 수가 줄어듭니다. [!DNL Project Masters] 이전에 각 프로젝트는 함께 사용 중인 ACE를 사용하여 3개의 사용자 그룹을 생성했으므로 100개의 프로젝트에 300개의 사용자 그룹이 생성되었습니다. 공유 멤버십이 프로젝트 전반의 비즈니스 요구 사항에 부합한다고 가정할 때 프로젝트 마스터를 사용하면 동일한 3개 그룹을 재사용할 수 있습니다.
