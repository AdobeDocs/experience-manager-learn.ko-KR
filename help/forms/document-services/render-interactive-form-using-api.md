---
title: AEM Forms에서 Forms Services를 사용하여 인터랙티브한 PDF 렌더링
description: AEM Forms에서 Forms 서비스 API를 사용하여 대화형 PDF 렌더링
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# AEM Forms에서 Forms Services를 사용하여 인터랙티브한 PDF 렌더링

AEM Forms에서 Forms 서비스 API를 사용하여 대화형 PDF 렌더링

이 문서에서는 다음 서비스를 살펴보겠습니다

* FormsService - 다양한 용도로 사용할 수 있는 서비스로, PDF 파일에서 로 데이터를 내보내고 가져올 수 있으며 xml 데이터를 xdp 템플릿으로 병합하여 대화형 pdf를 생성할 수도 있습니다

AEM Forms API용 공식 [javadoc이 여기에 나열되어 있습니다](https://helpx.adobe.com/kr/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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

1행: xdp 템플릿이 포함된 폴더의 위치

Line2-4: PDFormRenderOptions를 만들고 해당 속성을 설정합니다.

7행: FormsService의 renderPDFForm 서비스 작업을 사용하여 대화형 PDF 생성

11행: 생성된 대화형 pdf를 호출 응용 프로그램으로 반환

**시스템에서 샘플 패키지를 테스트하려면**
1. [DevelopingWithServiceUserBundle 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Felix 웹 콘솔을 사용하여 DocumentServices 샘플 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEM 패키지 관리자를 사용하여 패키지 다운로드 및 설치](assets/downloadinteractivepdffrommobileform.zip)

1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF 필터 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다
1. /bin/generateinteractivepdf
1. _Apache Sling Service 사용자 매퍼 서비스_&#x200B;를 검색하고 클릭하여 속성을 엽니다.
   1. *+* 아이콘(더하기)을 클릭하여 다음 서비스 매핑을 추가합니다
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. &#39; 저장 &#39; 클릭
1. [모바일 양식 열기](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 필드 두 개를 입력한 다음 ***다운로드 및 채우기 단추를 ....*** 단추
1. 대화형 PDF는 로컬 시스템으로 다운로드해야 합니다


샘플 패키지에는 모바일 양식과 연결된 사용자 지정 프로필이 포함되어 있습니다. [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 파일을 살펴보십시오. 이 jsp는 모바일 양식에서 데이터를 추출하고 ***/bin/generateinteractivepdf*** 경로에 탑재된 서블릿에 POST 요청을 수행합니다. 서블릿은 대화형 pdf를 호출 응용 프로그램으로 반환합니다. 그런 다음 customtoolbar.jsp의 코드는 파일을 로컬 시스템에 다운로드합니다
