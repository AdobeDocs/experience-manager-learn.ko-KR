---
title: 데이터베이스에서 양식의 서명 상태 업데이트
description: AEM 작업 과정을 사용하여 데이터베이스에서 서명된 양식의 서명 상태를 업데이트합니다.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 0%

---


# 서명 상태 업데이트

UpdateSignatureStatus 워크플로우는 사용자가 서명 의식을 완료하면 트리거됩니다. 다음은 워크플로우의 흐름입니다.

![기본 워크플로우](assets/update-signature.PNG)

서명 상태 업데이트는 사용자 지정 프로세스 단계입니다.
사용자 지정 프로세스 단계를 구현하는 주된 이유는 AEM Workflow를 확장하는 것입니다. 다음은 서명 상태를 업데이트하는 데 사용되는 사용자 지정 코드입니다.
이 사용자 지정 프로세스 단계의 코드는 SignMultipleForms 서비스를 참조합니다.


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## 자산

서명 상태 업데이트 작업 과정은 여기에서 [다운로드할 수 있습니다](assets/update-signature-status-workflow.zip)

