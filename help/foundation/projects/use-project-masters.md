---
title: AEM에서 프로젝트 마스터를 사용하는 방법
description: 프로젝트 마스터는 AEM Projects를 사용하여 사용자 및 팀 관리를 크게 단순화합니다.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# 프로젝트 마스터 사용

프로젝트 마스터를 통해 사용자 및 팀 관리를 크게 간소화 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

관리자는 이제 다음을 만들 수 있습니다. **[!DNL Master Project]** 프로젝트 팀의 일부로 역할/권한에 사용자를 할당합니다. 프로젝트는 기본 프로젝트에서 만들 수 있으며 자동으로 팀 멤버십을 상속합니다. 이는 다음과 같은 몇 가지 이점을 제공합니다.

* 여러 프로젝트에서 기존 팀 재사용
* 수동으로 팀을 다시 만들 필요가 없으므로 프로젝트 만들기 가속화
* 중앙 위치에서 팀 멤버십을 관리하며 팀에 대한 모든 업데이트는 프로젝트에 자동으로 상속됩니다
* 성능 문제를 일으킬 수 있는 중복 ACL 생성 방지

[!DNL Master Projects] 아래에 만들 수 있습니다. [!UICONTROL 마스터] 폴더 [!UICONTROL AEM 프로젝트]. 기본 프로젝트가 만들어지면 새 프로젝트를 만들 때 마법사에서 사용 가능한 템플릿과 함께 옵션으로 표시됩니다.

[!DNL Project Masters] URL(로컬 AEM 작성자 인스턴스): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 삭제 [!DNL Project Masters]

마스터 프로젝트를 삭제하면 파생 프로젝트를 사용할 수 없게 됩니다.

마스터 프로젝트를 삭제하기 전에 모든 파생 프로젝트가 완료되고 AEM에서 제거되었는지 확인하십시오. 파생 프로젝트를 제거하기 전에 필요한 프로젝트 데이터를 저장해야 합니다. 모든 파생 프로젝트가 AEM에서 제거되면 마스터 프로젝트를 안전하게 삭제할 수 있습니다.

## 표시 [!DNL Project Masters] 비활성 상태로

프로젝트 속성에서 마스터 프로젝트의 상태를 비활성으로 변경하면 비활성 마스터 프로젝트가 마스터 프로젝트 목록에서 사라집니다.

비활성 마스터 프로젝트를 표시하려면 목록 표시 토글 옆에 있는 상단 막대에서 &quot;활성 표시&quot; 필터 버튼을 토글합니다. 비활성 프로젝트를 다시 활성화하려면 비활성 마스터 프로젝트를 선택하고 프로젝트 속성을 편집한 다음 다시 활성으로 설정하기만 하면 됩니다.

## 이해 [!DNL Project Masters]

![프로젝트 마스터 기술 보기](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] AEM 사용자 그룹(소유자, 편집자 및 관찰자) 세트를 정의하고 파생 프로젝트가 중앙에 정의된 사용자 그룹을 참조하고 재사용할 수 있도록 하여 작업을 수행합니다.

이렇게 하면 AEM에 필요한 전체 사용자 그룹 수가 줄어듭니다. 다음 이전 [!DNL Project Masters], 각 프로젝트는 ACE와 함께 3개의 사용자 그룹을 생성하여 권한 처리를 시행했으며, 따라서 100개의 프로젝트에서 300개의 사용자 그룹을 생성했습니다. 공유 멤버십이 프로젝트 전체의 비즈니스 요구 사항에 부합한다고 가정할 경우 프로젝트 마스터를 사용하면 원하는 수의 프로젝트에서 동일한 세 개의 그룹을 재사용할 수 있습니다.
