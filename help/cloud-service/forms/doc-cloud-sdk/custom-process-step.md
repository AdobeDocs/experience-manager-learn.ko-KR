---
title: 사용자 지정 프로세스 단계 만들기
description: Document Cloud을 사용하여 word, excel 첨부 파일을 PDF로 변환하는 사용자 정의 프로세스 단계입니다.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 개발
thumbnail: 7837.jpg
kt: 7837
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 1%

---

# 사용자 지정 프로세스 단계

다음은 기본 파일을 변환된 pdf로 변환하고 대체하는 사용자 정의 프로세스 단계의 전체 코드입니다.
이 사용자 지정 단계에서는 워크플로우에서 프로세스 인수로 제공되는 폴더 이름 아래의 모든 첨부 파일을 검색합니다.
이 사용자 정의 프로세스 단계에서는 사용자 정의 DocumentCloudSDK 서비스의 메서드를 사용하여 PDF를 만듭니다.


```java
package com.aemforms.doccloud.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;;

@Component(property = {
	Constants.SERVICE_DESCRIPTION + "=Convert form attachments to PDF Using Document Cloud API",
	Constants.SERVICE_VENDOR + "=Adobe Systems",
	"process.label" + "=Convert form attachments to PDF Using Document Cloud API"
})

public class ConvertAttachmentsToPDF implements WorkflowProcess {
	private static final Logger log = LoggerFactory.getLogger(ConvertAttachmentsToPDF.class);
	@Reference
	QueryBuilder queryBuilder;
	@Reference
	DocumentCloudSDKService documentCloudService;
	@Override
	public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException {

		log.debug("The payload path is " + workItem.getWorkflowData().getPayload().toString());
		log.debug("The form attachments are stored under  ..." + processArguments.get("PROCESS_ARGS", "string").toString() + " folder in the payload");

		Session session = workflowSession.adaptTo(Session.class);
		Map<String, String> map = new HashMap<String, String> ();
		map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
		map.put("type", "nt:file");
		Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
		query.setStart(0);
		query.setHitsPerPage(20);

		SearchResult result = query.getResult();
		log.debug("Get result hits " + result.getHits().size());
		Node attachmentNode = null;
		for (Hit hit: result.getHits()) {
			try {
				String path = hit.getPath();
				log.debug("The title of the attachment is  " + hit.getTitle() + " and path " + path);
				String title = hit.getTitle();
				title = title.substring(0, title.indexOf("."));
				attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
				InputStream xmlDataStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
				Document convertedPDF = documentCloudService.createPDFFromInputStream(xmlDataStream, hit.getTitle());
				Node nonPDFNode = session.getNode(path);
				session.move(nonPDFNode.getPath(), nonPDFNode.getParent().getPath() + "/" + title + ".pdf");
				Binary binary = session.getValueFactory().createBinary(convertedPDF.getInputStream());
				attachmentNode.setProperty("jcr:data", binary);
				log.debug("Replacing the original file with converted pdf ");
				session.save();

			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}

	}

}
```
