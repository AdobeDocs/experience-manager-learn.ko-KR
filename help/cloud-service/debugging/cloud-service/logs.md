---
title: 로그
description: 로그는 AEM에서 Cloud Service으로 AEM 애플리케이션을 디버깅하기 위해 최전선으로 사용되지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: 개발
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '995'
ht-degree: 2%

---


# 로그를 사용하여 AEM을 Cloud Service으로 디버깅

로그는 AEM에서 Cloud Service으로 AEM 애플리케이션을 디버깅하기 위해 최전선으로 사용되지만 배포된 AEM 응용 프로그램에서 적절한 로깅에 의존합니다.

지정된 환경의 AEM 서비스(작성자, 게시/게시 디스패처)에 대한 모든 로그 활동은 해당 서비스 내의 다른 창이 로그 문을 생성하더라도 단일 로그 파일로 통합됩니다.

창 ID는 각 로그 문에 제공되므로 로그 문을 필터링하거나 수집할 수 있습니다. 창 ID 형식은 다음과 같습니다.

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 예: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 사용자 정의 로그 파일

Cloud Services은 사용자 정의 로그 파일을 지원하지 않지만 사용자 정의 로깅을 지원합니다.

AEM에서 Java 로그를 Cloud Service([Cloud Manager](#cloud-manager) 또는 [Adobe I/O CLI](#aio)를 통해)로 사용할 수 있도록 하려면 사용자 정의 로그 문을 `error.log`에 작성해야 합니다. `example.log` 등의 사용자 지정 이름 있는 로그에 기록된 로그는 AEM에서 Cloud Service으로 액세스할 수 없습니다.

## AEM 작성자 및 게시 서비스 로그

AEM 작성자 및 게시 서비스 모두 AEM 런타임 서버 로그를 제공합니다.

+ `aemerror` 는 AEM SDK 로컬 quickstart `/crx-quickstart/error.log` 의 Java 오류 로그입니다. 다음은 환경 유형별로 사용자 정의 로거 작업에 대한 [권장 로그 수준](#log-levels)입니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemaccess` 자세한 내용은 AEM 서비스에 대한 HTTP 요청을 나열합니다.
+ `aemrequest` AEM 서비스에 대한 HTTP 요청 및 해당 HTTP 응답을 나열합니다.

## AEM 게시 디스패처 로그

AEM 게시 디스패처만 Apache 웹 서버 및 디스패처 로그를 제공합니다. 이러한 양상은 AEM 게시 계층에만 존재하며 AEM 작성자 계층에는 없습니다.

+ `httpdaccess` 는 AEM 서비스의 Apache 웹 서버/Dispatcher에 대한 HTTP 요청을 나열합니다.
+ `httperror`  에는 Apache 웹 서버의 로그 메시지와, 지원되는 Apache 모듈(예:  `mod_rewrite`디버깅)의 도움말이 나와 있습니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemdispatcher` 캐시 메시지의 필터링 및 제공을 포함하여 Dispatcher 모듈의 로그 메시지를 나열합니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager를 사용하면 환경의 [다운로드 로그] 작업을 통해 일별로 로그를 다운로드할 수 있습니다.

![Cloud Manager - 다운로드 로그](./assets/logs/download-logs.png)

이러한 로그는 로그 분석 도구를 통해 다운로드 및 검사할 수 있습니다.

## Cloud Manager 플러그인이 포함된 Adobe I/O CLI{#aio}

Adobe Cloud Manager는 Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)용 [Cloud Manager 플러그인과 함께 [Adobe I/O CLI](https://github.com/adobe/aio-cli)를 통해 AEM을 Cloud Service 로그으로 액세스할 수 있도록 지원합니다.

먼저 [클라우드 관리자 플러그인](../../local-development-environment/development-tools.md#aio-cli)을 사용하여 Adobe I/O을 설정합니다.

관련 프로그램 ID 및 환경 ID가 식별되었는지 확인하고 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)을 사용하여 [꼬리](#aio-cli-tail-logs) 또는 [다운로드](#aio-cli-download-logs) 로그에 사용되는 로그 옵션을 나열합니다.

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

### 로그 기록{#aio-cli-tail-logs}

Adobe I/O CLI는 [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 명령을 사용하여 Cloud Service에서 실시간으로 로그를 추적하는 기능을 제공합니다. 추적 기능은 Cloud Service 환경으로 AEM에서 작업이 수행되므로 실시간 로그 활동을 보는 데 유용합니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

`grep`과 같은 다른 명령줄 도구는 `tail-logs`과 함께 사용하여 관심 있는 로그 문을 분리할 수 있습니다. 예를 들면 다음과 같습니다.

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... `com.example.MySlingModel`에서 생성된 로그 명령문만 표시하거나 해당 문자열을 포함합니다.

### 로그 다운로드 중{#aio-cli-download-logs}

Adobe I/O CLI는 [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) 명령을 사용하여 AEM에서 로그를 Cloud Service으로 다운로드할 수 있는 기능을 제공합니다. 이렇게 하면 Cloud Manager 웹 UI에서 로그를 다운로드하는 것과 동일한 최종 결과를 얻을 수 있으며, 이 차이는 `download-logs` 명령이 요청한 로그 요청 일수에 따라 여러 날 간에 로그를 통합하는 것과 같습니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 로그 이해

Cloud Service으로 AEM에 로그인하면 여러 창에 로그 명령문이 기록됩니다. 여러 AEM 인스턴스가 동일한 로그 파일에 기록되므로 디버깅하는 동안 분석 방법을 이해하고 노이즈를 줄이는 방법을 이해하는 것이 중요합니다. 설명하기 위해 다음 `aemerror` 로그 조각이 사용됩니다.

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

날짜 및 시간 이후의 데이터 포인트, 로그를 서비스 내 AEM 인스턴스로 수집할 수 있으므로 코드 실행을 쉽게 추적하고 이해할 수 있습니다.

__Pod cm-p12345-e56789-aem-author-abcdefg-111__

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

+ 로컬 개발(AEM SDK):`DEBUG`
+ 개발: `DEBUG`
+ 단계: `WARN`
+ 프로덕션: `ERROR`

각 환경 유형에 가장 적절한 로그 수준을 설정하는 것은 Cloud Service으로 AEM을 사용하는 것이며, 로그 수준은 코드에서 유지 관리됩니다

+ OSGi 구성에서 Java 로그 구성 유지
+ Dispatcher 프로젝트의 Apache 웹 서버 및 Dispatcher 로그 수준

...따라서 배포를 변경해야 합니다.

### Java 로그 수준을 설정하는 환경별 변수

각 환경에 대해 정적으로 잘 알려진 Java 로그 수준을 설정하는 방법은 AEM을 Cloud Service [환경별 변수](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)로 사용하여 로그 수준을 매개 변수화하여 Cloud Manager 플러그인](#aio-cli)이 있는 [Adobe I/O CLI를 통해 값을 동적으로 변경할 수 있도록 하는 것입니다.

따라서 환경별 변수 자리 표시자를 사용하려면 로깅 OSGi 구성을 업데이트해야 합니다. [로그 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 수준의 기본 값은  [Adobe 권장 사항에 따라 설정해야 합니다](#log-levels). 예:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

이 접근 방식에는 고려해야 할 사항이 있습니다.

+ [제한된 수의 환경 변수가 허용됩니다](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables). 로그 수준을 관리하기 위한 변수를 만들면 로그 변수가 사용됩니다.
+ 환경 변수는 [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 또는 [Cloud Manager HTTP API](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)를 통해서만 프로그래밍 방식으로 관리할 수 있습니다.
+ 환경 변수에 대한 변경 사항은 지원되는 도구를 통해 수동으로 재설정해야 합니다. 프로덕션과 같은 높은 트래픽 환경을 더 자세한 로그 수준으로 재설정하는 것을 잊어버리면 로그가 범람하고 AEM 성능에 영향을 줄 수 있습니다.

_Apache 웹 서버 또는 Dispatcher 로그 구성에는 OSGi 구성을 통해 구성되지 않으므로 환경별 변수가 작동하지 않습니다._