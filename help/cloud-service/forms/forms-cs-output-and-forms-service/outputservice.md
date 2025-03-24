---
title: 출력 서비스를 사용하여 PDF 문서 생성
description: 데이터를 XDP 템플릿과 병합하여 비대화형 PDF 생성
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---

# 출력 서비스를 사용하여 PDF 문서 생성

[출력 서비스](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html)는 AEM 문서 서비스의 일부인 OSGi 서비스입니다. AEM Forms Designer의 다양한 출력 형식 및 디자인 기능을 지원합니다. 출력 서비스는 XFA 템플릿과 XML 데이터를 변환하여 다른 형식의 인쇄 문서를 생성합니다.

AEM Forms as a Cloud Service의 출력 서비스는 AEM Forms 6.5의 출력 서비스와 매우 유사하므로, AEM Forms 6.5의 출력 서비스 사용에 익숙한 경우 AEM Forms as a Cloud Service으로 전환하는 것은 간단해야 합니다.

Output 서비스를 사용하여 다음과 같은 작업을 수행할 수 있는 응용 프로그램을 만들 수 있습니다.

+ XML 데이터로 템플릿 파일을 채워 최종 양식 문서를 생성합니다.
+ 비대화형 PDF, PostScript, PCL 및 ZPL 인쇄 스트림을 포함하여 다양한 형식의 출력 양식을 생성합니다.
+ XFA 양식 PDF에서 인쇄 PDF를 생성합니다.
+ 제공된 템플릿과 여러 데이터 세트를 병합하여 PDF, PostScript, PCL 및 ZPL 문서를 대량으로 생성합니다.

이 서비스는 AEM Forms as a Cloud Service 인스턴스의 컨텍스트 내에서 사용할 수 있도록 설계되었습니다. 다음 코드 조각은 `OutputService`을(를) 사용하여 서블릿에 PDF 문서를 생성합니다.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
