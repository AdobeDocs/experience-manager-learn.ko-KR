---
title: AEM Forms에서 출력 및 Forms 서비스를 사용한 개발
seo-title: AEM Forms에서 출력 및 Forms 서비스를 사용한 개발
description: AEM Forms에서 출력 및 Forms 서비스 API 사용
seo-description: AEM Forms에서 출력 및 Forms 서비스 API 사용
feature: Forms Service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 2%

---


# AEM Forms에서 Forms 서비스를 사용하여 인터랙티브한 PDF 렌더링

AEM Forms에서 Forms Service API를 사용하여 인터랙티브한 PDF 렌더링

이 문서에서는 다음 서비스를 살펴보겠습니다.

* FormsService - PDF 파일에서 데이터를 가져오거나 내보낼 수 있고 xml 데이터를 xdp 템플릿으로 병합하여 인터랙티브한 pdf를 생성할 수 있는 매우 다양한 서비스입니다

AEM Forms API에 대한 공식 javadoc는 [여기에](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) 나열됩니다.

다음 코드 조각은 FormsService의 renderPDForm 작업을 사용하여 대화형 pdf를 렌더링합니다. schengen.xdp는 xml 데이터를 병합하는 데 사용되는 템플릿입니다.

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

1행:xdp 템플릿이 포함된 폴더의 위치

라인2-4:PDFormRenderOptions를 만들고 해당 속성을 설정합니다.

7행:FormsService의 renderPDForm 서비스 작업을 사용하여 인터랙티브한 PDF 생성

11행:생성된 대화형 pdf를 호출하는 응용 프로그램에 반환합니다.

**시스템에서 샘플 패키지를 테스트하려면**
1. [Felix Web Console을 사용하여 DocumentServices 샘플 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEM 패키지 관리자를 사용하여 패키지 다운로드 및 설치](assets/downloadinteractivepdffrommobileform.zip)



1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF 필터 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다.
1. /bin/generateinteractivepdf
1. [모바일 양식 열기](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 두 개의 필드를 채운 다음 ***다운로드 및 채우기...를 클릭합니다..*** 단추
1. 인터랙티브한 pdf를 로컬 시스템에 다운로드해야 합니다.


샘플 패키지에는 모바일 양식과 연결된 사용자 지정 프로필이 포함되어 있습니다. [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 파일을 탐색하십시오. 이 jsp는 모바일 양식에서 데이터를 추출하고 POST 요청을 ***/bin/generateinteractivepdf*** 경로에 마운트된 서블릿으로 만듭니다. 서블릿은 대화형 pdf를 호출 응용 프로그램으로 반환합니다. customtoolbar.jsp의 코드가 로컬 시스템에 파일을 다운로드합니다


