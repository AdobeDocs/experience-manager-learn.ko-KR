---
title: 적응형 Forms을 사용한 바코드 서비스
seo-title: 적응형 Forms을 사용한 바코드 서비스
description: 바코드 서비스를 사용하여 바코드 코드를 디코딩하고 추출된 데이터의 양식 필드 채우기
seo-description: 바코드 서비스를 사용하여 바코드 코드를 디코딩하고 추출된 데이터의 양식 필드 채우기
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---


# 적응형 Forms 포함 바코드 서비스{#barcode-service-with-adaptive-forms}

이 문서에서는 바코드 서비스를 사용하여 적응형 양식을 채우는 방법을 설명합니다. 활용 사례는 다음과 같습니다.

1. 사용자가 적응형 양식의 첨부 파일로 바코드가 있는 PDF를 추가합니다.
1. 첨부 파일 경로가 서블릿으로 전송됩니다.
1. 이 서블릿은 바코드를 디코딩하고 데이터를 JSON 형식으로 반환합니다.
1. 그런 다음 디코딩된 데이터를 사용하여 응용 양식이 채워집니다

다음 코드는 바코드를 디코딩하고 JSON 개체를 디코딩된 값으로 채웁니다. 그런 다음 서블릿은 호출 응용 프로그램에 대한 응답으로 JSON 개체를 반환합니다.

이 기능을 라이브로 볼 수 있습니다. [샘플 포털](https://forms.enablementadobe.com/content/samples/samples.html?query=0)을 방문하여 바코드 서비스 데모 검색

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

다음은 서블릿 코드입니다. 이 서블릿은 사용자가 응용 양식에 첨부를 추가할 때 호출됩니다. 서블릿은 JSON 개체를 호출 응용 프로그램으로 다시 반환합니다. 그런 다음 호출 응용 프로그램이 응용 양식을 JSON 개체에서 추출한 값으로 채웁니다.

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

다음 코드는 응용 양식에 의해 참조되는 클라이언트 라이브러리의 일부입니다. 사용자가 응용 양식에 첨부 파일을 추가하면 이 코드가 트리거됩니다. 이 코드는 요청 매개 변수에서 전달된 첨부 파일의 경로를 사용하여 서블릿에 GET 호출을 만듭니다. 그런 다음 서블릿 호출에서 수신한 데이터는 적응형 양식을 채우는 데 사용됩니다.

```
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
>이 패키지에 포함된 응용 형식은 AEM Forms 6.4를 사용하여 빌드되었습니다. AEM Forms 6.3 환경에서 이 패키지를 사용하려면 AEM Form 6.3에서 응용 양식을 만드십시오

12행 - 서비스 확인자를 가져오기 위한 사용자 지정 코드입니다. 이 번들은 이 아티클 에셋의 일부로 포함되어 있습니다.

23행 - DocumentServices extractBarCode 메서드를 호출하여 디코딩된 데이터로 JSON 개체를 채웁니다

시스템에서 이 작업을 실행하려면 다음 단계를 수행하십시오

1. [패키지 관리자를 사용하여 BarcodeService.](assets/barcodeservice.zip) zip을 다운로드하고 AEM으로 가져오기
1. [사용자 지정 Document Services 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [샘플 PDF 양식 다운로드](assets/barcode.pdf)
1. 브라우저를 [샘플 적응형 양식](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)으로 가리킵니다.
1. 제공된 샘플 PDF 업로드
1. 데이터가 채워진 양식을 볼 수 있습니다

