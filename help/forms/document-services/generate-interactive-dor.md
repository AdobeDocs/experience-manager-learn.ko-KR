---
title: 적응형 양식 데이터를 사용하여 대화형 DoR 생성
description: 적응형 양식 데이터를 XDP 템플릿에 병합하여 다운로드 가능한 pdf를 생성합니다
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: 2ed78bb8b122acbe69e98d63caee1115615d568f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---


# 대화형 DoR 다운로드

일반적인 사용 사례는 적응형 양식 데이터를 사용하여 대화형 DoR을 다운로드할 수 있는 것입니다. 다운로드한 DoR은 Adobe Acrobat 또는 Adobe Reader을 사용하여 완료됩니다.

이 사용 사례를 수행하려면 다음을 수행해야 합니다

## XDP용 샘플 데이터 생성

* AEM Forms 디자이너에서 XDP를 엽니다.
* 파일 클릭 | 양식 속성 | 미리 보기
* 미리 보기 데이터 생성을 클릭합니다
* 생성을 누릅니다
* &quot;form-data.xml&quot;과 같은 의미 있는 파일 이름을 제공합니다

## xml 데이터에서 XSD 생성

무료 온라인 도구를 사용하여 [xsd 생성](https://www.freeformatter.com/xsd-generator.html) 이전 단계에서 생성된 xml 데이터에서 생성합니다.

## 적응형 양식 만들기

이전 단계의 XSD를 기반으로 적응형 양식을 만듭니다. 클라이언트 라이브러리 &quot;irs&quot;를 사용할 양식을 연결합니다. 이 클라이언트 라이브러리에는 호출 애플리케이션에 PDF을 반환하는 서블릿에 대한 POST 호출을 수행하는 코드가 있습니다. 다음 코드는 _다운로드 PDF_ 를 클릭합니다.

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## 사용자 지정 서블릿 만들기

데이터를 XDP 템플릿과 병합하고 pdf를 반환하는 사용자 지정 서블릿을 만듭니다. 이를 수행할 코드는 아래에 나와 있습니다. 사용자 지정 서블릿은 [AEMFormsDocumentServices.core-1.0-SNAPSHOT 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

샘플 코드에서 템플릿 이름(f8918-r14e_redo-barcode_3 2.xdp)은 하드 코딩됩니다. 템플릿 이름을 서블릿에 쉽게 전달하여 이 코드가 모든 템플릿에 대해 작동하도록 만들 수 있습니다.


## 서버에 샘플 배포

로컬 서버에서 테스트하려면 다음 단계를 수행하십시오.

1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Apache Sling Service User Mapper Service DevelopingWithServiceUser.core:getformsresourceresresolver=fd-service에 다음 항목을 추가합니다.
1. [사용자 지정 DocumentServices 번들을 다운로드하여 설치합니다](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). 여기에 데이터를 XDP 템플릿과 병합하고 pdf를 다시 스트리밍할 서블릿이 있습니다
1. [클라이언트 라이브러리 가져오기](assets/irs.zip)
1. [적응형 양식 가져오기](assets/f8918complete.zip)
1. [XDP 템플릿 및 스키마 가져오기](assets/xdp-template-and-xsd.zip)
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 양식 필드 몇 개 채우기
1. PDF 다운로드 을 클릭하여 PDF을 가져옵니다
