---
title: AEM Forms에서 PDFG 사용
description: AEM Forms을 사용하여 PDF을 만들기 위한 드래그 앤 드롭 기능 시연
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 63
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# AEM Forms에서 PDFG 사용{#using-pdfg-in-aem-forms}

AEM Forms을 사용하여 PDF을 만들기 위한 드래그 앤 드롭 기능 시연

PDFG는 PDF 생성을 나타냅니다. 즉, 다양한 파일 형식을 PDF 형식으로 변환할 수 있습니다. 가장 일반적인 문서는 Microsoft Office 문서입니다. PDFG는 6.1 이후 AEM Forms의 일부였습니다.
[PDFG API용 javadoc이 여기에 나열됩니다](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

이 문서와 연결된 에셋을 사용하면 MS Office 문서 또는 JPG 파일을 HTML 페이지의 드롭 영역으로 드래그 앤 드롭할 수 있습니다. 문서가 삭제되면 PDFG 서비스를 호출하고 문서를 PDF으로 변환하여 AEM 서버의 파일 시스템에 저장합니다.

데모 자산을 설치하려면 다음 단계를 수행하십시오

1. 이 문서에서 언급한 대로 PDFG 구성 [여기](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. AEM Forms 버전과 관련된 적절한 설명서를 따르십시오.
1. [패키지 관리자를 사용하여 이 문서와 관련된 에셋을 가져오고 설치합니다.](assets/createpdfgdemov2.zip)
1. [post.jsp로 이동](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) CRX에서
1. 원하는 대로 저장 위치 변경(9행)
1. 변경 사항을 저장합니다.
1. 를 엽니다. [html 페이지](http://localhost:4502/content/DocumentServices/CreatePDFG.html) 변환을 위해 파일을 드래그 앤 드롭하는 데 사용됩니다.
1. Word 파일 또는 jpg를 드롭 영역에 드롭합니다.
1. 입력된 문서는 PDF으로 변환되고 포인트 4에 지정된 것과 동일한 위치에 저장됩니다.

다음 코드 조각은 파일을 PDF으로 변환하는 PDFG 서비스 사용을 보여줍니다

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
