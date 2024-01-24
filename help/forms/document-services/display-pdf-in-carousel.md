---
title: 여러 PDF 문서 표시
description: 적응형 양식에서 여러 PDF 문서를 반복합니다.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 74
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# 캐러셀에 여러 PDF 문서 표시

일반적인 사용 사례는 양식을 제출하기 전에 검토하기 위해 양식 작성기에 여러 PDF 문서를 표시하는 것입니다.

이 사용 사례를 달성하기 위해 [Adobe PDF 포함 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[이 샘플의 라이브 데모는 여기에서 경험할 수 있습니다.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

통합을 완료하기 위해 다음 단계를 수행했습니다

## 여러 PDF 문서를 표시할 맞춤형 구성 요소 만들기

pdf 문서를 주기 위해 맞춤형 구성 요소(pdf-carousel)가 생성되었습니다

## 클라이언트 라이브러리

Adobe PDF Embed API를 사용하여 PDF을 표시하는 클라이언트 라이브러리가 생성되었습니다. 표시할 PDF은 pdf-carousel 구성 요소에 지정됩니다.

## 적응형 양식 만들기

일부 탭을 기반으로 적응형 양식 만들기(이 샘플에는 3개의 탭이 있음) 처음 두 탭에서 일부 적응형 양식 구성 요소 추가 세 번째 탭에서 pdf 슬라이드 구성 요소 추가 아래 스크린샷과 같이 pdf 슬라이드 구성 요소 구성
![pdf-carousel](assets/pdf-carousel-af-component.png)

**포함 PDF API 키** - PDF를 포함하는 데 사용할 수 있는 키입니다. 이 키는 localhost에서만 작동합니다. 다음을 만들 수 있습니다. [사용자 키](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 다른 도메인과 연결합니다.

**PDF 문서 지정** - 여기서는 회전판에 표시할 pdf 문서를 지정할 수 있습니다.


## 서버에 샘플 배포

로컬 서버에서 테스트하려면 다음 단계를 수행하십시오.

1. [클라이언트 라이브러리 가져오기](assets/pdf-carousel-client-lib.zip) 로컬 AEM 인스턴스로 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [PDF 슬라이드 구성 요소 가져오기](assets/pdf-carousel-component.zip) 로컬 AEM 인스턴스로 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [적응형 양식 가져오기](assets/adaptive-form-pdf-carousel.zip) 로컬 AEM 인스턴스로 [패키지 관리자 사용](http://localhost:4502/crx/packmgr/index.jsp)
1. [표시할 샘플 pdf 가져오기](assets/pdf-carousel-sample-documents.zip) 로컬 AEM 인스턴스로 [에셋 파일 업로드 링크 사용](http://localhost:4502/assets.html/content/dam)
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 탭으로 이동하여 검토할 문서 탭을 표시합니다. 슬라이드 구성 요소에 3개의 PDF 문서가 표시됩니다.
