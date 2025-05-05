---
title: 기록 문서 인라인 표시
description: 적응형 양식 데이터를 XDP 템플릿과 병합하고 document cloud embed pdf API를 사용하여 PDF 인라인을 표시합니다.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# 인라인 DoR 표시

일반적인 사용 사례는 양식 작성기에서 입력한 데이터가 포함된 pdf 문서를 표시하는 것입니다.

이 사용 사례를 수행하기 위해 [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)를 활용했습니다.

통합을 완료하기 위해 다음 단계를 수행했습니다

## PDF 인라인을 표시할 사용자 지정 구성 요소 만들기

POST 호출에서 반환된 pdf를 임베드하기 위해 사용자 지정 구성 요소(embed-pdf)가 생성되었습니다.

## 클라이언트 라이브러리

`viewPDF` 확인란 단추를 클릭하면 다음 코드가 실행됩니다. 적응형 양식 데이터, 템플릿 이름을 끝점에 전달하여 PDF를 생성합니다. 그런 다음 임베드 pdf JavaScript 라이브러리를 사용하여 생성된 pdf가 양식 작성기에 표시됩니다.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## XDP에 대한 샘플 데이터 생성

* AEM Forms 디자이너에서 XDP를 엽니다.
* 파일 클릭 | 양식 속성 | 미리 보기
* 미리 보기 데이터 생성 을 클릭합니다
* Generate 클릭
* &quot;form-data.xml&quot;과 같은 의미 있는 파일 이름 제공

## xml 데이터에서 XSD 생성

무료 온라인 도구를 사용하여 이전 단계에서 생성된 xml 데이터에서 [XSD를 생성](https://www.freeformatter.com/xsd-generator.html)할 수 있습니다.

## 템플릿 업로드

만들기 단추를 사용하여 xdp 템플릿을 [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에 업로드해야 합니다.


## 적응형 양식 만들기

이전 단계의 XSD를 기반으로 적응형 양식을 만듭니다.
새 탭을 적응형 양식에 추가합니다. 이 탭에 확인란 구성 요소 및 embed-pdf 구성 요소 추가
확인란 이름을 viewPDF로 지정해야 합니다.
아래 스크린샷과 같이 embed-pdf 구성 요소를 구성합니다
![embed-pdf](assets/embed-pdf-configuration.png)

**PDF API 키 포함** - pdf를 포함하는 데 사용할 수 있는 키입니다. 이 키는 localhost에서만 작동합니다. [자신의 키](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)를 만들어 다른 도메인과 연결할 수 있습니다.

**PDF를 반환하는 끝점** - 데이터를 xdp 템플릿과 병합하고 PDF를 반환하는 사용자 지정 서블릿입니다.

**템플릿 이름** - xdp 경로입니다. 일반적으로 formsanddocuments 폴더에 저장됩니다.

**PDF 파일 이름** - 포함 pdf 구성 요소에 표시되는 문자열입니다.

## 사용자 정의 서블릿 만들기

데이터를 XDP 템플릿과 병합하고 pdf를 반환하는 사용자 지정 서블릿이 생성되었습니다. 이를 수행하기 위한 코드가 아래에 나와 있습니다. 사용자 지정 서블릿은 [embedpdf 번들](assets/embedpdf.core-1.0-SNAPSHOT.jar)의 일부입니다.

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
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

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## 서버에 샘플 배포

로컬 서버에서 테스트하려면 다음 단계를 따르십시오.

1. [embedpdf 번들을 다운로드하여 설치](assets/embedpdf.core-1.0-SNAPSHOT.jar)합니다.
이 템플릿에는 데이터를 XDP 템플릿과 병합하고 pdf를 다시 스트리밍하는 서블릿이 있습니다.
1. [AEM ConfigMgr](http://localhost:4502/system/console/configMgr)을 사용하여 Adobe Granite CSRF 필터의 제외된 경로 섹션에 /bin/getPDFToEmbed 경로를 추가합니다. 프로덕션 환경에서는 [CSRF 보호 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=ko)를 사용하는 것이 좋습니다
1. [클라이언트 라이브러리 및 사용자 지정 구성 요소 가져오기](assets/embed-pdf.zip)
1. [적응형 양식 및 템플릿 가져오기](assets/embed-pdf-form-and-xdp.zip)
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. 양식 필드 몇 개 입력
1. 탭으로 이동하여 PDF 보기를 표시합니다. pdf 보기 확인란을 선택합니다. 적응형 양식 데이터로 채워진 양식에 pdf가 표시됩니다
