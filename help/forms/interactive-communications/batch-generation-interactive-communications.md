---
title: 배치 API를 사용하여 대화형 통신 문서 생성
description: 배치 API를 사용하여 인쇄 채널 문서를 생성하기 위한 샘플 자산
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 16%

---

# 배치 API

배치 API를 사용하여 템플릿에서 여러 대화형 커뮤니케이션을 생성할 수 있습니다. 템플릿은 데이터가 없는 대화형 커뮤니케이션입니다. 배치 API는 데이터를 템플릿과 결합하여 대화형 커뮤니케이션을 생성합니다. API는 대량의 대화형 커뮤니케이션 제작 시 유용합니다. 예를 들면 여러 고객을 위한 전화 요금 청구서, 신용 카드 명세서 등이 있습니다.

[배치 생성 API에 대해 자세히 알아보기](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

이 문서에서는 배치 API를 사용하여 인터랙티브 통신 문서를 생성하는 샘플 자산을 제공합니다.

## 감시 폴더를 사용한 배치 생성

* 가져오기 [대화형 통신 템플릿](assets/Beneficiaries-confirmation.zip) AEM Forms 서버에 연결할 수도 있습니다.
* 가져오기 [감시 폴더 구성](assets/batch-generation-api.zip). 이렇게 하면 라는 폴더가 만들어집니다 `batchAPI` C 드라이브에서

**Windows가 아닌 운영 체제에서 AEM Forms을 실행하는 경우 아래에 언급된 3단계를 수행하십시오.**

1. [감시 폴더를 엽니다.](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 일괄 처리(BatchAPIWatchedFolder)를 선택하고 편집을 누릅니다.
3. 운영 체제와 일치하도록 경로 를 변경합니다.

![경로](assets/watched-folder-batch-api-basic.PNG)

* 의 컨텐츠를 다운로드하고 추출합니다. [zip 파일](assets/jsonfile.zip). zip 파일에는 라는 폴더가 있습니다. `jsonfile` 다음 포함 `beneficiaries.json` 파일. 이 파일에는 3개의 문서를 생성하는 데이터가 있습니다.

* 를 `jsonfile` 폴더를 감시 폴더의 입력 폴더로
* 처리할 폴더를 선택했으면 감시 폴더의 결과 폴더를 확인합니다. 생성된 3개의 PDF 파일이 표시됩니다

## REST 요청을 사용한 배치 생성

를 호출할 수 있습니다 [배치 API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) 사용. 다른 응용 프로그램의 REST 엔드포인트를 노출하여 API를 호출하여 문서를 생성할 수 있습니다.
제공된 샘플 자산은 Interactive Communication 문서를 생성하기 위한 REST 끝점을 표시합니다. 서블릿은 다음 매개 변수를 허용합니다.

* fileName - 파일 시스템의 데이터 파일 위치입니다.
* templatePath - IC 템플릿 경로
* saveLocation - 생성된 문서를 파일 시스템에 저장할 위치
* channelType - 인쇄, 웹 또는 둘 다
* recordId - 대화형 통신 이름을 설정하는 요소에 대한 JSON 경로

다음 스크린샷에서는 매개 변수와 그 값을 보여줍니다
![샘플 요청](assets/generate-ic-batch-servlet.PNG)

## 서버에 샘플 자산 배포

* 가져오기 [ICTemplate](assets/ICTemplate.zip) 사용 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 가져오기 [사용자 지정 제출 처리기](assets/BatchAPICustomSubmit.zip) 사용 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 가져오기 [적응형 양식](assets/BatchGenerationAPIAF.zip) 사용 [Forms 및 문서 인터페이스](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 배포 및 시작 [사용자 지정 OSGI 번들](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 사용 [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)
* [양식을 제출하여 배치 생성을 트리거합니다](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
