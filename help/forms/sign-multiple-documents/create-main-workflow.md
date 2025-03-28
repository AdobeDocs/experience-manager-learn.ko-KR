---
title: 서명 프로세스를 트리거할 주 워크플로우 만들기
description: 데이터베이스에 서명할 양식을 저장하는 워크플로우 만들기
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 6887.jpg
jira: KT-6887
topic: Development
role: Developer
level: Intermediate
exl-id: 338d9522-f6da-4aa7-b5d8-b9fff39ea94b
duration: 70
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# 주 워크플로우 만들기

사용자가 초기 양식(**RefinanceForm**)을 제출하면 기본 워크플로우가 트리거됩니다. 다음은 워크플로의 흐름입니다

![main-workflow](assets/main-workflow.PNG)

**서명에 Forms 저장**&#x200B;은(는) 사용자 지정 프로세스 단계입니다.

사용자 지정 프로세스 단계를 구현하는 목적은 AEM 워크플로우를 확장하는 것입니다. 다음 코드는 사용자 지정 프로세스 단계를 구현합니다. 이 코드는 서명할 양식 이름을 추출하고 제출된 양식 데이터를 SignMultipleForms 서비스의 `insertData` 메서드에 전달합니다. 그런 다음 `insertData` 메서드는 데이터 원본 **aemformstuorial**&#x200B;에 의해 식별된 데이터베이스에 행을 삽입합니다.

이 사용자 지정 프로세스 단계의 코드는 `SignMultipleForms` 서비스를 참조합니다.



```java
package com.aem.forms.signmultipleforms;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=StoreFormsToSign",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=StoreFormsToSign"
})
public class StoreFormsToSignWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(StoreFormsToSignWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log.debug("The payload  in StoreFormsToSign " + workItem.getWorkflowData().getPayload().toString());
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    String serverURL = arg2.get("PROCESS_ARGS", "string").toString();
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/formsToSign").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      log.debug("The form names to sign are  t" + node.getTextContent());
      String formNamesToSign[] = node.getTextContent().split(",");
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      DOMSource source = new DOMSource(xmlDocument);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      InputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
      StringWriter writer = new StringWriter();
      IOUtils.copy(inputStream, writer, Charset.defaultCharset());
      String formData = writer.toString();
      signMultipleForms.insertData(formNamesToSign, formData, serverURL, workItem, workflowSession);

  }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }
}
```




## 자산

이 문서에 사용된 여러 Forms 서명 워크플로는 [여기에서 다운로드](assets/sign-multiple-forms-workflows.zip)할 수 있습니다.

>[!NOTE]
> 전자 메일 알림을 전송하려면 Day CQ 메일 서비스를 구성해야 합니다. 전자 메일 템플릿도 위의 패키지에 포함되어 있습니다.

## 다음 단계

[문서 서명 시 서명 상태 업데이트](./update-signature-status.md)
