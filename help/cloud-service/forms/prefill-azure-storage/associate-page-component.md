---
title: 페이지 구성 요소를 새 적응형 양식 템플릿과 연결
description: 새 페이지 구성 요소 만들기
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 5%

---

# 페이지 구성 요소를 템플릿과 연결

다음 단계는 페이지 구성 요소를 새 적응형 양식 템플릿과 연결하는 것입니다. 이렇게 하면 새 템플릿을 기반으로 하는 적응형 양식이 렌더링될 때마다 페이지 구성 요소의 코드가 실행됩니다. 이 자습서에서는 라는 새로운 적응형 양식 템플릿을 사용합니다. **Azure에서 저장 및 복원** 이(가)에서 만들어졌습니다. **AzurePortalStorage** 폴더를 삭제합니다.
/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content 노드로 이동하여 다음 속성을 추가하고 변경 사항을 저장합니다.

| **속성 이름** | **속성 유형** | **속성 값** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 문자열 | azureportalpagecomponent/component/page/storeandfetch |

/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorermazure/structure/jcr:content 노드로 이동하여 다음 속성을 추가하고 변경 사항을 저장합니다.
| **속성 이름**  | **속성 유형** | **속성 값**                                    | -------------------- -------------------------------------------------------------------------- | sling:resourceType | 문자열 | azureportalpagecomponent/component/page/storeandfetch |


## 다음 단계

[Azure Storage와의 통합 만들기](./create-fdm.md)
