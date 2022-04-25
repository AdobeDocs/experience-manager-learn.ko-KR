---
title: Display multiple pdf documents
description: Cycle through multiple pdf documents in an adaptive form.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
source-git-commit: cb5b3eb77a57fa8a2918710b7dbcd1b0a58b74bd
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# 회전판에 여러 pdf 문서 표시

일반적인 사용 사례는 양식을 제출하기 전에 검토하도록 양식 필러에 여러 PDF 문서를 표시하는 것입니다.

이 사용 사례를 달성하기 위해 [Adobe PDF 포함 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[A live demo of this sample can be experienced here.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

통합을 완료하기 위해 다음 단계가 수행되었습니다

## 여러 PDF 문서를 표시할 사용자 지정 구성 요소 만들기

pdf 문서를 순환하기 위해 사용자 정의 구성 요소(pdf-carousel)가 생성되었습니다

## 클라이언트 라이브러리

Adobe PDF 포함 API를 사용하여 PDF을 표시하기 위해 클라이언트 라이브러리가 생성되었습니다. 표시할 PDF은 pdf 회전 구성 요소에 지정됩니다.

## Create Adaptive Form

Create an adaptive form based with some tabs (This sample has 3 tabs)
Add some adaptive form components in the first two tabs
Add the pdf carousel component in the third tab
Configure the pdf-carousel component as shown in the screenshot below
![pdf-carousel](assets/pdf-carousel-af-component.png)

**포함 PDF API 키** - pdf를 포함하는 데 사용할 수 있는 키입니다. This key will only work with localhost. You can create [your own key](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) and associate it with other domain.

**PDF 문서 지정** - 여기서는 회전판에 표시할 pdf 문서를 지정할 수 있습니다.


## 서버에 샘플 배포

로컬 서버에서 테스트하려면 다음 단계를 수행합니다.

1. [클라이언트 라이브러리 가져오기](assets/pdf-carousel-client-lib.zip) 로컬 AEM 인스턴스에 추가 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [pdf 회전 구성 요소 가져오기](assets/pdf-carousel-component.zip) 로컬 AEM 인스턴스에 추가 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [적응형 양식 가져오기 ](assets/adaptive-form-pdf-carousel.zip) 로컬 AEM 인스턴스에 추가 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [표시할 샘플 pdf를 가져옵니다](assets/pdf-carousel-sample-documents.zip) 로컬 AEM 인스턴스에 추가 [자산 파일 업로드 링크 사용](http://localhost:4502/assets.html/content/dam)
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Tab to the Documents to Review tab. 회전판 구성 요소에 3개의 PDF 문서가 표시됩니다.
