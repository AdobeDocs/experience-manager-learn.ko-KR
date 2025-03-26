---
title: 작업 알림 할당 사용자 지정
description: 작업 알림 할당 전자 메일에 양식 데이터 포함
feature: Workflow
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 144
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---

# 작업 알림 할당 사용자 지정

작업 할당 구성 요소는 워크플로 참여자에게 작업을 할당하는 데 사용됩니다. 작업이 사용자 또는 그룹에 할당되면 정의된 사용자 또는 그룹 구성원에게 이메일 알림이 전송됩니다.
이 전자 메일 알림에는 일반적으로 작업과 관련된 동적 데이터가 포함됩니다. 이 동적 데이터는 시스템에서 생성한 [메타데이터 속성](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)을 사용하여 가져옵니다.
제출된 양식 데이터의 값을 이메일 알림에 포함하려면 사용자 지정 메타데이터 속성을 만든 다음 이메일 템플릿에서 이러한 사용자 지정 메타데이터 속성을 사용해야 합니다



## 사용자 지정 메타데이터 속성을 만드는 중

권장 접근 방법은 [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)의 getUserMetadata 메서드를 구현하는 OSGI 구성 요소를 만드는 것입니다.

다음 코드는 4개의 메타데이터 속성(_firstName_,_lastName_,_reason_ 및 _amountRequested_)을 만들고 제출된 데이터에서 해당 값을 설정합니다. 예를 들어 메타데이터 속성 _firstName_&#x200B;의 값은 제출된 데이터에서 firstName이라는 요소의 값으로 설정됩니다. 다음 코드는 적응형 양식의 제출된 데이터가 xml 형식이라고 가정합니다. JSON 스키마 또는 양식 데이터 모델 기반의 적응형 Forms은 JSON 형식으로 데이터를 생성합니다.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## 작업 알림 이메일 템플릿에서 사용자 지정 메타데이터 속성 사용

전자 메일 템플릿에서 amountRequested가 메타데이터 속성 `${amountRequested}`인 다음 구문을 사용하여 메타데이터 속성을 포함할 수 있습니다.

## 사용자 지정 메타데이터 속성을 사용하도록 할당 작업 구성

OSGi 구성 요소가 AEM 서버에 빌드 및 배포되면 사용자 지정 메타데이터 속성을 사용하도록 아래와 같이 작업 할당 구성 요소를 구성합니다.


![작업 알림](assets/task-notification.PNG)

## 사용자 지정 메타데이터 속성 사용 활성화

![사용자 지정 메타데이터 속성](assets/custom-meta-data-properties.PNG)

## 서버에서 시도

* [일별 CQ 메일 서비스 구성](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* 올바른 전자 메일 ID를 [관리자](http://localhost:4502/security/users.html)와(과) 연결
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [워크플로 및 알림 템플릿](assets/workflow-and-task-notification-template.zip)을(를) 다운로드하여 설치하십시오.
* [적응형 양식](assets/request-travel-authorization.zip)을(를) 다운로드하고 [양식 및 문서 ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 AEM으로 가져옵니다.
* [웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [사용자 지정 번들](assets/work-items-user-service-bundle.jar)을 배포 및 시작
* [양식 미리 보기 및 제출](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

양식 제출 시 작업 할당 알림이 책임자와 연결된 전자 메일 ID로 전송됩니다. 다음 스크린샷은 샘플 작업 할당 알림을 보여 줍니다

![알림](assets/task-nitification-email.png)

>[!NOTE]
>할당 작업 알림의 이메일 템플릿은 다음 형식이어야 합니다.
>
> subject=작업 할당됨 - `${workitem_title}`
>
> message=새 줄 문자 없이 이메일 템플릿을 나타내는 문자열입니다.

## 작업 할당 전자 메일 알림의 작업 설명

경우에 따라 이전 작업 소유자의 주석을 후속 작업 알림에 포함할 수 있습니다. 작업의 마지막 주석을 캡처하는 코드가 아래에 나열됩니다.

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

위의 코드를 사용하는 번들은 [여기에서 다운로드](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)할 수 있습니다.
