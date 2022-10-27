---
title: 출력 서비스에서 조각 사용
description: crx 저장소에 있는 조각을 사용하여 pdf 문서 생성
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 1%

---

# 조각을 사용하여 pdf 문서 생성{#developing-with-output-and-forms-services-in-aem-forms}


이 문서에서는 출력 서비스를 사용하여 xdp 조각을 사용하여 pdf 파일을 생성합니다. 기본 xdp 및 조각은 crx 저장소에 있습니다. AEM에서 파일 시스템 폴더 구조를 모방하는 것이 중요합니다. 예를 들어 xdp의 조각 폴더에서 조각을 사용하는 경우 다음과 같은 폴더를 만들어야 합니다. **조각** AEM의 기본 폴더 아래에 있습니다. 기본 폴더에는 기본 xdp 템플릿이 들어 있습니다. 예를 들어 파일 시스템에 다음 구조가 있는 경우
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 폴더 xdpdocuments에는 기본 템플릿과 **조각** 폴더

를 사용하여 필요한 구조를 만들 수 있습니다 [양식 및 문서 ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

다음은 2개의 조각을 사용하는 샘플 xdp의 폴더 구조입니다
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* 출력 서비스 - 일반적으로 이 서비스는 xml 데이터를 xdp 템플릿 또는 pdf와 병합하여 병합된 pdf를 생성하는 데 사용됩니다. 자세한 내용은 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 출력 서비스에 대해 사용됩니다. 이 샘플에서는 crx 저장소에 있는 조각을 사용합니다.


다음 코드는 PDF 파일에 조각을 포함하는 데 사용되었습니다

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**시스템에서 샘플 패키지를 테스트하려면**

* [샘플 xdp 파일을 다운로드하여 AEM에 가져옵니다](assets/xdp-templates-fragments.zip)
* [AEM 패키지 관리자를 사용하여 패키지를 다운로드하여 설치합니다](assets/using-fragments-assets.zip)
* [샘플 xdp 및 조각은 여기에서 다운로드할 수 있습니다](assets/xdptemplates.zip)

**패키지를 설치한 후에는 Granite CSRF 필터허용 목록에 추가하다에 다음 URL을 배치해야 합니다.**

1. 위에 언급된 경로를 추적하려면 허용 목록에 추가하다 아래 단계를 따르십시오.
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Granite CSRF 필터 Adobe 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다
1. /content/AemFormsSamples/usingfragments

샘플 코드를 테스트하는 방법에는 여러 가지가 있습니다. 가장 빠르고 가장 쉬운 것은 Postman 앱을 사용하는 것입니다. Postman을 사용하면 서버에 POST 요청을 수행할 수 있습니다. 시스템에 Postman 앱을 설치합니다.
앱을 실행하고 다음 URL을 입력하여 데이터 내보내기 API를 테스트합니다

드롭다운 목록에서 &quot;POST&quot;을 선택했는지 확인합니다. http://localhost:4502/content/AemFormsSamples/usingfragments.html &quot;Authorization&quot;을 &quot;Basic Auth&quot;로 지정합니다. AEM Server 사용자 이름 및 암호 지정 &quot;Body&quot; 탭으로 이동하여 아래 이미지에 표시된 대로 요청 매개 변수를 지정합니다
![내보내기](assets/using-fragment-postman.png)
그런 다음 보내기 단추를 클릭합니다

[이 postman 컬렉션을 가져와서 API를 테스트할 수 있습니다](assets/usingfragments.postman_collection.json)
