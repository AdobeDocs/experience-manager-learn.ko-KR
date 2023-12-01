---
title: 제출된 데이터를 문자열 변수에 저장
description: 바인딩된 데이터를 추출하고 유형 문자열의 워크플로 변수에 저장하는 사용자 지정 프로세스 단계
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11199
last-substantial-update: 2022-10-02T00:00:00Z
thumbnail: string-variable.jpg
exl-id: 65dcbfbb-7eb5-4fa3-aeb3-587c59ee2fe9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# 바인딩된 데이터를 추출하여 문자열 변수에 저장

이 기능을 사용하면 제출된 데이터를 이메일의 본문에 포함할 수 있습니다. 사용자 지정 프로세스 단계에서는 **바인딩된 데이터** 적응형 양식 제출에서 형식 문자열의 변수를 데이터로 채웁니다. 그런 다음 이 문자열 변수를 사용하여 전자 메일 템플릿에 데이터를 삽입할 수 있습니다.
다음 스크린샷은 사용자 지정 프로세스 단계에 전달하는 데 필요한 인수를 보여 줍니다
![프로세스 단계](assets/save-submitted-data-string.png)

다음은 매개변수입니다

* `data.xml` - 제출된 데이터가 있는 파일 . 포맷이 json인 경우 파일 이름은 data.json이 될 수 있습니다.

그런 다음 사용자 지정 프로세스 단계는 바인딩된 데이터를 추출하여 워크플로우에 정의된 submittedDataString 변수에 저장합니다


[사용자 지정 번들은 여기에서 다운로드할 수 있습니다.](assets/AEMFormsProcessStep.core-1.0.0-SNAPSHOT.jar)

```java
package AEMFormsProcessStep.core;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import javax.jcr.Session;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import javax.xml.parsers.DocumentBuilder;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.StringWriter;

import javax.jcr.Node;

@Component(property = {
  "service.description=Save submitted Payload as String",
  "process.label=Save submitted Payload as String"
})
public class SaveSubmittedDataInStringVariable implements WorkflowProcess {
  private Logger log;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap) throws WorkflowException {

    String submittedDataFile = ((String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string")).toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log = LoggerFactory.getLogger(SaveSubmittedDataInStringVariable.class);

    String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
    Session session = (Session) workflowSession.adaptTo((Class) Session.class);
    Node submittedDataNode = null;
    try {
      submittedDataNode = session.getNode(dataFilePath);
      if (submittedDataFile.endsWith("json")) {
        InputStream jsonDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        while ((inputStr = streamReader.readLine()) != null) {
          responseStrBuilder.append(inputStr);
        }
        JsonObject jsonObject = new Gson().fromJson(responseStrBuilder.toString(), JsonObject.class);
        JsonObject boundData = jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data");
        System.out.println(jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data"));
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", boundData.toString());

      }
      if (submittedDataFile.endsWith("xml")) {
        log.debug("Got xml file");
        DocumentBuilderFactory factory = null;
        DocumentBuilder builder = null;
        Document xmlDocument = null;
        InputStream xmlDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        System.out.println("Got xml file" + submittedDataNode);
        XPath xPath = XPathFactory.newInstance().newXPath();
        factory = DocumentBuilderFactory.newInstance();
        builder = factory.newDocumentBuilder();
        xmlDocument = builder.parse(xmlDataStream);

        org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afBoundData").evaluate(xmlDocument, XPathConstants.NODE);
        Transformer transformer = TransformerFactory.newInstance().newTransformer();
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");
        StreamResult result = new StreamResult(new StringWriter());
        DOMSource source = new DOMSource(node);
        transformer.transform(source, result);
        String xmlString = result.getWriter().toString();
        log.debug("The xml string is " + xmlString);
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", xmlString);
      }

    } catch (Exception e) {
      
      log.debug(e.getMessage());
    }

  }

}
```
