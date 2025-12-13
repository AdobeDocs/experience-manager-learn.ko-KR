---
title: AEM as a Cloud Service의 리더 인스턴스에서 작업을 실행하는 방법
description: AEM as a Cloud Service의 리더 인스턴스에서 작업을 실행하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
exl-id: b8b88fc1-1de1-4b5e-8c65-d94fcfffc5a5
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# AEM as a Cloud Service의 리더 인스턴스에서 작업을 실행하는 방법

AEM as a Cloud Service의 일부로 AEM Author 서비스의 리더 인스턴스에서 작업을 실행하는 방법을 알아보고 한 번만 실행되도록 구성하는 방법을 이해합니다.

슬링 작업은 시스템 또는 사용자가 트리거한 이벤트를 처리하도록 설계된 백그라운드에서 작동하는 비동기 작업입니다. 기본적으로 이러한 작업은 클러스터의 모든 인스턴스(포드)에 균등하게 분배됩니다.

자세한 내용은 [Apache Sling 이벤트 및 작업 처리](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)를 참조하십시오.

## 작업 만들기 및 처리

데모용으로 작업 프로세서에 메시지를 기록하도록 지시하는 간단한 _작업을 만들어 보겠습니다_.

### 작업 만들기

아래 코드를 사용하여 Apache Sling 작업을 _만들기_&#x200B;합니다.

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

위 코드에서 유의할 사항은 다음과 같습니다.

- 작업 페이로드에는 `action` 및 `message` 속성이 두 개 있습니다.
- [JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html)의 `addJob(...)` 메서드를 사용하여 작업이 `wknd/simple/job/topic` 주제에 추가됩니다.

### 작업 처리

아래 코드를 사용하여 위의 Apache Sling 작업을 _처리_&#x200B;합니다.

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

위 코드에서 유의할 사항은 다음과 같습니다.

- `SimpleJobConsumerImpl` 클래스는 `JobConsumer` 인터페이스를 구현합니다.
- `wknd/simple/job/topic` 항목에서 작업을 사용하도록 등록된 서비스입니다.
- `process(...)` 메서드는 작업 페이로드의 `message` 속성을 로깅하여 작업을 처리합니다.

### 기본 작업 처리

위의 코드를 AEM as a Cloud Service 환경에 배포하고 여러 AEM 작성자 JVM이 있는 클러스터로 작동하는 AEM 작성자 서비스에서 실행하면 각 AEM 작성자 인스턴스(pod)에서 작업이 한 번 실행됩니다. 즉, 생성된 작업 수는 pod 수와 일치합니다. RDE가 아닌 환경의 경우 Pod의 수는 항상 두 개 이상이지만 AEM as a Cloud Service의 내부 리소스 관리에 따라 달라집니다.

`wknd/simple/job/topic`이(가) 사용 가능한 모든 인스턴스에 작업을 배포하는 AEM의 기본 큐와 연결되어 있으므로 작업은 각 AEM 작성자 인스턴스(pod)에서 실행됩니다.

이 문제는 작업에서 리소스 또는 외부 서비스 만들기 또는 업데이트와 같은 상태 변경을 담당하는 경우 자주 발생합니다.

AEM 작성자 서비스에서 작업을 한 번만 실행하려면 아래에 설명된 [작업 큐 구성](#how-to-run-a-job-on-the-leader-instance)을 추가하십시오.

[Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager)에서 AEM 작성자 서비스의 로그를 검토하여 확인할 수 있습니다.

![모든 인스턴스에서 작업 처리](./assets/run-job-once/job-processed-by-all-instances.png)


다음이 표시됩니다.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

각 AEM 작성자 인스턴스(`68775db964-nxxcx` 및 `68775db964-r4zk7`)에 대해 하나씩, 각 인스턴스(pod)에서 작업을 처리했음을 나타내는 두 개의 로그 항목이 있습니다.

## 리더 인스턴스에서 작업을 실행하는 방법

AEM 작성자 서비스에서 _한 번만_ 작업을 실행하려면 **주문** 유형의 새 Sling 작업 큐를 만들고 작업 주제(`wknd/simple/job/topic`)를 이 큐와 연결하십시오. 이 구성을 사용하면 리더 AEM 작성자 인스턴스(pod)에서만 작업을 처리할 수 있습니다.

AEM 프로젝트의 `ui.config` 모듈에서 OSGi 구성 파일(`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`)을 만들고 `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author` 폴더에 저장합니다.

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

위 구성에서 중요 사항은 다음과 같습니다.

- 대기열 주제가 `wknd/simple/job/topic`(으)로 설정되어 있습니다.
- 큐 유형이 `ORDERED`(으)로 설정되어 있습니다.
- 최대 병렬 작업 수가 `1`(으)로 설정되어 있습니다.

위의 구성을 배포하면 작업이 리더 인스턴스에서만 처리되어 전체 AEM 작성자 서비스에서 한 번만 실행됩니다.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
