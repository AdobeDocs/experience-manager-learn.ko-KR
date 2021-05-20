---
title: AEM Forms을 사용한 Acrobat
seo-title: Acroform과 적응형 양식 데이터 병합
description: Acroforms와 AEM Forms을 통합하는 자습서의 3부. 시스템에서 워크플로우 및 적응형 양식을 테스트합니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# 시스템에서 이 기능 테스트

[이 패키지를 다운로드하여 ](assets/acro-form-aem-form.zip)
AEM으로 가져오십시오. 이 패키지에는 업로드된 Acrobat에서 스키마를 생성할 수 있는 샘플 워크플로우 및 html 페이지가 포함되어 있습니다.

## 워크플로우 구성

1. [편집 모드에서 워크플로우 모델 을 엽니다](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. MergeAcroformData 단계의 구성 속성을 엽니다.
3. 프로세스 탭을 클릭합니다.
4. 전달하는 인수가 서버의 올바른 폴더인지 확인하십시오.
5. 변경 사항을 저장합니다.

## 적응형 양식 만들기

1. 이전 단계에서 만든 스키마를 사용하여 적응형 양식을 만듭니다.
2. 몇 가지 스키마 요소를 적응형 양식에 드래그하여 놓습니다.
3. AEM 작업 과정에 제출할 적응형 양식의 제출 작업을 구성합니다(MergeAcroformData).
4. **데이터 파일 경로를 &quot;Data.xml&quot;로 지정해야 합니다. 이는 샘플 코드가 워크플로우 페이로드에서 Data.xml이라는 파일을 찾으므로 매우 중요합니다.**
5. 적응형 양식을 미리 보고 양식을 입력한 다음 제출합니다.
6. 구성 워크플로우 아래의 4단계에서 지정한 폴더에 병합된 데이터가 있는 PDF가 표시됩니다

>[!NOTE]
>
>데이터를 Acrobat과 병합하여 생성된 pdf는 워크플로우의 페이로드 폴더 아래에 pdfdocument.pdf로 저장됩니다. 그런 다음 이 문서를 워크플로우의 일부로 추가 처리에 사용할 수 있습니다
