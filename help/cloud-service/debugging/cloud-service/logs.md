---
title: 로그
description: 로그는 AEM에서 Cloud Service으로 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: e2473a1584ccf315fffe5b93cb6afaed506fdbce
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 2%

---


# 로그를 사용하여 AEM as a Cloud Service 디버깅

로그는 AEM에서 Cloud Service으로 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.

지정된 환경의 AEM 서비스(작성자, 게시/게시 Dispatcher)에 대한 모든 로그 활동은 해당 서비스 내에서 다른 pod가 로그 문을 생성하더라도 단일 로그 파일에 통합됩니다.

Pod Id는 각 로그 문에 제공되며 로그 문을 필터링하거나 수집할 수 있습니다. Pod Id의 형식은 다음과 같습니다.

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 예: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 사용자 지정 로그 파일

AEM as a Cloud Services은 사용자 지정 로그 파일을 지원하지 않지만 사용자 지정 로깅을 지원합니다.

Java 로그를 AEM에서 Cloud Service([Cloud Manager](#cloud-manager) 또는 [Adobe I/O CLI](#aio)를 통해)로 사용할 수 있도록 하려면 사용자 지정 로그 문을 `error.log`에 작성해야 합니다. 사용자 지정 명명된 로그(예: `example.log`)에 작성된 로그는 AEM에서 Cloud Service으로 액세스할 수 없습니다.

애플리케이션의 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 파일에서 Sling LogManager OSGi 구성 속성을 사용하여 `error.log`에 로그를 쓸 수 있습니다.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM 작성자 및 게시 서비스 로그

AEM 작성자 및 게시 서비스 모두 AEM 런타임 서버 로그를 제공합니다.

+ `aemerror` 는 AEM SDK 로컬 빠른 시작 `/crx-quickstart/logs/error.log` 에 있는 Java 오류 로그입니다. 다음은 환경 유형별 사용자 지정 로거들에 대한 [권장 로그 수준](#log-levels)입니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemaccess` 세부 정보와 함께 AEM 서비스에 대한 HTTP 요청 나열
+ `aemrequest` AEM 서비스에 대한 HTTP 요청 및 해당 HTTP 응답을 나열합니다

## AEM Publish Dispatcher 로그

이러한 측면이 AEM 게시 계층에만 있고 AEM 작성 계층에는 존재하지 않으므로 AEM 게시 디스패처만 Apache 웹 서버 및 Dispatcher 로그를 제공합니다.

+ `httpdaccess` AEM 서비스의 Apache 웹 서버/Dispatcher에 대한 HTTP 요청을 나열합니다.
+ `httperror`  apache 웹 서버의 로그 메시지를 나열하고 지원되는 Apache 모듈(예:  `mod_rewrite`)을 디버깅하는 데 도움이 됩니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemdispatcher` 캐시 메시지에서 필터링 및 서비스를 포함하여 Dispatcher 모듈의 로그 메시지를 나열합니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager에서는 환경의 다운로드 로그 작업을 통해 일별로 로그를 다운로드할 수 있습니다.

![Cloud Manager - 다운로드 로그](./assets/logs/download-logs.png)

모든 로그 분석 도구를 통해 이러한 로그를 다운로드하여 검사할 수 있습니다.

## Cloud Manager 플러그인이 있는 Adobe I/O CLI{#aio}

Adobe Cloud Manager에서는 Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)에 대한 [Cloud Manager 플러그인과 함께 [Adobe I/O CLI](https://github.com/adobe/aio-cli)를 통해 AEM을 Cloud Service 로그으로 액세스할 수 있습니다.

먼저 [Cloud Manager 플러그인](../../local-development-environment/development-tools.md#aio-cli)을 사용하여 Adobe I/O을 설정합니다.

관련 프로그램 ID 및 환경 ID가 식별되었는지 확인하고 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)를 사용하여 [tail](#aio-cli-tail-logs) 또는 [다운로드](#aio-cli-download-logs) 로그에 사용되는 로그 옵션을 나열합니다.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### 로그 지우기{#aio-cli-tail-logs}

Adobe I/O CLI는 [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 명령을 사용하여 AEM as a Cloud Service에서 실시간으로 로그를 추적할 수 있는 기능을 제공합니다. Tailing은 AEM as a Cloud Service 환경에서 작업이 수행되므로 실시간 로그 활동을 보는 데 유용합니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

`grep` 등의 기타 명령줄 도구는 `tail-logs` 과 함께 사용하여 다음과 같이 관심 있는 로그 문을 분리할 수 있습니다.

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...은(는) `com.example.MySlingModel`에서 생성된 로그 명령문만 표시하거나 해당 문자열에 해당 문자열을 포함합니다.

### 로그 다운로드{#aio-cli-download-logs}

Adobe I/O CLI는 [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) 명령을 사용하여 AEM에서 Cloud Service으로 로그를 다운로드할 수 있는 기능을 제공합니다. 이렇게 하면 Cloud Manager 웹 UI에서 로그를 다운로드하는 것과 동일한 종료 결과가 제공되며, 이 경우 요청 받은 로그 수를 기준으로 여러 날에 걸쳐 로그가 통합됩니다. `download-logs` 명령을 사용하면

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 로그 이해

AEM as a Cloud Service에 로그인하면 여러 pod가 로그 명령문에 기록됩니다. 여러 AEM 인스턴스가 동일한 로그 파일에 쓰기 때문에 디버깅하는 동안 분석을 이해하고 노이즈를 줄이는 방법을 이해하는 것이 중요합니다. 설명하기 위해 다음 `aemerror` 로그 코드 조각이 사용됩니다.

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Pod Id를 사용하여 날짜 및 시간 후의 데이터 포인트를 사용하여 로그를 Pod 또는 서비스 내의 AEM 인스턴스에 의해 수집할 수 있으므로 코드 실행을 쉽게 추적하고 이해할 수 있습니다.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 권장 로그 수준{#log-levels}

Cloud Service 환경으로서 AEM당 로그 수준에 대한 Adobe의 일반적인 지침은 다음과 같습니다.

+ 로컬 개발(AEM SDK): `DEBUG`
+ 개발: `DEBUG`
+ 단계: `WARN`
+ 프로덕션: `ERROR`

각 환경 유형에 대해 가장 적절한 로그 수준을 설정하는 것은 AEM as a Cloud Service을 사용하는 것이며, 로그 수준은 코드에서 유지됩니다

+ Java 로그 구성은 OSGi 구성에서 유지됩니다
+ Dispatcher 프로젝트의 Apache 웹 서버 및 Dispatcher 로그 수준

...따라서 배포를 변경해야 합니다.

### Java 로그 수준을 설정할 환경별 변수

각 환경에 대해 잘 알려진 정적 Java 로그 수준을 설정하는 대신, AEM을 Cloud Service의 [환경별 변수](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)로 사용하여 로그 수준을 매개 변수화함으로써 Cloud Manager 플러그인](#aio-cli)이 있는 [Adobe I/O CLI를 통해 값을 동적으로 변경할 수 있습니다.

따라서 환경별 변수 자리 표시자를 사용하려면 로깅 OSGi 구성을 업데이트해야 합니다. [로그 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 수준에 대한 기본값은  [Adobe 권장 사항에 따라 설정되어야 합니다](#log-levels). 예:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

이 접근 방식은 다음과 같은 사항을 고려해야 합니다.

+ [제한된 수의 환경 변수가 허용됩니다](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables). 로그 수준을 관리하기 위해 변수를 만들면 이 변수가 사용됩니다.
+ 환경 변수는 [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 또는 [Cloud Manager HTTP API](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)를 통해서만 프로그래밍 방식으로 관리할 수 있습니다.
+ 환경 변수에 대한 변경 사항은 지원되는 도구에 의해 수동으로 재설정되어야 합니다. 프로덕션과 같은 높은 트래픽 환경을 더 적은 세부 로그 수준으로 재설정하지 않도록 하면 로그가 붓고 AEM 성능에 영향을 줄 수 있습니다.

_Apache 웹 서버 또는 Dispatcher 로그 구성에서는 OSGi 구성을 통해 구성되지 않으므로 환경별 변수가 작동하지 않습니다._
