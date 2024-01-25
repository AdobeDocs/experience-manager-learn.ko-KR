---
title: DAM에 AEM Forms DoR 태그 지정 및 저장
description: 이 문서에서는 AEM DAM에 AEM Forms에서 생성한 DoR을 저장하고 태그 지정하는 사용 사례를 살펴봅니다. 제출된 양식 데이터를 기반으로 문서에 대한 태깅을 수행합니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 195
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# DAM에 AEM Forms DoR 태그 지정 및 저장 {#tagging-and-storing-aem-forms-dor-in-dam}

이 문서에서는 AEM DAM에 AEM Forms에서 생성한 DoR을 저장하고 태그 지정하는 사용 사례를 살펴봅니다. 제출된 양식 데이터를 기반으로 문서에 대한 태깅을 수행합니다.

고객의 일반적인 요구는 AEM DAM에 AEM Forms에서 생성한 기록 문서(DoR)를 저장하고 태그를 지정하는 것입니다. 문서의 태깅은 적응형 Forms의 제출 데이터를 기반으로 해야 합니다. 예를 들어 제출된 데이터의 고용 상태가 &quot;사용 중지됨&quot;인 경우 &quot;사용 중지됨&quot; 태그로 문서에 태그를 지정하고 문서를 DAM에 저장하려고 합니다.

사용 사례는 다음과 같습니다.

* 사용자가 적응형 양식을 작성합니다. 적응형 양식에서는 사용자의 결혼 상태(ex Single)와 취업 상태(ex Retired)를 포착한다.
* 양식 제출 시 AEM 워크플로가 트리거됩니다. 이 워크플로우는 결혼 상태(미혼) 및 취업 상태(퇴직됨)로 문서에 태그를 지정하고 문서를 DAM에 저장합니다.
* 문서가 DAM에 저장되면 관리자는 이러한 태그로 문서를 검색할 수 있어야 합니다. 예를 들어 Single 또는 Retired에 대한 검색은 적절한 DoR을 가져옵니다.

이 사용 사례를 충족하기 위해 맞춤형 프로세스 단계가 작성되었습니다. 이 단계에서는 제출된 데이터에서 적절한 데이터 요소의 값을 가져옵니다. 그런 다음 이 값을 사용하여 태그 타일을 구성합니다. 예를 들어 결혼 상태 요소의 값이 &quot;Single&quot;이면 태그 제목은 **Peak:EmploymentStatus/Single이 됩니다. **TagManager API 를 사용하여 태그를 찾고 태그를 DoR에 적용합니다.

다음은 AEM DAM에 기록 문서에 태그를 지정하고 저장하는 전체 코드입니다.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

시스템에서 이 샘플을 작업하려면 아래 단계를 따르십시오.
* [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue 번들 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 제출된 양식 데이터의 태그를 설정하는 사용자 정의 OSGI 번들입니다.

* [샘플 적응형 양식 다운로드](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Forms 및 문서로 이동](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 만들기 를 클릭합니다 | 파일 업로드 및 태그 업로드 및 dam-adaptive-form.zip에 저장

* [문서 에셋 가져오기](assets/tag-and-store-in-dam-assets.zip) AEM 패키지 관리자 사용
* 를 엽니다. [미리 보기 모드의 샘플 양식](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **모든 필드 입력** 양식을 제출하십시오.
* [DAM의 Peak 폴더로 이동](http://localhost:4502/assets.html/content/dam/Peak). Peak 폴더에 DoR이 표시됩니다. 문서의 속성을 확인합니다. 적절하게 태그가 지정되어 있어야 합니다.
축하합니다!! 시스템에 샘플을 성공적으로 설치했습니다.

* 다음을 살펴보겠습니다. [워크플로우](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) 양식 제출 시 트리거됩니다.
* 워크플로우의 첫 번째 단계에서는 지원자 이름과 거주 국가를 연결하여 고유한 파일 이름을 만듭니다.
* 워크플로우의 두 번째 단계는 태그 계층 구조와 태그 지정이 필요한 양식 필드 요소를 전달합니다. 프로세스 단계는 제출된 데이터에서 값을 추출하고 문서에 태그를 지정해야 하는 태그 제목을 구성합니다.
* DAM의 다른 폴더에 DoR을 저장하려면 아래 스크린샷에 지정된 대로 구성 속성을 사용하여 폴더 위치를 지정합니다.

다른 두 매개 변수는 적응형 양식 제출 옵션에 지정된 대로 DoR 및 데이터 파일 경로와 관련이 있습니다. 여기서 지정하는 값이 적응형 양식 제출 옵션에서 지정한 값과 일치하는지 확인하십시오.

![태그 Dor](assets/tag_dor_service_configuration.gif)
