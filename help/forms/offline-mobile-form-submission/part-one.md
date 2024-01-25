---
title: HTM5 양식 제출에서 AEM 워크플로우 트리거 - 사용자 정의 프로필 만들기
description: 오프라인 모드에서 모바일 양식을 계속 채우고 AEM 워크플로우를 트리거하기 위한 모바일 양식을 제출합니다.
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 101
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 사용자 지정 프로필 만들기

이 부분에서는 [사용자 지정 프로필.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 프로필은 XDP를 HTML으로 렌더링합니다. XDP를 HTML으로 렌더링하기 위해 기본 프로필이 즉시 제공됩니다. 이는 Mobile Forms 렌디션 서비스의 사용자 지정 버전을 나타냅니다. Mobile Form Rendition Service를 사용하여 Mobile Forms의 모양, 동작 및 상호 작용을 사용자 지정할 수 있습니다. 사용자 지정 프로필에서는 guidebridge API를 사용하여 모바일 양식에 입력된 데이터를 캡처합니다. 그런 다음 이 데이터는 사용자 지정 서블릿으로 전송되고, 사용자 지정 서블릿은 대화형 PDF을 생성하고 호출 애플리케이션으로 다시 스트리밍합니다.

를 사용하여 양식 데이터 가져오기 `formBridge` JavaScript API입니다. 다음을 사용합니다. `getDataXML()` 방법:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

성공 핸들러 메서드에서 AEM에서 실행 중인 사용자 정의 서블릿을 호출합니다. 이 서블릿은 모바일 양식의 데이터를 사용하여 대화형 pdf를 렌더링하고 반환합니다

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## 대화형 PDF 생성

다음은 대화형 pdf를 렌더링하고 pdf를 호출 애플리케이션으로 반환하는 서블릿 코드입니다. 서블릿이 호출합니다 `mobileFormToInteractivePdf` 사용자 지정 DocumentServices OSGi 서비스의 메서드입니다.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
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
        // TODO Add proper error logging
      }
    }
}
```

### 대화형 PDF 렌더링

다음 코드에서는 [Forms 서비스 API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) 모바일 양식의 데이터로 대화형 PDF을 렌더링합니다.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

부분적으로 완료된 모바일 양식에서 대화형 PDF을 다운로드하는 기능을 보려면 [여기를 클릭하십시오.](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
PDF이 다운로드되면 다음 단계는 AEM 워크플로를 트리거하기 위해 PDF을 제출하는 것입니다. 이 워크플로우는 제출된 PDF의 데이터를 병합하고 검토를 위해 비대화형 PDF을 생성합니다.

이 사용 사례용으로 만들어진 사용자 지정 프로필은 이 자습서 자산의 일부로 사용할 수 있습니다.
