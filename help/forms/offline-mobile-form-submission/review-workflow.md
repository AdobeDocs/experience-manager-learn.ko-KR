---
title: HTML5 양식 제출에서 AEM 워크플로우 트리거 - PDF 검토 및 승인
description: 제출된 PDF을 검토하는 워크플로우
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# 제출된 PDF을 검토하고 승인하는 워크플로우

마지막 및 마지막 단계는 검토 및 승인을 위해 정적 또는 비대화형 AEM을 생성하는 PDF 워크플로우를 만드는 것입니다. 워크플로는 `/content/formsubmissions` 노드에 구성된 AEM 런처를 통해 트리거됩니다.

다음 스크린샷은 워크플로우와 관련된 단계를 보여 줍니다.

![워크플로](assets/workflow.PNG)

## 비대화형 PDF 워크플로 단계 생성

XDP 템플릿과 템플릿에 병합할 데이터가 여기에 지정됩니다. 병합할 데이터는 PDF에서 제출된 데이터입니다. 이 제출된 데이터는 ```/content/formsubmissions``` 노드에 저장됩니다.

![워크플로](assets/generate-pdf1.PNG)

생성된 PDF은 `submittedPDF`(이)라는 워크플로 변수에 할당됩니다.

![워크플로](assets/generate-pdf2.PNG)

### 검토 및 승인을 위해 생성된 PDF 할당

작업 워크플로 구성 요소 할당은 여기에서 검토 및 승인을 위해 생성된 PDF을 할당하는 데 사용됩니다. 변수 `submittedPDF`은(는) 작업 할당 워크플로 구성 요소의 Forms 및 문서 탭에서 사용됩니다.

![워크플로](assets/assign-task.PNG)


## 다음 단계

[환경에 자산 배포](./deploy-assets.md)