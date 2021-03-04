---
title: 사용 권한으로 XDP를 PDF로 렌더링
seo-title: 사용 권한으로 XDP를 PDF로 렌더링
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
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 1%

---


# Reader 확장 적용

Reader 확장을 사용하면 PDF 문서의 사용 권한을 조작할 수 있습니다. 사용 권한은 Acrobat에서 사용할 수 있지만 Adobe Reader에서는 사용할 수 없는 기능과 관련이 있습니다. Reader 익스텐션에서 제어하는 기능에는 문서에 주석을 추가하고 양식을 작성하며 문서를 저장하는 기능이 포함되어 있습니다. 사용 권한이 추가된 PDF 문서를 권한 사용 문서라고 합니다. Adobe Reader에서 권한이 활성화된 PDF 문서를 여는 사용자는 해당 문서에 대해 활성화된 작업을 수행할 수 있습니다.
이 기능을 테스트하려면 이 [link](https://forms.enablementadobe.com/content/samples/samples.html?query=0)을 사용해 볼 수 있습니다. 샘플 이름은 &quot;RE를 사용하여 XDP 렌더링&quot;입니다.

이 사용 사례를 수행하려면 다음을 수행해야 합니다.
* &quot;fd-service&quot; 사용자에게 Reader 확장 인증서를 추가합니다. Reader 확장 자격 증명을 추가하는 단계는 [여기](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)에 나열되어 있습니다.

* 문서에 사용 권한을 적용할 사용자 정의 OSGi 서비스를 만듭니다. 이 작업을 수행할 코드는 아래에 나열되어 있습니다

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

## PDF {#create-servlet-to-stream-the-pdf}을(를) 스트리밍할 서블릿 만들기

다음 단계는 POST 방법으로 서블릿을 만들어 독자의 확장 PDF를 사용자에게 반환하는 것입니다. 이 경우 PDF를 파일 시스템에 저장하라는 메시지가 표시됩니다. 이는 PDF가 동적 PDF로 렌더링되고 브라우저와 함께 제공되는 PDF 뷰어가 동적 PDF를 처리하지 않기 때문입니다.

다음은 서블릿의 코드입니다. 서블릿은 적응형 양식의 **customsubmit** 작업에서 호출됩니다.
서블릿은 UsageRights 객체를 만들고 응용 양식에 사용자가 입력한 값을 기반으로 해당 속성을 설정합니다. 그런 다음 서블릿은 이 목적으로 만들어진 서비스의 **applyUsageRights** 메서드를 호출합니다.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Enumeration;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFormatException;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = Servlet.class, property = {
sling.servlet.methods=get", "sling.servlet.paths=/bin/applyrights"
})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {
  @Reference
  GetResolver getResolver;
  @Reference
  ApplyUsageRights applyRights;
  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
  try {
        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
        InputSource is = new InputSource();
        is.setCharacterStream(new StringReader(xmlString));
  return db.parse(is);
} catch (ParserConfigurationException e) {
    e.printStackTrace();
} catch (SAXException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
return null;
}
@Override
protected void doGet(SlingHttpServletRequest request,SlingHttpServletResponse response)
{
  doPost(request,response);
}
@Override
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  UsageRights usageRights = new UsageRights();
  String submittedData = request.getParameter("jcr:data");
  org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
  String formFill = submittedXml.getElementsByTagName("formfill").item(0).getTextContent();
  usageRights.setEnabledFormDataImportExport(true);
  usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
  usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
  usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
  usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
  String attachmentPath = submittedXml.getElementsByTagName("attachmentpath").item(0).getTextContent();
  Node pdfNode = getResolver.getFormsServiceResolver().getResource(attachmentPath).adaptTo(Node.class);
  javax.jcr.Node jcrContent = null;
try {
    jcrContent = pdfNode.getNode("jcr:content");
    } catch (RepositoryException e1) {
      e1.printStackTrace();
    }

try {
    InputStream is = jcrContent.getProperty("jcr:data").getBinary().getStream();
    Session jcrSession = getResolver.getFormsServiceResolver().adaptTo(Session.class);
    Document documentToReaderExtend = new Document(is);
    documentToReaderExtend =  applyRights.applyUsageRights(documentToReaderExtend,usageRights);
    documentToReaderExtend.copyToFile(new File("c:\\scrap\\doctore.pdf"));
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
  } catch (ValueFormatException e) {
      e.printStackTrace();
  } catch (PathNotFoundException e) {
      e.printStackTrace();
  } catch (RepositoryException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

}

}
```

로컬 서버에서 테스트하려면 다음 단계를 수행하십시오.
1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [ares.ares.core-ares 번들을 다운로드하고 설치합니다](assets/ares.ares.core-ares.jar). 이 서비스에는 사용 권한을 적용하고 PDF를 다시 스트리밍할 사용자 정의 서비스와 서블릿이 있습니다.
1. [클라이언트 라이브러리 및 사용자 정의 제출 가져오기](assets/applyaresdemo.zip)
1. [적응형 양식 가져오기](assets/applyaresform.zip)
1. &quot;fd-service&quot; 사용자에게 Reader 확장 인증서 추가
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. 적절한 권한을 선택하고 PDF 파일 업로드
1. [제출]을 클릭하여 Reader 확장 PDF를 가져옵니다.



