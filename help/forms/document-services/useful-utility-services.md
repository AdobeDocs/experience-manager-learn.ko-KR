---
title: 유용한 유틸리티 서비스
description: AEM Forms 개발자를 위한 몇 가지 유용한 유틸리티 서비스
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# 유용한 유틸리티 서비스

이 샘플 번들은 AEM Forms 개발자가 사용할 수 있는 유용한 유틸리티 서비스를 제공합니다. 다음 서비스를 사용할 수 있습니다.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

샘플 번들은 [여기에서 다운로드](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)할 수 있습니다.

## 유틸리티 서비스를 사용할 샘플 코드

다음은 문자열에서 org.w3c.dom.Document를 만들고 문서를 변환하여 다음 코드 조각에 표시된 CRX 저장소에 저장하는 데 사용되는 JSP 페이지에 사용된 코드입니다.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 사전 요구 사항


[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar)을(를) 배포하고 번들을 시작해야 합니다.


이러한 유틸리티 서비스를 사용하여 CRX 저장소에 문서를 저장하려면 [서비스 사용자로 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms)을(를) 따르십시오. 적절한 CRX 폴더에 대한 [필요한 권한](http://localhost:4502/useradmin)을(를) fd-service 사용자에게 제공해야 합니다.
