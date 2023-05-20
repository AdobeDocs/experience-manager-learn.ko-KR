---
title: 데이터베이스에서 양식의 서명 상태 업데이트
description: AEM 워크플로를 사용하여 데이터베이스에서 서명된 양식의 서명 상태 업데이트
feature: Adaptive Forms
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 2%

---

# 서명 상태 업데이트

UpdateSignatureStatus 워크플로는 사용자가 서명식을 완료하면 트리거됩니다. 다음은 워크플로의 흐름입니다

![메인 워크플로](assets/update-signature.PNG)

서명 상태 업데이트는 사용자 지정 프로세스 단계입니다.
사용자 지정 프로세스 단계를 구현하는 주요 이유는 AEM Workflow를 확장하기 위해서입니다. 다음은 서명 상태를 업데이트하는 데 사용되는 사용자 지정 코드입니다.
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

## Assets

서명 상태 업데이트 워크플로는 다음과 같을 수 있습니다 [여기에서 다운로드됨](assets/update-signature-status-workflow.zip)

## 다음 단계

[서명할 다음 양식을 표시하도록 요약 단계 사용자 지정](./customize-summary-component.md)
