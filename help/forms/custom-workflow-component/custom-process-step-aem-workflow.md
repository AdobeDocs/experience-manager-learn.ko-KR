---
title: 대화 상자를 사용하여 사용자 지정 프로세스 단계 구현
description: 사용자 정의 프로세스 단계를 사용하여 파일 시스템에 적응형 양식 첨부 파일 작성
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 0%

---

# 사용자 지정 프로세스 단계

이 자습서는 사용자 지정 워크플로우 구성 요소를 구현해야 하는 AEM Forms 고객을 위한 것입니다.워크플로우 구성 요소를 만드는 첫 번째 단계는 워크플로우 구성 요소와 연결될 Java 코드를 작성하는 것입니다. 이 자습서를 위해 적응형 양식 첨부 파일을 파일 시스템에 저장하는 간단한 Java 클래스를 작성하겠습니다.이 Java 코드는 워크플로우 구성 요소에 지정된 인수를 읽습니다.

Java 클래스를 작성하고 클래스를 OSGi 번들로 배포하는 데 다음 단계가 필요합니다

## Maven 프로젝트 만들기

첫 번째 단계는 적절한 Adobe Maven Archetype을 사용하여 maven 프로젝트를 만드는 것입니다. 자세한 단계는 다음과 같습니다 [문서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). maven 프로젝트를 eclipse로 가져온 후에는 프로세스 단계에서 사용할 수 있는 첫 번째 OSGi 구성 요소 작성을 시작할 준비가 되었습니다.


### WorkflowProcess를 구현하는 클래스 만들기

Eclipse IDE에서 maven 프로젝트를 엽니다. 확장 **projectname** > **코어** 폴더를 입력합니다. src/main/java 폴더를 확장합니다. &quot;core&quot;로 끝나는 패키지가 표시됩니다. 이 패키지에서 WorkflowProcess를 구현하는 Java 클래스를 만듭니다. 실행 메서드를 재정의해야 합니다. 실행 메서드의 서명은 public void execute(WorkItem, WorkflowSession, MetaDataMap processArguments)와 같습니다. WorkflowException

이 자습서에서는 AEM Workflow의 일부로 적응형 양식에 추가된 첨부 파일을 파일 시스템에 작성하려고 합니다.

이 사용 사례를 수행하기 위해 다음 java 클래스가 작성됩니다

이 코드를 살펴보겠습니다

```java
package com.mysite.core;
import java.io.File;
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
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);
<<<<<<< HEAD
    log.debug("Got Attachments PAth" + attachmentsPath);
    String saveToLocation = metaDataMap.get("saveToLocation", String.class);
=======
    log.debug("Got Attachments Path" + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
>>>>>>> 1d85e32dec060b5f5f33607225cd6ceb8d2b21c5
    log.debug("Got Save Location" + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* 첨부 파일 경로 - 적응형 양식의 제출 작업을 구성하여 AEM Workflow를 호출하는 경우 적응형 양식에 지정한 위치와 동일합니다. 워크플로우의 페이로드와 관련하여 첨부 파일을 AEM에 저장할 폴더의 이름입니다.

* saveToLocation - 첨부 파일을 AEM 서버의 파일 시스템에 저장할 위치입니다.

이 두 값은 워크플로우 구성 요소의 대화 상자를 사용하여 프로세스 인수로 전달됩니다

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder 서비스는 attachmentsPath 폴더 아래에 있는 nt:file 유형의 노드를 쿼리하는 데 사용됩니다. 나머지 코드는 검색 결과를 반복하여 Document 개체를 만들고 파일 시스템에 저장합니다


>[!NOTE]
>
>AEM Forms에만 사용되는 Document 개체를 사용하고 있으므로 maven 프로젝트에 aemfd-client-sdk 종속성을 포함해야 합니다.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 빌드 및 배포

[여기에 설명된 대로 번들을 빌드합니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[번들이 배포되어 활성 상태인지 확인합니다.](http://localhost:4502/system/console/bundles)
