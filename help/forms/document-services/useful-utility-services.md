---
title: 유용한 유틸리티 서비스
description: AEM Forms 개발자를 위한 몇 가지 유용한 유틸리티 서비스
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '155'
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

샘플 번들은 [여기에서 다운로드](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## 유틸리티 서비스를 사용할 샘플 코드

다음은 JSP 페이지에서 문자열을 사용하여 org.w3c.dom.Document를 만들고 문서를 변환하여 다음 코드 조각에 표시된 것처럼 CRX 저장소에 저장하는 데 사용한 코드입니다.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 사전 요구 사항


를 배포해야 합니다. [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) 번들을 시작합니다.


이러한 유틸리티 서비스를 사용하여 CRX 저장소에 문서를 저장하려면 다음을 수행하십시오 [서비스 사용 문서 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). 에 를 제공해야 합니다. [필수 권한](http://localhost:4502/useradmin) 를 입력합니다.
