---
title: 사용 권한으로 XDP를 PDF에 렌더링
description: PDF에 사용 권한 적용
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
duration: 150
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# 사용 권한으로 XDP를 PDF에 렌더링{#rendering-xdp-into-pdf-with-usage-rights}

일반적인 사용 사례는 xdp를 PDF으로 렌더링하고 Reader 확장을 렌더링된 PDF에 적용하는 것입니다.

예를 들어 AEM Forms의 Forms 포털에서 사용자가 XDP를 클릭하면 XDP를 PDF으로 렌더링하고 판독기에서 PDF을 확장할 수 있습니다.


이 사용 사례를 달성하려면 다음을 수행해야 합니다.

* Reader 확장 인증서를 &quot;fd-service&quot; 사용자에게 추가합니다. Reader 확장 자격 증명을 추가하는 단계는 [여기](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=ko)에 나열됩니다.


* [Reader 확장 자격 증명 구성](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=ko)에 대한 비디오를 참조할 수도 있습니다.


* 사용 권한을 렌더링하고 적용하는 사용자 지정 OSGi 서비스를 만듭니다. 이를 수행하기 위한 코드가 아래에 나와 있습니다

## XDP 렌더링 및 사용 권한 적용 {#render-xdp-and-apply-usage-rights}

* 7행: FormsService의 renderPDFForm을 사용하여 XDP에서 PDF을 생성합니다.

* 8-14행: 적절한 사용 권한이 설정됩니다. 이러한 사용 권한은 OSGi 구성 설정에서 가져옵니다.

* 20행 : 서비스 사용자 fd-service와 연결된 resourceresolver 사용

* 24행: 사용 권한을 적용하려면 DocumentAssuranceService의 secureDocument 메서드를 사용합니다.

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

다음 스크린샷은 구성 속성이 노출된 것을 보여 줍니다. 대부분의 공통 사용 권한은 이 구성을 통해 노출됩니다.

![구성 속성](assets/configurationproperties.gif)

다음 코드는 OSGi 구성 설정을 작성하는 데 사용되는 코드를 보여 줍니다

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## PDF을 스트리밍할 서블릿 만들기 {#create-servlet-to-stream-the-pdf}

다음 단계는 GET 메서드를 사용하여 서블릿을 만들어 사용자에게 PDF 확장 판독기를 반환하는 것입니다. 이 경우 사용자는 PDF을 파일 시스템에 저장하라는 메시지가 표시됩니다. 이는 PDF이 동적 PDF으로 렌더링되고 브라우저와 함께 제공되는 pdf 뷰어가 동적 pdf를 처리하지 않기 때문입니다.

다음은 서블릿에 대한 코드입니다. CRX 저장소의 XDP 경로를 이 서블릿에 전달합니다.

그런 다음 com.aemformssamples.documentservices.core.DocumentServices의 renderAndExtendXdp 메서드를 호출합니다.

그러면 리더가 PDF을 확장하여 호출 애플리케이션으로 스트리밍합니다

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

로컬 서버에서 테스트하려면 다음 단계를 따르십시오
1. [DevelopingWithServiceUser 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [AEMFormsDocumentServices 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [사용자 지정 포털 템플릿 html 다운로드](assets/render-and-extend-template.zip)
1. [패키지 관리자를 사용하여 이 문서와 관련된 에셋을 AEM에 다운로드하고 가져옵니다.](assets/renderandextendxdp.zip)
   * 이 패키지에는 샘플 포털 및 xdp 파일이 있습니다.
1. &quot;fd-service&quot; 사용자에게 Reader 확장 인증서 추가
1. 브라우저를 [포털 웹 페이지](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)&#x200B;(으)로 지정
1. xdp를 사용 권한이 적용된 pdf 파일로 렌더링하려면 pdf 아이콘을 클릭합니다.
