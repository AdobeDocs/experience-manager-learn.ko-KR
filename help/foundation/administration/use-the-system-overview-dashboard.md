---
title: AEM에서 시스템 개요 대시보드 사용
description: 이전 버전의 AEM 관리자는 AEM 인스턴스의 전체 상황을 파악하기 위해 여러 위치를 확인해야 했습니다. 시스템 개요는 AEM 인스턴스의 구성, 하드웨어 및 상태를 모두 하나의 대시보드에서 자세히 확인하여 이 문제를 해결하도록 합니다.
version: 6.4, 6.5
feature: null
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# 시스템 개요 대시보드 사용

Adobe Experience Manager(AEM) [!UICONTROL 시스템 개요] 는 단일 대시보드에서 모두 AEM 인스턴스의 구성, 하드웨어 및 상태를 자세히 볼 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 시스템 개요는 다음 위치에서 액세스할 수 있습니다. **AEM 시작** > **[!UICONTROL 도구]** > **[!UICONTROL 작업]** **[!UICONTROL >시스템 개요]**

   에서 바로 **[!DNL <server-host>/libs/granite/operations/content/systemoverview.html]**

1. 시스템 개요 [!UICONTROL 의] 정보는 [!UICONTROL 다운로드] 단추를 클릭하여 내보낼 수 있습니다. 정보는 다음 종단점을 통해 [!DNL REST] 노출됩니다.
1. 다음은 [!UICONTROL 시스템 개요에서 내보낸 JSON의 샘플 출력입니다].

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
