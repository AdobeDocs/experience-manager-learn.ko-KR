---
title: AEM Forms이 포함된 Acroforms
description: Acroforms와 AEM Forms 통합 자습서의 3부. 시스템에서 워크플로 및 적응형 양식을 테스트합니다.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 49
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# 시스템에서 이 기능 테스트

[이 패키지를 다운로드하여 AEM에 가져오기](assets/acro-form-aem-form.zip)
이 패키지에는 업로드된 Acroform에서 스키마를 만들 수 있는 샘플 워크플로우 및 html 페이지가 포함되어 있습니다.

## 워크플로우 구성

1. [편집 모드에서 워크플로 모델 열기](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. MergeAcroformData 단계의 구성 속성을 엽니다.
3. 프로세스 탭을 클릭합니다.
4. 전달하려는 인수가 서버의 유효한 폴더인지 확인하십시오.
5. 변경 사항을 저장합니다.

## 적응형 양식 만들기

1. 이전 단계에서 만든 스키마를 사용하여 적응형 양식을 만듭니다.
2. 몇 개의 스키마 요소를에 적응형 양식으로 끌어다 놓습니다.
3. 적응형 양식의 제출 액션을 구성하여 AEM Workflow(MergeAcroformData)에 제출합니다.
4. **데이터 파일 경로를 &quot;Data.xml&quot;로 지정해야 합니다. 샘플 코드가 워크플로 페이로드에서 Data.xml이라는 파일을 검색하므로 이 작업이 매우 중요합니다.**
5. 적응형 양식을 미리 보고 양식을 입력한 후 제출합니다.
6. 구성 워크플로 아래에 4단계에서 지정한 폴더에 저장된 병합된 데이터의 PDF이 표시됩니다

>[!NOTE]
>
>데이터를 acroform과 병합하여 생성된 pdf는 워크플로우의 페이로드 폴더 아래에 pdfdocument.pdf로 저장됩니다. 그런 다음 이 문서를 워크플로우의 일부로 추가 처리에 사용할 수 있습니다
