---
title: AEM SDK의 원격 디버깅
description: AEM SDK의 로컬 quickstart를 사용하면 IDE에서 원격 Java 디버깅을 수행할 수 있으므로 AEM에서 라이브 코드 실행을 단계별로 진행하여 정확한 실행 흐름을 파악할 수 있습니다.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# AEM SDK의 원격 디버깅

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDK의 로컬 quickstart를 사용하면 IDE에서 원격 Java 디버깅을 수행할 수 있으므로 AEM에서 라이브 코드 실행을 단계별로 진행하여 정확한 실행 흐름을 파악할 수 있습니다.

원격 디버거를 AEM에 연결하려면 AEM SDK의 로컬 quickstart를 IDE가 해당 디버거에 연결할 수 있도록 허용하는 특정 매개 변수(`-agentlib:...`)로 시작해야 합니다.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` AEM이 원격 디버그 연결을 수신 대기하며 로컬 개발 컴퓨터의 사용 가능한 포트로 변경할 수 있는 포트를 지정합니다.
+ 마지막 매개 변수(예: `aem-author-p4502.jar`)은 AEM SKD 빠른 시작 병입니다. AEM 작성자 서비스(`aem-author-p4502.jar`) 또는 AEM 게시 서비스(`aem-publish-p4503.jar`)일 수 있습니다.

## IDE 설정 지침

대부분의 Java IDE는 Java 프로그램의 원격 디버깅을 지원하지만 각 IDE의 정확한 설정 단계는 다릅니다. 정확한 단계에 대해서는 IDE의 원격 디버깅 설정 지침을 검토하십시오. 일반적으로 IDE 구성에는 다음이 필요합니다.

+ 호스트 AEM SDK의 로컬 quickstart가 수신 대기 중입니다(예: `localhost`).
+ AEM SDK의 로컬 quickstart 포트가 원격 디버그 연결에 대해 수신 대기 중입니다. 이 포트는 AEM SDK의 로컬 quickstart를 시작할 때 `address` 매개 변수에 지정된 포트입니다.
+ 경우에 따라 소스 코드를 원격 디버깅에 제공하는 Maven 프로젝트를 지정해야 합니다.OSGi bundle maven 프로젝트 프로젝트입니다.

### 지침 설정

+ [VS 코드 Java 원격 디버거 설정](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA 원격 디버거 설정](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse 원격 디버거 설정](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
