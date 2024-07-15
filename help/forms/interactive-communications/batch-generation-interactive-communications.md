---
title: 대화형 통신 문서 생성을 위해 배치 API 사용
description: 일괄 처리 API를 사용하여 인쇄 채널 문서를 생성하기 위한 샘플 에셋
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 77
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 13%

---

# 일괄 처리 API

배치 API를 사용하여 템플릿에서 여러 대화형 커뮤니케이션을 생성할 수 있습니다. 템플릿은 데이터가 없는 대화형 커뮤니케이션입니다. 배치 API는 데이터를 템플릿과 결합하여 대화형 커뮤니케이션을 생성합니다. API는 대량의 대화형 커뮤니케이션 제작 시 유용합니다. 예를 들어 여러 고객을 위한 전화 요금 청구서, 신용 카드 명세서 등이 있습니다.

[일괄 처리 생성 API에 대해 자세히 알아보기](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

이 문서에서는 일괄 처리 API를 사용하여 대화형 통신 문서를 생성하는 샘플 에셋을 제공합니다.

## 감시 폴더를 사용한 일괄 생성

* [대화형 통신 템플릿](assets/Beneficiaries-confirmation.zip)을(를) AEM Forms 서버로 가져옵니다.
* [감시 폴더 구성](assets/batch-generation-api.zip)을 가져옵니다. C 드라이브에 `batchAPI` 폴더가 만들어집니다.

**Windows가 아닌 운영 체제에서 AEM Forms을 실행하는 경우 아래 세 단계를 따르십시오.**

1. [감시 폴더 열기](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 일괄 처리APIWatchedFolder를 선택하고 Edit를 클릭합니다.
3. 경로를 운영 체제와 일치하도록 변경합니다.

![path](assets/watched-folder-batch-api-basic.PNG)

* [zip 파일](assets/jsonfile.zip)의 내용을 다운로드하여 추출하십시오. zip 파일에 `beneficiaries.json` 파일이 포함된 `jsonfile` 폴더가 있습니다. 이 파일에는 3개의 문서를 생성할 수 있는 데이터가 있습니다.

* `jsonfile` 폴더를 감시 폴더의 입력 폴더에 놓습니다.
* 처리를 위해 폴더를 선택했으면 감시 폴더의 결과 폴더를 확인합니다. 3개의 PDF 파일이 생성되었습니다.

## REST 요청을 사용한 일괄 처리 생성

REST 요청을 통해 [일괄 처리 API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html)를 호출할 수 있습니다. 다른 응용 프로그램에 대한 REST 끝점을 노출하여 API를 호출하여 문서를 생성할 수 있습니다.
제공된 샘플 자산은 대화형 통신 문서를 생성하기 위한 REST 끝점을 노출합니다. 서블릿은 다음 매개변수를 허용합니다.

* fileName - 파일 시스템에서 데이터 파일의 위치.
* templatePath - IC 템플릿 경로
* saveLocation - 생성된 문서를 파일 시스템에 저장할 위치
* channelType - Print, Web 또는 둘 다
* recordId - 대화형 통신의 이름을 설정할 요소의 JSON 경로

다음 스크린샷에는 매개 변수와 해당 값이 표시됩니다
![샘플 요청](assets/generate-ic-batch-servlet.PNG)

## 서버에 샘플 자산 배포

* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [ICTemplate](assets/ICTemplate.zip) 가져오기
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [사용자 지정 제출 처리기](assets/BatchAPICustomSubmit.zip) 가져오기
* [Forms 및 문서 인터페이스](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)를 사용하여 [적응형 양식](assets/BatchGenerationAPIAF.zip) 가져오기
* [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [사용자 지정 OSGI 번들](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)을 배포 및 시작
* [양식을 제출하여 일괄 처리 생성 트리거](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
