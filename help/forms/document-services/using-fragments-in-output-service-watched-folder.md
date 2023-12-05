---
title: 감시 폴더로 출력 서비스에서 조각 사용
description: crx 저장소에 있는 조각으로 PDF 문서 생성
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 120
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# ECMA 스크립트를 사용하여 조각이 있는 PDF 문서 생성{#developing-with-output-and-forms-services-in-aem-forms}


이 문서에서는 출력 서비스를 사용하여 xdp 조각을 사용하여 pdf 파일을 생성합니다. 기본 xdp와 조각은 crx 저장소에 있습니다. AEM의 파일 시스템 폴더 구조를 모방하는 것이 중요합니다. 예를 들어 xdp의 조각 폴더에서 조각을 사용 중인 경우 라는 폴더를 생성해야 합니다. **조각** AEM의 기본 폴더 아래에 있습니다. 기본 폴더에는 기본 xdp 템플릿이 포함됩니다. 예를 들어 파일 시스템에 다음 구조가 있는 경우
* c:\xdptemplates - 여기에 기본 xdp 템플릿이 포함됩니다.
* c:\xdptemplates\fragments - 이 폴더에는 조각이 포함되며 기본 템플릿은 아래와 같이 조각을 참조합니다.
  ![fragment-xdp](assets/survey-fragment.png).
* 폴더 xdpdocuments에는 기본 템플릿과 조각이 포함됩니다. **조각** 폴더

다음을 사용하여 필요한 구조를 만들 수 있습니다. [양식 및 문서 ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

다음은 2개의 조각을 사용하는 샘플 xdp의 폴더 구조입니다
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* 출력 서비스 - 일반적으로 이 서비스는 xml 데이터를 xdp 템플릿 또는 pdf와 병합하여 병합된 pdf를 생성하는 데 사용됩니다. 자세한 내용은 다음을 참조하십시오 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 출력 서비스용입니다. 이 샘플에서는 crx 저장소에 있는 조각을 사용하고 있습니다.


다음 ECMA 스크립트가 PDF 생성에 사용되었습니다. 코드에서 ResourceResolver 및 ResourceResolverHelper를 사용합니다. ResourceResolver가 필요한 이유는 이 코드가 사용자 컨텍스트 외부에서 실행되기 때문입니다.

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
* 항목 추가 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 아래 스크린샷에 표시된 대로 user mapper 서비스 수정
  ![사용자 매퍼 수정](assets/user-mapper-service-amendment.png)
* [샘플 xdp 파일 및 ECMA 스크립트를 다운로드하여 가져옵니다.](assets/watched-folder-fragments-ecma.zip).
이렇게 하면 c:/fragmentsandoutputservice 폴더에 감시 폴더 구조가 생성됩니다

* [샘플 데이터 파일 추출](assets/usingFragmentsSampleData.zip) 감시 폴더의 설치 폴더에 넣습니다(c:\fragmentsandoutputservice\install).

* 생성된 PDF 파일에 대한 감시 폴더 구성의 결과 폴더를 확인합니다
