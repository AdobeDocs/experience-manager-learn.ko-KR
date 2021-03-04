---
title: 유용한 유틸리티 서비스
description: AEM Forms 개발자를 위한 유용한 유틸리티 서비스
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 3%

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

샘플 번들은 여기에서 [다운로드할 수 있습니다](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar).

## 유틸리티 서비스를 사용할 샘플 코드

다음은 JSP 페이지에서 org.w3c.dom.Document를 문자열로 만들어 문서를 변환하고 다음 코드 조각에 표시된 대로 CRX 저장소에 저장하는 데 사용한 코드입니다.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 전제 조건


[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar)을(를) 배포하고 번들을 시작해야 합니다.


이러한 유틸리티 서비스를 사용하여 CRX 저장소에 문서를 저장하려면 [서비스 사용자 문서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms)로 개발을 참조하십시오. fd-서비스 사용자에게 해당 CRX 폴더에 대한 [필수 권한](http://localhost:4502/useradmin)을 제공해야 합니다.

