---
title: 감시 폴더가 있는 출력 서비스에서 조각 사용
description: crx 저장소에 있는 조각을 사용하여 pdf 문서 생성
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# ECMA 스크립트를 사용하여 조각으로 pdf 문서 생성{#developing-with-output-and-forms-services-in-aem-forms}


이 문서에서는 출력 서비스를 사용하여 xdp 조각을 사용하여 pdf 파일을 생성합니다. 기본 xdp 및 조각은 crx 저장소에 있습니다. AEM에서 파일 시스템 폴더 구조를 모방하는 것이 중요합니다. 예를 들어 xdp의 조각 폴더에서 조각을 사용하는 경우 다음과 같은 폴더를 만들어야 합니다. **조각** AEM의 기본 폴더 아래에 있습니다. 기본 폴더에는 기본 xdp 템플릿이 들어 있습니다. 예를 들어 파일 시스템에 다음 구조가 있는 경우
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 폴더 xdpdocuments에는 기본 템플릿과 **조각** 폴더

를 사용하여 필요한 구조를 만들 수 있습니다 [양식 및 문서 ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

다음은 2개의 조각을 사용하는 샘플 xdp의 폴더 구조입니다
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* 출력 서비스 - 일반적으로 이 서비스는 xml 데이터를 xdp 템플릿 또는 pdf와 병합하여 병합된 pdf를 생성하는 데 사용됩니다. 자세한 내용은 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 출력 서비스에 대해 사용됩니다. 이 샘플에서는 crx 저장소에 있는 조각을 사용합니다.


다음 ECMA 스크립트가 PDF 생성 스크립트를 사용했습니다. 코드에서 ResourceResolver 및 ResourceResolverHelper를 사용하는 것을 확인합니다. 이 코드가 사용자 컨텍스트 외부에서 실행되므로 ResourceReolver가 필요합니다.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**시스템에서 샘플 패키지를 테스트하려면**
* [DevelopingWithServiceUSer 번들 배포](assets/DevelopingWithServiceUser.jar)
* 항목 추가 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 아래 스크린샷에 나와 있는 대로 user mapper 서비스 개정판에서
   ![사용자 매퍼 수정](assets/user-mapper-service-amendment.png)
* [샘플 xdp 파일 및 ECMA 스크립트를 다운로드하여 가져옵니다](assets/watched-folder-fragments-ecma.zip).
이렇게 하면 c:/fragmentsandoutputservice 폴더에 감시 폴더 구조가 만들어집니다

* [샘플 데이터 파일 추출](assets/usingFragmentsSampleData.zip) 감시 폴더의 설치 폴더(c:\fragmentsandoutputservice\install)에 배치합니다.

* 감시 폴더 구성의 결과 폴더에서 생성된 pdf 파일을 확인합니다