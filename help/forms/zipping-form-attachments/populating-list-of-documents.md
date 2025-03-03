---
title: 목록 변수를 채우는 맞춤형 프로세스 단계
description: Adobe Experience Manager에서 유형 문서 및 문자열의 목록 변수를 채우는 사용자 지정 프로세스 단계를 만드는 방법을 알아봅니다.
feature: Workflow
topic: Development
version: 6.5
role: Developer
level: Beginner
kt: kt-8063
exl-id: 09d9eabf-4815-4159-b6c7-cf2ebc8a2df5
duration: 68
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---


# 사용자 정의 프로세스 단계

이 안내서에서는 Adobe Experience Manager에서 배열 목록 유형의 목록 변수를 첨부 파일 및 첨부 파일 이름으로 채우는 사용자 지정 프로세스 단계를 안내합니다. 이러한 변수는 이메일 전송 워크플로우 구성 요소에 필수적입니다.

OSGi 번들 만들기에 익숙하지 않은 경우 다음 [지침](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)을(를) 따르십시오.

사용자 지정 프로세스 단계의 코드는 다음 작업을 수행합니다.

1. 페이로드 폴더 아래의 모든 적응형 양식 첨부 파일에 대한 쿼리 폴더 이름은 단계에 프로세스 인수로 전달됩니다.
2. `listOfDocuments` 워크플로 변수를 채웁니다.
3. `attachmentNames` 워크플로 변수를 채웁니다.
4. 워크플로 변수 `no_of_attachments`의 값을 설정합니다.

```java
package com.aemforms.formattachments.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
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
import com.day.cq.search.result.SearchResult;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=PopulateListOfDocuments",
        "process.label=PopulateListOfDocuments"
})
public class PopulateListOfDocuments implements WorkflowProcess {

        private static final Logger log = LoggerFactory.getLogger(PopulateListOfDocuments.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                int no_of_attachments = result.getHits().size();
                Document[] listOfDocuments = new Document[no_of_attachments];
                String[] attachmentNames = new String[no_of_attachments];
                int i = 0;
                for (Hit hit: result.getHits()) {
                        try {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                listOfDocuments[i] = new Document(attachmentStream);
                                attachmentNames[i] = new String(hit.getTitle());
                                log.debug("Added " + hit.getTitle() + "to the list");
                                i++;
                        } catch (Exception e) {
                                log.error("Unable to obtain attachment", e);
                        }
                }

                metaDataMap.put("no_of_attachments", no_of_attachments);
                metaDataMap.put("listOfDocuments", listOfDocuments);
                metaDataMap.put("attachmentNames", attachmentNames);

                log.debug("Updated workflow");
        }

}
```

>[!NOTE]
>
> 코드가 작동하도록 워크플로우에 다음 변수가 정의되어 있는지 확인합니다.
> 
> - `listOfDocuments`: ArrayList of Documents 형식의 변수
> - `attachmentNames`: String의 ArrayList 유형 변수
> - `no_of_attachments`: Double 형식의 변수

## 다음 단계

[로컬 시스템에서 솔루션 테스트](./test.md)