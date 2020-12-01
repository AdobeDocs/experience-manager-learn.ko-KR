---
title: HTM5 양식 제출 시 AEM 워크플로우 트리거
seo-title: HTML5 양식 제출 시 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
seo-description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# 제출된 PDF 검토 및 승인 워크플로우

마지막 단계와 마지막 단계는 AEM 워크플로우를 만들어 검토 및 승인을 위해 정적이거나 비대화형 PDF를 생성합니다. 워크플로우는 `/content/pdfsubmissions` 노드에 구성된 AEM Launcher를 통해 트리거됩니다.

다음 스크린샷은 워크플로우와 관련된 단계를 보여줍니다.

![workflow](assets/workflow.PNG)

## 비대화형 PDF 워크플로우 생성 단계

XDP 템플릿 및 템플릿과 병합할 데이터는 여기에 지정됩니다. 병합할 데이터는 PDF에서 제출한 데이터입니다. 이 전송된 데이터는 노드 `/content/pdfsubmissions`에 저장됩니다.

![워크플로우](assets/generate-pdf1.PNG)

생성된 PDF가 `submittedPDF`이라는 워크플로우 변수에 할당됩니다.

![워크플로우](assets/generate-pdf2.PNG)

### 검토 및 승인을 위해 생성된 pdf 할당

작업 워크플로우 지정 구성 요소는 여기에서 검토 및 승인을 위해 생성된 PDF를 할당하는 데 사용됩니다. `submittedPDF` 변수는 작업 할당 워크플로우 구성 요소의 Forms 및 문서 탭에서 사용됩니다.

![워크플로우](assets/assign-task.PNG)
