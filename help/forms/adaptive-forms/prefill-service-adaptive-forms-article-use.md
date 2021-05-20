---
title: 응용 Forms의 미리 채우기 서비스
seo-title: 응용 Forms의 미리 채우기 서비스
description: 백엔드 데이터 소스에서 데이터를 가져와 적응형 양식을 미리 채웁니다.
seo-description: 백엔드 데이터 소스에서 데이터를 가져와 적응형 양식을 미리 채웁니다.
sub-product: forms
feature: 적응형 양식
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---


# 적응형 Forms에서 미리 채우기 서비스 사용

기존 데이터를 사용하여 적응형 양식의 필드를 미리 채울 수 있습니다. 사용자가 양식을 열면 해당 필드의 값이 미리 채워집니다. 적응형 양식 필드를 미리 채우는 방법에는 여러 가지가 있습니다. 이 문서에서는 AEM Forms 미리 채우기 서비스를 사용하여 적응형 양식 미리 채우기를 살펴봅니다.

적응형 양식을 미리 채우는 다양한 방법에 대해 알아보려면 [이 설명서를 따르십시오](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

미리 채우기 서비스를 사용하여 적응형 양식을 미리 채우려면 DataProvider 인터페이스를 구현하는 클래스를 만들어야 합니다. getPrefillData 메서드는 적응형 양식이 필드를 미리 채우기 위해 사용하는 데이터를 작성하고 반환하는 논리를 갖습니다. 이 방법에서는 소스에서 데이터를 가져온 다음 데이터 문서의 입력 스트림을 반환할 수 있습니다. 다음 샘플 코드는 로그인한 사용자의 사용자 프로필 정보를 가져오고 적응형 양식에 의해 입력 스트림을 사용하도록 반환되는 XML 문서를 구성합니다.

아래의 코드 조각에는 DataProvider 인터페이스를 구현하는 클래스가 있습니다. 로그인한 사용자에 대한 액세스 권한을 얻은 다음 로그인한 사용자의 프로필 정보를 가져옵니다. 그런 다음 &quot;data&quot;라는 루트 노드 요소를 사용하여 XML 문서를 만들고 이 데이터 노드에 적절한 요소를 추가합니다. XML 문서가 구성되면 XML 문서의 입력 스트림이 반환됩니다.

그런 다음 이 클래스는 OSGi 번들로 만들어져서 AEM에 배포됩니다. 번들을 배포하면 이 미리 채우기 서비스를 적응형 양식의 미리 채우기 서비스로 사용할 수 있습니다.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

서버에서 이 기능을 테스트하려면 다음을 수행하십시오

* [컴퓨터에 zip 파일의 내용을 다운로드하고 추출합니다](assets/prefillservice.zip)
* 로그인한 [사용자의 프로필](http://localhost:4502/libs/granite/security/content/useradmin) 정보가 완전히 입력되었는지 확인하십시오. 이것은 샘플이 작동하는 데 필요합니다. 샘플에 누락된 사용자 프로필 속성을 확인하는 오류 검사가 없습니다.
* [AEM 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들을 배포합니다
* XSD를 사용하여 적응형 양식 만들기
* 적응형 양식의 미리 채우기 서비스로 &quot;사용자 지정 Aem 양식 미리 채우기 서비스&quot;를 연결
* 스키마 요소를 양식에 드래그하여 놓습니다
* 양식 미리 보기

>[!NOTE]
>
>적응형 양식이 XSD를 기반으로 하는 경우 미리 채우기 서비스에서 반환한 XML 문서가 적응형 양식을 기반으로 하는 XSD와 일치하는지 확인하십시오.
>
>적응형 양식이 XSD를 기반으로 하지 않으면 필드를 수동으로 바인딩해야 합니다. 예를 들어 적응형 양식 필드를 XML 데이터의 fname 요소에 바인딩하려면 적응형 양식 필드의 바인딩 참조에서 `/data/fname` 을 사용합니다.

