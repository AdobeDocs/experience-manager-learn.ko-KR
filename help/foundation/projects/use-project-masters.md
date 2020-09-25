---
title: AEM에서 프로젝트 마스터 사용
description: Project Master는 AEM Projects를 통해 사용자 및 팀 관리를 크게 간소화합니다.
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# 프로젝트 마스터 사용

Project Master를 사용하면 사용자 및 팀 관리를 크게 간소화할 수 있습니다 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

관리자는 이제 프로젝트 팀 **[!DNL Master Project]** 의 일부로서 역할을 만들고 사용자에게 역할/권한을 할당할 수 있습니다. 프로젝트는 기본 프로젝트에서 만들 수 있으며 팀 멤버십을 자동으로 상속받게 됩니다. 다음과 같은 여러 이점을 제공합니다.

* 여러 프로젝트에서 기존 팀 재사용
* 팀은 수동으로 다시 만들지 않아도 되므로 프로젝트 제작 시간 단축
* 중앙에서 팀 멤버십 관리 및 팀 업데이트가 프로젝트에 의해 자동으로 상속됩니다.
* 성능 문제를 야기할 수 있는 중복 ACL 생성 방지

[!DNL Master Projects] 는 [!UICONTROL AEM 프로젝트] 아래의 [!UICONTROL 마스터]폴더 아래에 만들 수 있습니다. 새 프로젝트 [!DNL Master Project] 를 만들면 마법사에서 사용 가능한 템플릿과 함께 옵션으로 표시됩니다.

[!DNL Project Masters] URL(로컬 AEM 작성자 인스턴스): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 삭제 [!DNL Project Masters]

마스터 프로젝트를 삭제하면 프로젝트를 사용할 수 없게 됩니다.

마스터 프로젝트를 삭제하기 전에 파생된 프로젝트를 모두 완료하고 AEM에서 제거해야 합니다. 파생된 프로젝트를 제거하기 전에 필요한 프로젝트 데이터를 저장해야 합니다. 파생된 프로젝트를 모두 AEM에서 제거하면 마스터 프로젝트를 안전하게 삭제할 수 있습니다.

## 비활성 [!DNL Project Masters] 으로 표시

마스터 프로젝트의 상태를 프로젝트 속성에서 비활성 상태로 변경하면 비활성 마스터 프로젝트가 마스터 프로젝트 목록에서 사라집니다.

비활성 마스터 프로젝트를 표시하려면 상단 막대(목록 표시 전환 옆에 있는 &quot;활성 상태 표시&quot; 필터 단추를 전환합니다. 비활성 프로젝트를 다시 활성화하려면 비활성 마스터 프로젝트를 선택하고 프로젝트 속성을 편집한 후 다시 한 번 활성화하면 됩니다.

## 이해 [!DNL Project Masters]

![프로젝트 마스터 기술 뷰](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] aem 사용자 그룹 집합(소유자, 편집자 및 관찰자)을 정의하고 파생된 프로젝트에 중앙에서 정의된 사용자 그룹을 참조하고 재사용할 수 있도록 함으로써 작업합니다.

이렇게 하면 AEM에 필요한 전체 사용자 그룹 수가 줄어듭니다. 이전에 각 프로젝트 [!DNL Project Masters]는 ACE와 함께 3개의 사용자 그룹을 만들어 사용 권한을 부여함으로써 100개의 프로젝트에 300개의 사용자 그룹이 참여했습니다. 공유 멤버십이 프로젝트 전체의 비즈니스 요구 사항에 부합한다고 가정할 경우, 프로젝트 마스터에서는 모든 수의 프로젝트에서 동일한 3개 그룹을 다시 사용할 수 있습니다.
