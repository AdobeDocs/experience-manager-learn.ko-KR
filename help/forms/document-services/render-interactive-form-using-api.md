---
title: AEM Forms에서 Forms 서비스를 사용하여 대화형 PDF 렌더링
description: AEM Forms에서 Forms Service API를 사용하여 대화형 PDF 렌더링
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 1%

---

# AEM Forms에서 Forms 서비스를 사용하여 대화형 PDF 렌더링

AEM Forms에서 Forms Service API를 사용하여 대화형 PDF 렌더링

이 문서에서 다음 서비스를 살펴보겠습니다

* FormsService - 이 서비스는 다양한 기능을 제공하므로 xml 데이터를 xdp 템플릿에 병합하여 PDF 파일에서 가져오거나 내보낼 수 있고 대화형 pdf를 생성할 수도 있습니다

AEM Forms API에 대한 공식 javadoc가 나열됩니다 [여기](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

다음 코드 조각은 FormsService의 renderPDFForm 작업을 사용하여 대화형 pdf를 렌더링합니다. schengen.xdp는 xml 데이터를 병합하는 데 사용되는 템플릿입니다.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

1행: xdp 템플릿이 들어 있는 폴더의 위치

Line2-4: PDFFormRenderOptions를 만들고 해당 속성을 설정합니다

7행: FormsService의 renderPDFForm 서비스 작업을 사용하여 대화형 PDF 생성

11호선: 생성된 대화형 pdf를 호출 애플리케이션에 반환합니다

**시스템에서 샘플 패키지를 테스트하려면**
1. [Felix 웹 콘솔을 사용하여 DocumentServices 샘플 번들을 다운로드하여 설치합니다](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEM 패키지 관리자를 사용하여 패키지를 다운로드하여 설치합니다](assets/downloadinteractivepdffrommobileform.zip)



1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Granite CSRF 필터 Adobe 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다
1. /bin/generateinteractivepdf
1. [모바일 양식을 엽니다](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 두 개의 필드를 채운 다음 ***다운로드 및 채우기....*** 단추
1. 대화형 pdf는 로컬 시스템에 다운로드해야 합니다


샘플 패키지에는 Mobile 양식과 연결된 사용자 지정 프로필이 포함되어 있습니다. 다음 문서를 살펴보십시오 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 파일. 이 jsp는 모바일 양식에서 데이터를 추출하고 서블릿에 대한 POST 요청을 수행합니다 ***/bin/generateinteractivepdf*** 경로. 서블릿은 호출 애플리케이션에 대화형 pdf를 반환합니다. customtoolbar.jsp의 코드는 파일을 로컬 시스템에 다운로드합니다
