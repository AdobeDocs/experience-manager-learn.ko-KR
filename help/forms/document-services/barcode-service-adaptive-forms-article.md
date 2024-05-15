---
title: 적응형 Forms이 포함된 바코드 서비스
description: 바코드 서비스를 사용하여 바코드를 디코딩하고 추출된 데이터에서 양식 필드를 채웁니다.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# 적응형 Forms이 포함된 바코드 서비스{#barcode-service-with-adaptive-forms}

이 문서에서는 Barcode Service를 사용하여 적응형 양식을 채우는 방법을 보여줍니다. 사용 사례는 다음과 같습니다.

1. 사용자가 바코드가 포함된 PDF을 적응형 양식 첨부 파일로 추가합니다.
1. 첨부 파일의 경로가 서블릿으로 전송됩니다.
1. 서블릿이 바코드를 디코딩하고 데이터를 JSON 형식으로 반환합니다.
1. 그런 다음 디코딩된 데이터를 사용하여 적응형 양식이 채워집니다

다음 코드는 바코드를 디코딩하고 디코딩된 값으로 JSON 개체를 채웁니다. 그런 다음 서블릿은 호출하는 애플리케이션에 대한 응답으로 JSON 개체를 반환합니다.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

다음은 서블릿 코드입니다. 이 서블릿은 사용자가 적응형 양식에 첨부 파일을 추가할 때 호출됩니다. 서블릿은 JSON 개체를 호출 응용 프로그램으로 되돌립니다. 그런 다음 호출 애플리케이션이 적응형 양식을 JSON 오브젝트에서 추출된 값으로 채웁니다.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

다음 코드는 적응형 양식에서 참조하는 클라이언트 라이브러리의 일부입니다. 사용자가 적응형 양식에 첨부 파일을 추가하면 이 코드가 트리거됩니다. 코드는 GET 매개 변수에서 전달된 첨부 파일의 경로를 사용하여 서블릿을 호출합니다. 그런 다음 서블릿 호출에서 수신된 데이터를 사용하여 적응형 양식을 채웁니다.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>이 패키지에 포함된 적응형 양식은 AEM Forms 6.4를 사용하여 빌드되었습니다. AEM Forms 6.3 환경에서 이 패키지를 사용하려면 AEM Form 6.3에서 적응형 양식을 만드십시오

12행 - 서비스 확인자를 가져오는 사용자 지정 코드. 이 번들은 이 문서 에셋의 일부로 포함됩니다.

23행 - DocumentServices extractBarCode 메서드를 호출하여 디코딩된 데이터로 채워진 JSON 개체를 가져옵니다.

시스템에서 이 작업을 실행하려면 다음 단계를 따르십시오.

1. [BarcodeService.zip 다운로드](assets/barcodeservice.zip) 패키지 관리자를 사용하여 AEM으로 가져오기
1. [사용자 지정 DocumentServices 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [샘플 PDF 양식 다운로드](assets/barcode.pdf)
1. 브라우저를 가리켜 [샘플 적응형 양식](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 제공된 샘플 PDF 업로드
1. 데이터로 채워진 양식이 표시됩니다
