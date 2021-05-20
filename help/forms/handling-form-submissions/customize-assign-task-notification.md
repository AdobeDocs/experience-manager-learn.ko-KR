---
title: 작업 알림 할당 사용자 지정
description: 작업 알림 할당 전자 메일에 양식 데이터를 포함합니다
sub-product: forms
feature: 워크플로우
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 1%

---


# 작업 알림 할당 사용자 지정

작업 할당 구성 요소는 워크플로우 참가자에게 작업을 할당하는 데 사용됩니다. 작업이 사용자 또는 그룹에 할당되면 정의된 사용자 또는 그룹 구성원에게 전자 메일 알림이 전송됩니다.
이 전자 메일 알림은 일반적으로 작업과 관련된 동적 데이터를 포함합니다. 시스템에서 생성한 [메타데이터 속성](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)을 사용하여 이 동적 데이터를 가져옵니다.
전자 메일 알림에 제출된 양식 데이터의 값을 포함하려면 사용자 지정 메타데이터 속성을 만든 다음 전자 메일 템플릿에서 이러한 사용자 지정 메타데이터 속성을 사용해야 합니다



## 사용자 지정 메타데이터 속성 만들기

권장되는 접근 방법은 [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)의 getUserMetadata 메서드를 구현하는 OSGI 구성 요소를 만드는 것입니다

다음 코드는 4개의 메타데이터 속성(_firstName_,_lastName_,_reason_ 및 _amountRequested_)을 만들고 제출된 데이터에서 해당 값을 설정합니다. 예를 들어 메타데이터 속성 _firstName_ 값은 제출된 데이터에서 firstName이라는 요소의 값으로 설정됩니다. 다음 코드는 적응형 양식의 제출된 데이터가 xml 형식으로 되어 있다고 가정합니다. JSON 스키마 또는 양식 데이터 모델을 기반으로 하는 적응형 Forms은 JSON 형식으로 데이터를 생성합니다.


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

## 작업 알림 전자 메일 템플릿에 사용자 지정 메타데이터 속성 사용

이메일 템플릿에서 amountRequested가 메타데이터 속성 `${amountRequested}`인 다음 구문을 사용하여 메타데이터 속성을 포함할 수 있습니다.

## 사용자 지정 메타데이터 속성을 사용하도록 작업 할당 구성

OSGi 구성 요소가 빌드되어 AEM 서버에 배포되면 사용자 지정 메타데이터 속성을 사용하도록 아래에 표시된 대로 작업 할당 구성 요소를 구성합니다.


![작업 알림](assets/task-notification.PNG)

## 사용자 지정 메타데이터 속성을 사용할 수 있도록 설정

![사용자 지정 메타데이터 속성](assets/custom-meta-data-properties.PNG)

## 서버에서 사용하려면 다음을 수행하십시오

* [일 CQ 메일 서비스 구성](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* 올바른 전자 메일 ID를 [관리 사용자](http://localhost:4502/security/users.html)와 연결
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip)을 다운로드하여 설치합니다
* [적응형 양식](assets/request-travel-authorization.zip)을 다운로드하고 [양식 및 문서 ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 AEM으로 가져옵니다.
* [웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [사용자 지정 번들](assets/work-items-user-service-bundle.jar)을 배포하고 시작합니다
* [양식 미리 보기 및 제출](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

양식 제출 작업 할당 알림은 관리자 사용자와 연결된 전자 메일 ID로 전송됩니다. 다음 스크린샷에서는 작업 할당 알림 샘플을 보여 줍니다

![알림](assets/task-nitification-email.png)

>[!NOTE]
>작업 알림 할당 전자 메일 서식 파일은 다음 형식이어야 합니다.
>
> subject=Task Assigned - `${workitem_title}`
>
> message=새 줄 문자가 없는 이메일 템플릿을 나타내는 문자열입니다.
