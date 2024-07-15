---
title: 페이지 구성 요소를 새 적응형 양식 템플릿과 연결
description: 새 페이지 구성 요소 만들기
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 4%

---

# 페이지 구성 요소를 템플릿과 연결

다음 단계는 페이지 구성 요소를 새 적응형 양식 템플릿과 연결하는 것입니다. 이렇게 하면 새 템플릿을 기반으로 하는 적응형 양식이 렌더링될 때마다 페이지 구성 요소의 코드가 실행됩니다. 이 자습서에서는 **AzurePortalStorage** 폴더에 **StoreAndRestoreFromAzure**라는 새로운 적응형 양식 템플릿을 만들었습니다.
/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content 노드로 이동하여 다음 속성을 추가하고 변경 사항을 저장합니다.

| **속성 이름** | **속성 유형** | **속성 값** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 문자열 | azureportalpagecomponent/component/page/storeandfetch |

/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorermazure/structure/jcr:content 노드로 이동하여 다음 속성을 추가하고 변경 사항을 저장합니다.
| **속성 이름**  | **속성 유형** | **속성 값**                                    |
-------------------- --------------------------------------------------------------------------
| sling:resourceType | 문자열            | azureportalpagecomponent/component/page/storeandfetch |


## 다음 단계

[Azure Storage와의 통합 만들기](./create-fdm.md)
