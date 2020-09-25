---
title: AEM Forms에서 PDF 사용
seo-title: AEM Forms에서 PDF 사용
description: AEM Forms을 사용하여 PDF 작성을 위한 드래그 앤 드롭 기능 시연
seo-description: AEM Forms을 사용하여 PDF 작성을 위한 드래그 앤 드롭 기능 시연
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# AEM Forms에서 PDF 사용{#using-pdfg-in-aem-forms}

AEM Forms을 사용하여 PDF 작성을 위한 드래그 앤 드롭 기능 시연

PDF는 PDF 생성을 의미합니다. 즉, 다양한 파일 포맷을 PDF로 변환할 수 있습니다. 가장 일반적인 것은 Microsoft Office 문서입니다. PDFG는 6.1 이후 AEM Forms의 일부입니다.[PDFG API용 javadoc는 여기에 나와 있습니다.](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

이 문서와 연결된 에셋을 사용하면 MS Office 문서 또는 JPG 파일을 HTML 페이지의 드롭 영역에 드래그하여 놓을 수 있습니다. 문서가 삭제되면 PDF 서비스를 불러온 다음 문서를 PDF로 변환하여 AEM Server의 파일 시스템에 저장합니다.

데모 에셋을 설치하려면 다음 단계를 수행하십시오

1. 이 문서에 언급된 대로 PDFG를 [구성합니다](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. AEM Forms 버전과 관련된 해당 문서를 따르십시오.
1. [패키지 관리자를 사용하여 이 아티클과 관련된 에셋을 가져오고 설치합니다.](assets/createpdfgdemov2.zip)
1. [CRX에서 post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 로 이동합니다.
1. 기본 설정에 따라 저장 위치 변경(9줄)
1. 변경 내용을 저장합니다.
1. 변환을 위해 파일을 드래그 앤 드롭할 [ html 페이지를](http://localhost:4502/content/DocumentServices/CreatePDFG.html) 엽니다.
1. 드롭 영역에 단어 파일이나 jpg를 놓습니다.
1. 입력 문서가 PDF로 변환되어 4번 포인트와 동일한 위치에 저장됩니다.

다음 코드 조각은 파일을 PDF로 변환하는 PDF 서비스의 사용을 보여줍니다

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

