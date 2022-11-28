---
title: 응용 Forms의 미리 채우기 서비스
description: 백엔드 데이터 소스에서 데이터를 가져와 적응형 양식을 미리 채웁니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 적응형 Forms에서 미리 채우기 서비스 사용

기존 데이터를 사용하여 적응형 양식의 필드를 미리 채울 수 있습니다. 사용자가 양식을 열면 해당 필드의 값이 미리 채워집니다. 적응형 양식 필드를 미리 채우는 방법에는 여러 가지가 있습니다. 이 문서에서는 AEM Forms 미리 채우기 서비스를 사용하여 적응형 양식 미리 채우기를 살펴봅니다.

적응형 양식을 미리 채우는 다양한 방법에 대해 자세히 알아보려면, [이 설명서를 따르십시오](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

미리 채우기 서비스를 사용하여 적응형 양식을 미리 채우려면 `com.adobe.forms.common.service.DataXMLProvider` 인터페이스. 메서드 `getDataXMLForDataRef` 적응형 양식에서 필드를 미리 채우기 위해 사용할 데이터를 작성하고 반환하는 논리를 갖습니다. 이 방법에서는 소스에서 데이터를 가져온 다음 데이터 문서의 입력 스트림을 반환할 수 있습니다. 다음 샘플 코드는 로그인한 사용자의 사용자 프로필 정보를 가져오고 적응형 양식에 의해 입력 스트림을 사용하도록 반환되는 XML 문서를 구성합니다.

아래의 코드 조각에는 DataXMLProvider 인터페이스를 구현하는 클래스가 있습니다. 로그인한 사용자에 대한 액세스 권한을 얻은 다음 로그인한 사용자의 프로필 정보를 가져옵니다. 그런 다음 &quot;data&quot;라는 루트 노드 요소를 사용하여 XML 문서를 만들고 이 데이터 노드에 적절한 요소를 추가합니다. XML 문서가 구성되면 XML 문서의 입력 스트림이 반환됩니다.

그런 다음 이 클래스는 OSGi 번들로 만들어져서 AEM에 배포됩니다. 번들을 배포하면 이 미리 채우기 서비스를 적응형 양식의 미리 채우기 서비스로 사용할 수 있습니다.

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

서버에서 이 기능을 테스트하려면 다음을 수행하십시오

* 로그인되어 있는지 확인하십시오. [사용자 프로필](http://localhost:4502/security/users.html) 정보가 입력되어 있습니다. 이 샘플은 로그인한 사용자의 FirstName, LastName 및 Email 속성을 찾습니다.
* [컴퓨터에 zip 파일의 내용을 다운로드하고 추출합니다](assets/prefillservice.zip)
* 를 사용하여 prefill.core-1.0.0-SNAPSHOT 번들 배포 [AEM 웹 콘솔](http://localhost:4502/system/console/bundles)
* 만들기 를 사용하여 적응형 양식 가져오기 | 파일에서 파일 업로드 [FormsAndDocuments 섹션](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 다음을 확인합니다. [양식](http://localhost:4502/editor.html/content/forms/af/prefill.html) 사용 중 **&quot;사용자 정의 AEM Forms PreFill Service&quot;** 미리 채우기 서비스로서. 이는 **양식 컨테이너** 섹션을 참조하십시오.
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). 올바른 값으로 채워진 양식이 표시됩니다.

>[!NOTE]
>
>com.aem.prefill.core.PrefillAdaptiveForm에 대한 디버깅을 활성화한 경우 생성된 xml 데이터 파일이 AEM 서버 설치 폴더에 기록됩니다.

