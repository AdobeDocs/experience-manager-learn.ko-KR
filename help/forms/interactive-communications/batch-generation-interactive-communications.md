---
title: Batch API를 사용하여 대화형 통신 문서 생성
description: 일괄 처리 API를 사용하여 인쇄 채널 문서를 생성하기 위한 샘플 에셋
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 16%

---


# 일괄 처리 API

배치 API를 사용하여 템플릿에서 여러 대화형 커뮤니케이션을 생성할 수 있습니다. 템플릿은 데이터가 없는 대화형 커뮤니케이션입니다. 배치 API는 데이터를 템플릿과 결합하여 대화형 커뮤니케이션을 생성합니다. API는 대량의 대화형 커뮤니케이션 제작 시 유용합니다. 예를 들면 여러 고객을 위한 전화 요금 청구서, 신용 카드 명세서 등이 있습니다.

[일괄 처리 생성 API에 대한 자세한 내용](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

이 문서에서는 배치 API를 사용하여 Interactive Communications 문서를 생성하는 샘플 에셋을 제공합니다.

## 감시 폴더를 사용한 배치 생성

* 대화형 [통신 템플릿을](assets/Beneficiaries-confirmation.zip) AEM Forms 서버로 가져옵니다.
* 감시 [폴더 구성을 가져옵니다](assets/batch-generation-api.zip). 그러면 C 드라이브에서 `batchAPI` 호출된 폴더가 생성됩니다.

**비windows 운영 체제에서 AEM Forms을 실행하는 경우 아래 3단계를 따르십시오.**

1. [감시 폴더 열기](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolder를 선택하고 편집을 클릭합니다.
3. 운영 체제와 일치하도록 경로를 변경합니다.

![경로](assets/watched-folder-batch-api-basic.PNG)

* zip 파일의 내용을 [다운로드하고 추출합니다](assets/jsonfile.zip). zip 파일에는 파일이 `jsonfile` 들어 있는 폴더 `beneficiaries.json` 가 들어 있습니다. 이 파일에는 3개의 문서를 생성하는 데이터가 있습니다.

* 감시 폴더의 입력 폴더에 `jsonfile` 폴더를 놓습니다.
* 처리를 위해 폴더를 선택하면 감시 폴더의 결과 폴더를 확인합니다. 생성된 3개의 PDF 파일이 있어야 합니다.

## REST 요청을 사용한 배치 생성

REST 요청을 통해 [배치](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) API를 호출할 수 있습니다. 다른 응용 프로그램의 REST 끝점을 노출하여 API를 호출하여 문서를 생성할 수 있습니다.
제공된 샘플 자산은 REST 끝점을 노출하여 대화형 통신 문서를 생성합니다. 서블릿은 다음 매개 변수를 허용합니다.

* fileName - 파일 시스템에 있는 데이터 파일의 위치입니다.
* templatePath - IC 템플릿 경로
* saveLocation - 생성된 문서를 파일 시스템에 저장할 위치
* channelType - Print, Web 또는 both
* recordId - 인터랙티브한 통신의 이름을 설정하는 요소에 대한 JSON 경로

다음 스크린샷은 매개 변수 및 해당 값![샘플 요청을 보여줍니다](assets/generate-ic-batch-servlet.PNG)

## 서버에 샘플 에셋 배포

* 패키지 [관리자를](assets/ICTemplate.zip) 사용하여 ICT 템플릿 [가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* 패키지 [관리자를 사용하여 사용자 지정 제출 처리기](assets/BatchAPICustomSubmit.zip) [가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* Forms [및 문서](assets/BatchGenerationAPIAF.zip) 인터페이스를 사용하여 [적응형 양식가져오기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Felix 웹 콘솔을 사용하여 [맞춤형 OSGI 번들](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) [배포 및 시작](http://localhost:4502/system/console/bundles)
* [양식을 제출하여 일괄 처리 생성 트리거](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
