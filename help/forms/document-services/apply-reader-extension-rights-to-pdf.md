---
title: 사용 권한을 사용하여 XDP를 PDF로 렌더링
seo-title: 사용 권한을 사용하여 XDP를 PDF로 렌더링
description: pdf에 사용 권한 적용
seo-description: pdf에 사용 권한 적용
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms 서비스
topic: 개발
role: Developer
level: Experienced
source-git-commit: aa90b2c1a066dc36d4ba26ecdb8b58939445ef34
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---


# Reader 확장 적용

Reader 확장을 사용하면 PDF 문서에 대한 사용 권한을 조작할 수 있습니다. 사용 권한은 Acrobat에서 사용할 수 있지만 Adobe Reader에서는 사용할 수 없는 기능과 관련이 있습니다. Reader 확장에서 제어하는 기능에는 문서에 주석을 추가하고, 양식을 작성하고, 문서를 저장하는 기능이 포함되어 있습니다. 사용 권한이 추가된 PDF 문서를 권한 사용 문서라고 합니다. Adobe Reader에서 권한이 활성화된 PDF 문서를 여는 사용자는 해당 문서에 대해 활성화된 작업을 수행할 수 있습니다.
이 기능을 테스트하기 위해 이 [링크](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html)를 시도할 수 있습니다.

이 사용 사례를 수행하려면 다음을 수행해야 합니다.
* [사용자에게 Reader 확장 ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) 인증서를  `fd-service` 추가합니다.

* 문서에 사용 권한을 적용할 사용자 지정 OSGi 서비스를 만듭니다. 이를 수행할 코드는 다음과 같습니다

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## PDF를 스트리밍할 서블릿 만들기 {#create-servlet-to-stream-the-pdf}

다음 단계는 Reader 확장 PDF를 사용자에게 반환할 POST 방법이 있는 서블릿을 만드는 것입니다. 이 경우 사용자에게 PDF를 파일 시스템에 저장하라는 메시지가 표시됩니다. PDF가 동적 PDF로 렌더링되고 브라우저와 함께 제공되는 pdf 뷰어가 동적 pdf를 처리하지 않기 때문입니다.

다음은 서블릿에 대한 코드입니다. 서블릿은 적응형 양식의 **customersubmit** 작업에서 호출됩니다.
서블릿은 UsageRights 개체를 만들고 적응형 양식에 사용자가 입력한 값을 기반으로 속성을 설정합니다. 그러면 서블릿은 이 목적으로 만들어진 서비스의 **applyUsageRights** 메서드를 호출합니다.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;

import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];
                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);

                                        documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        documentToReaderExtend.close();
                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();
                                        documentToReaderExtend.close();
                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        int bytes;
                                        while ((bytes = fileInputStream.read()) != -1) {
                                                responseOutputStream.write(bytes);
                                        }
                                        responseOutputStream.flush();
                                        responseOutputStream.close();

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

로컬 서버에서 테스트하려면 다음 단계를 수행하십시오.
1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [ares.core-ares 번들을 다운로드하여 설치합니다](assets/ares.ares.core-ares.jar). 사용 권한을 적용하고 pdf를 다시 스트리밍할 사용자 지정 서비스 및 서블릿이 있습니다
1. [클라이언트 라이브러리 및 사용자 지정 제출 가져오기](assets/applyaresdemo.zip)
1. [적응형 양식 가져오기](assets/applyaresform.zip)
1. &quot;fd-service&quot; 사용자에게 Reader 확장 인증서 추가
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. 적절한 권한을 선택하고 PDF 파일을 업로드합니다
1. 제출 을 클릭하여 Reader 확장 PDF를 가져옵니다


