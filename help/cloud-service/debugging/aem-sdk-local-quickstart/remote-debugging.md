---
title: AEM SDK 원격 디버깅
description: AEM SDK의 로컬 빠른 시작을 사용하면 IDE에서 원격 Java 디버깅을 사용할 수 있으므로 AEM에서 라이브 코드 실행을 단계별로 진행하여 정확한 실행 흐름을 파악할 수 있습니다.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# AEM SDK 원격 디버깅

>[!VIDEO](https://video.tv.adobe.com/v/38164?quality=12&learn=on&captions=kor)

AEM SDK의 로컬 빠른 시작을 사용하면 IDE에서 원격 Java 디버깅을 사용할 수 있으므로 AEM에서 라이브 코드 실행을 단계별로 진행하여 정확한 실행 흐름을 파악할 수 있습니다.

원격 디버거를 AEM에 연결하려면 IDE가 연결할 수 있는 특정 매개 변수(`-agentlib:...`)로 AEM SDK의 로컬 빠른 시작을 시작해야 합니다.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK는 Java 11만 지원합니다
+ `address`은(는) AEM이 원격 디버그 연결을 수신하는 포트를 지정하며 로컬 개발 컴퓨터에서 사용 가능한 포트로 변경할 수 있습니다.
+ 마지막 매개 변수(예: `aem-author-p4502.jar`)은(는) AEM SKD Quickstart Jar입니다. AEM 작성자 서비스(`aem-author-p4502.jar`) 또는 AEM Publish 서비스(`aem-publish-p4503.jar`)일 수 있습니다.


## IDE 설정 지침

대부분의 Java IDE는 Java 프로그램의 원격 디버깅을 지원하지만 각 IDE의 정확한 설정 단계는 다릅니다. 정확한 단계에 대한 IDE의 원격 디버깅 설정 지침을 검토하십시오. 일반적으로 IDE 구성에는 다음이 필요합니다.

+ 호스트 AEM SDK의 로컬 빠른 시작이 `localhost`에서 수신 대기 중입니다.
+ 포트 AEM SDK의 로컬 빠른 시작이 원격 디버그 연결에 대해 수신 대기합니다. 이 포트는 AEM SDK의 로컬 빠른 시작을 시작할 때 `address` 매개 변수로 지정된 포트입니다.
+ 경우에 따라 소스 코드를 원격 디버그에 제공하는 Maven 프로젝트를 지정해야 합니다. 이는 OSGi 번들 Maven 프로젝트입니다.

### 지침 설정

+ [VS 코드 Java 원격 디버거 설정](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA 원격 디버거 설정](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse 원격 디버거 설정](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
