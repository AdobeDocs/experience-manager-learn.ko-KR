---
title: 적응형 양식 데이터를 사용하여 대화형 DoR 생성
description: 적응형 양식 데이터를 XDP 템플릿과 병합하여 다운로드 가능한 PDF 생성
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# 인터랙티브 DoR 다운로드

일반적인 사용 사례는 적응형 양식 데이터가 있는 대화형 DoR을 다운로드할 수 있는 것입니다. 그런 다음 다운로드한 DoR이 Adobe Acrobat 또는 Adobe Reader을 사용하여 완료됩니다.

## 적응형 양식이 XSD 스키마를 기반으로 하지 않음

XDP 및 적응형 양식이 스키마를 기반으로 하지 않는 경우 다음 단계에 따라 대화형 기록 문서를 생성하십시오.

### 적응형 양식 만들기

적응형 양식을 만들고 적응형 양식 필드 이름이 xdp 템플릿의 필드 이름과 동일한지 확인합니다.
xdp 템플릿의 루트 요소 이름을 기록합니다.
![root-element](assets/xfa-root-element.png)

### 클라이언트 라이브러리

다음 코드는 PDF 다운로드 단추가 트리거될 때 트리거됩니다

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## XSD 스키마 기반 적응형 양식

xdp가 XSD를 기반으로 하지 않는 경우 다음 단계에 따라 적응형 양식의 기반이 되는 XSD(스키마)를 생성하십시오

### XDP에 대한 샘플 데이터 생성

* AEM Forms 디자이너에서 XDP를 엽니다.
* 파일 클릭 | 양식 속성 | 미리 보기
* 미리 보기 데이터 생성 을 클릭합니다
* Generate 클릭
* &quot;form-data.xml&quot;과 같은 의미 있는 파일 이름 제공

### xml 데이터에서 XSD 생성

무료 온라인 도구를 사용하여 이전 단계에서 생성된 xml 데이터에서 [XSD를 생성](https://www.freeformatter.com/xsd-generator.html)할 수 있습니다.

### 적응형 양식 만들기

이전 단계의 XSD를 기반으로 적응형 양식을 만듭니다. 클라이언트 라이브러리 &quot;irs&quot;를 사용하도록 양식을 연결하십시오. 이 클라이언트 라이브러리에는 호출 응용 프로그램에 PDF을 반환하는 서블릿에 대한 POST 호출을 수행하는 코드가 있습니다
_PDF 다운로드_&#x200B;를 클릭하면 다음 코드가 트리거됩니다

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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



## 사용자 정의 서블릿 만들기

데이터를 XDP 템플릿과 병합하고 pdf를 반환하는 사용자 지정 서블릿을 만듭니다. 이를 수행하기 위한 코드가 아래에 나와 있습니다. 사용자 지정 서블릿은 [AEMFormsDocumentServices.core-1.0-SNAPSHOT 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar))의 일부입니다.

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

샘플 코드에서는 요청 개체에서 xdp 이름 및 기타 매개 변수를 추출합니다. 양식이 XSD를 기반으로 하지 않는 경우 xdp와 병합할 xml 문서가 만들어집니다. 양식이 XSD를 기반으로 하는 경우 적응형 양식 제출 데이터에서 적절한 노드를 추출하여 xdp 템플릿과 병합할 xml 문서를 생성하면 됩니다.

## 서버에 샘플 배포

로컬 서버에서 테스트하려면 다음 단계를 따르십시오.

1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Apache Sling Service User Mapper 서비스에 다음 항목 추가
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [사용자 지정 DocumentServices 번들을 다운로드하여 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)합니다. 데이터를 XDP 템플릿과 병합하고 pdf를 다시 스트리밍하는 서블릿이 있습니다.
1. [클라이언트 라이브러리 가져오기](assets/generate-interactive-dor-client-lib.zip)
1. [문서 Assets 가져오기(적응형 양식, XDP 템플릿 및 XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 양식 필드 몇 개를 입력합니다.
1. PDF 다운로드 를 클릭하여 PDF을 가져옵니다. PDF이 다운로드될 때까지 몇 초 동안 기다려야 할 수 있습니다.

>[!NOTE]
>
>[비xsd 기반 적응형 양식](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled)에서 동일한 사용 사례를 시도할 수 있습니다. irs clientlib에 있는 streampdf.js의 post 끝점에 적절한 매개 변수를 전달해야 합니다.
