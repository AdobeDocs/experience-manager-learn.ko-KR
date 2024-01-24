---
title: PDF 양식에서 데이터를 내보내는 OSGi 서비스 만들기
description: FormsService API를 사용하여 PDF 양식에서 데이터 내보내기
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: c3032669-154c-4565-af6e-32d94e975e37
duration: 65
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 1%

---

# 데이터 내보내기

PDF 파일에서 적응형 양식을 채우는 첫 번째 단계는 주어진 PDF 파일에서 데이터를 내보내고 AEM 저장소에 저장하는 것입니다.

다음 코드는 업로드된 PDF에서 데이터를 추출하기 위해 작성되었으며 적응형 양식을 채우는 데 사용할 수 있는 것보다 올바른 형식을 가져오도록 마사지

```java
public String getFormData(Document pdfForm) {
   DocumentBuilderFactory factory = null;
   DocumentBuilder builder = null;
   org.w3c.dom.Document xmlDocument = null;

   try {
      Document xmlData = formsService.exportData(pdfForm, DataFormat.Auto);
      xmlData.copyToFile(new File("xmlData.xml"));
      factory = DocumentBuilderFactory.newInstance();
      factory.setNamespaceAware(true);
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlData.getInputStream());

      org.w3c.dom.Node xdpNode = xmlDocument.getDocumentElement();
      log.debug("Got xdp " + xdpNode.getNodeName());
      org.w3c.dom.Node datasets = getChildByTagName(xdpNode, "xfa:datasets");
      log.debug("Got datasets " + datasets.getNodeName());
      org.w3c.dom.Node data = getChildByTagName(datasets, "xfa:data");
      log.debug("Got data " + data.getNodeName());
      org.w3c.dom.Node topmostSubform = getChildByTagName(data, "topmostSubform");

      if (topmostSubform != null) {

         org.w3c.dom.Document newXmlDocument = builder.newDocument();
         org.w3c.dom.Node importedNode = newXmlDocument.importNode(topmostSubform, true);
         newXmlDocument.appendChild(importedNode);
         Document aemFDXmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXmlDocument);
         aemFDXmlDocument.copyToFile(new File("aemFDXmlDocument.xml"));
         // saveDocumentInCrx is an utility method of the custom DocumentServices service. 
         return documentServices.saveDocumentInCrx("/content/exporteddata", ".xml", aemFDXmlDocument);
      }

   } catch (Exception e) {
      log.debug("Error:  " + e.getMessage());

   }
   return null;
}
```

다음은 를 추출하기 위해 작성된 유틸리티 함수입니다. _**최상위 하위 양식**_ 적절한 네임스페이스 사용

```java
private static org.w3c.dom.Node getChildByTagName(org.w3c.dom.Node parent, String tagName) {
   NodeList nodeList = parent.getChildNodes();
   for (int i = 0; i < nodeList.getLength(); i++) {
      org.w3c.dom.Node node = nodeList.item(i);
      if (node.getNodeType() == org.w3c.dom.Node.ELEMENT_NODE && node.getNodeName().equals(tagName)) {
         return node;
      }
   }
   return null;
}
```

추출된 데이터는 crx 저장소의 /content/exporteddata 노드 아래에 저장됩니다. 내보낸 데이터의 파일 경로는 적응형 양식을 채우기 위한 호출 애플리케이션으로 반환됩니다.

## 다음 단계

[PDF 파일에서 데이터 가져오기](./populate-adaptive-form.md)
