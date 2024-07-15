---
title: 자동 시작 워크플로우
description: 자동 시작 워크플로우는 업로드 또는 재처리 시 사용자 지정 워크플로를 자동으로 호출하여 자산 처리를 확장합니다.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 자동 시작 워크플로우

자동 시작 워크플로우는 자산 처리가 완료되면 업로드 또는 재처리 시 사용자 지정 워크플로를 자동으로 호출하여 AEM as a Cloud Service의 자산 처리를 확장합니다.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>워크플로우 런처를 사용하는 대신 자산 후처리를 사용자 지정하려면 자동 시작 워크플로우를 사용하십시오. 에셋 처리가 완료되면 에셋 처리 중에 여러 번 호출될 수 있는 런처 대신 자동 시작 워크플로우가 _only_&#x200B;됩니다.

## Post 처리 워크플로우 사용자 지정

Post 처리 워크플로를 사용자 지정하려면 기본 Assets Cloud Post 처리 [워크플로 모델](../../foundation/workflow/use-the-workflow-editor.md)을(를) 복사합니다.

1. _도구_ > _워크플로_ > _모델_&#x200B;로 이동하여 워크플로 모델 화면에서 시작하십시오.
2. _Assets Cloud Post 처리_ 워크플로우 모델<br/> 찾기 및 선택
   ![Assets Cloud Post 처리 워크플로 모델 선택](assets/auto-start-workflow-select-workflow.png)
3. 사용자 지정 워크플로를 만들려면 _복사_ 단추를 선택하세요.
4. 지금 워크플로 모델(_Assets Cloud Post-Processing1_)을 선택하고 _편집_ 단추를 클릭하여 워크플로를 편집합니다
5. 워크플로 속성에서 사용자 지정 Post 처리 워크플로에 의미 있는 이름<br/>을(를) 지정하십시오.
   ![이름 변경](assets/auto-start-workflow-change-name.png)
6. 비즈니스 요구 사항을 충족하는 단계를 추가합니다. 이 경우 에셋의 처리가 완료되면 작업을 추가합니다. 워크플로우의 마지막 단계가 항상 _워크플로우 완료_ 단계<br/>인지 확인하십시오.
   ![워크플로 단계 추가](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >자동 시작 워크플로는 모든 자산 업로드 또는 재처리에서 실행되므로 특히 [일괄 가져오기](../../cloud-service/migration/bulk-import.md) 또는 마이그레이션과 같은 대량 작업에 대해 워크플로 단계의 크기 조정 의미를 신중하게 고려하십시오.

7. 변경 내용을 저장하고 워크플로 모델을 동기화하려면 _동기화_ 단추를 선택하십시오.

## 사용자 지정 Post 처리 워크플로우 사용

사용자 지정 Post 처리가 폴더에 구성됩니다. 폴더에서 사용자 지정 Post 처리 워크플로우를 구성하려면 다음을 수행합니다.

1. 워크플로를 구성할 폴더를 선택하고 폴더 속성을 편집합니다
2. _자산 처리_ 탭으로 전환
3. _워크플로 자동 시작_ 선택 상자<br/>에서 사용자 지정 Post 처리 워크플로를 선택합니다.
   ![Post 처리 워크플로 설정](assets/auto-start-workflow-set-workflow.png)
4. 변경 사항 저장

이제 사용자 지정 Post 처리 워크플로우가 해당 폴더에서 업로드되거나 다시 처리되는 모든 자산에 대해 실행됩니다.
