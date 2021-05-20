---
title: AEM Forms에서 PDFG 사용
seo-title: AEM Forms에서 PDFG 사용
description: AEM Forms을 사용하여 PDF를 만드는 드래그 앤 드롭 기능 데모
seo-description: AEM Forms을 사용하여 PDF를 만드는 드래그 앤 드롭 기능 데모
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf 생성기
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 2%

---


# AEM Forms에서 PDFG 사용{#using-pdfg-in-aem-forms}

AEM Forms을 사용하여 PDF를 만드는 드래그 앤 드롭 기능 데모

PDFG는 PDF 생성을 의미합니다. 즉, 다양한 파일 형식을 PDF로 변환할 수 있습니다. 가장 일반적인 것은 Microsoft Office 문서입니다. PDFG는 6.1 이후 AEM Forms의 일부가 되었습니다.
[PDFG API용 Javadoc는 여기에 나열되어 있습니다](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

이 문서와 연결된 자산을 사용하여 MS office 문서 또는 JPG 파일을 HTML 페이지의 드롭 영역으로 끌어다 놓을 수 있습니다. 문서가 삭제되면 PDFG 서비스를 호출하고 문서를 PDF로 변환하여 AEM Server의 파일 시스템에 저장합니다.

데모 자산을 설치하려면 다음 단계를 수행하십시오

1. 이 문서 [여기](https://helpx.adobe.com/kr/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)에 언급된 대로 PDFG를 구성합니다.
1. AEM Forms 버전과 관련된 적절한 설명서를 따르십시오.
1. [패키지 관리자를 사용하여 이 문서와 관련된 자산을 가져오고 설치합니다.](assets/createpdfgdemov2.zip)
1. [CRX를 ](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) post.jspin으로 이동합니다.
1. 기본 설정에 따라 저장 위치 변경(9행)
1. 변경 사항을 저장합니다.
1. 변환할 파일을 드래그 앤 드롭할 [ html 페이지](http://localhost:4502/content/DocumentServices/CreatePDFG.html)를 엽니다.
1. 단어 파일 또는 jpg를 드롭 영역에 놓습니다.
1. 입력 문서는 PDF로 변환되고 4번 지점에 지정된 것과 동일한 위치에 저장됩니다.

다음 코드 조각은 PDFG 서비스를 사용하여 파일을 PDF로 변환하는 방법을 보여 줍니다

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

