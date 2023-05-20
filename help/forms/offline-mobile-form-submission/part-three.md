---
title: HTM5 양식 제출에서 AEM 워크플로우 트리거 - PDF 검토 및 승인
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 오프라인 모드에서 모바일 양식을 계속 채우고 AEM 워크플로우를 트리거하기 위한 모바일 양식을 제출합니다.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# 제출된 PDF을 검토하고 승인하는 워크플로우

마지막 및 마지막 단계는 검토 및 승인을 위한 정적 또는 비대화형 PDF을 생성하는 AEM 워크플로우를 만드는 것입니다. 워크플로우는 노드에 구성된 AEM 런처를 통해 트리거됩니다. `/content/pdfsubmissions`.

다음 스크린샷은 워크플로우와 관련된 단계를 보여 줍니다.

![워크플로우](assets/workflow.PNG)

## 비대화형 PDF 워크플로 단계 생성

XDP 템플릿과 템플릿에 병합할 데이터가 여기에 지정됩니다. 병합할 데이터는 PDF에서 제출된 데이터입니다. 이렇게 제출된 데이터는 노드 아래에 저장됩니다 `/content/pdfsubmissions`.

![워크플로우](assets/generate-pdf1.PNG)

생성된 PDF은 이라는 워크플로 변수에 할당됩니다. `submittedPDF`.

![워크플로우](assets/generate-pdf2.PNG)

### 검토 및 승인을 위해 생성된 PDF 할당

작업 워크플로 구성 요소 할당은 여기에서 검토 및 승인을 위해 생성된 PDF을 할당하는 데 사용됩니다. 변수 `submittedPDF` 은 작업 할당 워크플로 구성 요소의 Forms 및 문서 탭에서 사용됩니다.

![워크플로우](assets/assign-task.PNG)
