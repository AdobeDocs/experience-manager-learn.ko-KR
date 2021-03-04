---
title: 적응형 Forms의 프리플라이트 서비스
seo-title: 적응형 Forms의 프리플라이트 서비스
description: 백엔드 데이터 소스에서 데이터를 가져와 적응형 양식을 미리 채웁니다.
seo-description: 백엔드 데이터 소스에서 데이터를 가져와 적응형 양식을 미리 채웁니다.
sub-product: 양식
feature: 적응형 양식
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 0%

---


# 적응형 Forms에서 프리플라이트 서비스 사용

기존 데이터를 사용하여 적응형 양식의 필드를 미리 채울 수 있습니다. 사용자가 양식을 열면 해당 필드에 대한 값이 미리 작성됩니다. 적응형 양식 필드를 미리 작성하는 방법에는 여러 가지가 있습니다. 이 문서에서는 AEM Forms 프리플라이트 서비스를 사용하여 자동 완성 양식을 만드는 방법을 살펴봅니다.

적응형 양식을 미리 채우는 다양한 방법에 대한 자세한 내용을 살펴보려면 [이 설명서를 따르십시오](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

프리플라이트 서비스를 사용하여 적응형 양식을 미리 채우려면 DataProvider 인터페이스를 구현하는 클래스를 만들어야 합니다. getPrefillData 메서드에는 적응형 양식이 필드를 미리 채우기 위해 사용할 데이터를 작성하고 반환하는 논리를 갖게 됩니다. 이 방법에서는 소스에서 데이터를 가져오고 데이터 문서의 입력 스트림을 반환할 수 있습니다. 다음 샘플 코드는 로그인한 사용자의 사용자 프로필 정보를 가져오고 적응형 양식에 의해 입력 스트림이 사용되도록 반환되는 XML 문서를 구성합니다.

아래의 코드 조각에는 DataProvider 인터페이스를 구현하는 클래스가 있습니다. 로그인한 사용자에 대한 액세스 권한을 얻은 다음 로그인한 사용자의 프로필 정보를 가져옵니다. 그런 다음 &quot;data&quot;라는 루트 노드 요소를 사용하여 XML 문서를 만들고 이 데이터 노드에 적절한 요소를 추가합니다. XML 문서를 구성하면 XML 문서의 입력 스트림이 반환됩니다.

이 클래스는 OSGi 번들로 제작되어 AEM에 배포됩니다. 번들이 배포되면 이 자동 완성 서비스를 응용 양식의 자동 완성 서비스로 사용할 수 있습니다.

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

* [컴퓨터에 zip 파일의 내용을 다운로드하고 추출합니다.](assets/prefillservice.zip)
* [사용자의 프로필](http://localhost:4502/libs/granite/security/content/useradmin) 정보에 로그인한 정보가 완전히 채워졌는지 확인합니다. 샘플이 작동하려면 필요합니다. 샘플에 누락된 사용자 프로필 속성을 확인하는 오류 검사가 없습니다.
* [AEM 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들 배포
* XSD를 사용하여 적응형 양식 만들기
* &quot;사용자 지정 Aem 양식 채우기 서비스&quot;를 적응형 양식의 자동 완성 서비스로 연결하십시오.
* 스키마 요소를 양식에 드래그하여 놓기
* 양식 미리 보기

>[!NOTE]
>
>적응형 양식이 XSD를 기반으로 하는 경우, 자동 완성 서비스에서 반환된 XML 문서가 적응형 양식의 XSD와 일치하는지 확인하십시오.
>
>적응형 양식이 XSD를 기반으로 하지 않으면 필드를 수동으로 바인딩해야 합니다. 예를 들어 적응형 양식 필드를 XML 데이터의 fname 요소에 바인딩하려면 적응형 양식 필드의 바인딩 참조에서 `/data/fname`을 사용합니다.

