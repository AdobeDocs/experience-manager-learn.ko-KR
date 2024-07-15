---
title: 로그
description: 로그는 AEM as a Cloud Service에서 AEM 애플리케이션을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 애플리케이션의 적절한 로깅에 의존합니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# 로그를 사용하여 AEM as a Cloud Service 디버깅

로그는 AEM as a Cloud Service에서 AEM 애플리케이션을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 애플리케이션의 적절한 로깅에 의존합니다.

지정된 환경의 AEM 서비스(작성자, Publish/Publish Dispatcher)에 대한 모든 로그 작업은 해당 서비스 내의 다른 Pod에서 로그 문을 생성하는 경우에도 단일 로그 파일로 통합됩니다.

Pod ID는 각 로그 문에 제공되며 로그 문을 필터링하거나 정렬할 수 있습니다. Pod ID의 형식은 다음과 같습니다.

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 예: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 사용자 지정 로그 파일

AEM as a Cloud Service은 사용자 정의 로그 파일을 지원하지 않지만 사용자 정의 로깅을 지원합니다.

AEM as a Cloud Service에서 Java 로그를 사용하려면([Cloud Manager](#cloud-manager) 또는 [Adobe I/O CLI](#aio)를 통해) 사용자 지정 로그 문을 `error.log`에 작성해야 합니다. `example.log`과(와) 같이 사용자 지정 명명된 로그에 기록된 로그를 AEM as a Cloud Service에서 액세스할 수 없습니다.

응용 프로그램의 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 파일에서 Sling LogManager OSGi 구성 속성을 사용하여 `error.log`에 로그를 쓸 수 있습니다.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Author 및 Publish 서비스 로그

AEM Author 및 Publish 서비스는 모두 AEM 런타임 서버 로그를 제공합니다.

+ `aemerror`은(는) Java 오류 로그(AEM SDK 로컬 빠른 시작의 `/crx-quickstart/logs/error.log`에 있음)입니다. 다음은 환경 유형별 사용자 지정 로거에 대한 [권장 로그 수준](#log-levels)입니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemaccess`에서 AEM 서비스에 대한 HTTP 요청을 세부 정보와 함께 나열합니다.
+ `aemrequest`은(는) AEM 서비스에 대한 HTTP 요청과 해당 HTTP 응답을 나열합니다

## AEM Publish Dispatcher 로그

Apache 웹 서버 및 Dispatcher 로그는 AEM Publish Dispatcher에서만 제공합니다. 이러한 측면은 AEM Publish 계층에만 존재하며 AEM 작성자 계층에는 존재하지 않습니다.

+ `httpdaccess`은(는) AEM 서비스의 Apache 웹 서버/Dispatcher에 대한 HTTP 요청을 나열합니다.
+ `httperror`은(는) Apache 웹 서버의 로그 메시지를 나열하며 `mod_rewrite`과(와) 같이 지원되는 Apache 모듈 디버깅에 대한 도움말을 제공합니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemdispatcher`은(는) 캐시 메시지에서 필터링 및 서비스 등 Dispatcher 모듈의 로그 메시지를 나열합니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager을 사용하면 환경의 로그 다운로드 작업을 통해 일별로 로그를 다운로드할 수 있습니다.

![Cloud Manager - 로그 다운로드](./assets/logs/download-logs.png)

이러한 로그는 모든 로그 분석 도구를 통해 다운로드 및 검사할 수 있습니다.

## Cloud Manager 플러그인으로 CLI Adobe I/O{#aio}

Adobe Cloud Manager은 [Adobe I/O CLI용 Cloud Manager 플러그인](https://github.com/adobe/aio-cli-plugin-cloudmanager)을 사용하여 [Adobe I/O CLI](https://github.com/adobe/aio-cli)를 통해 AEM as a Cloud Service 로그에 액세스할 수 있도록 지원합니다.

먼저 [Cloud Manager 플러그인으로 Adobe I/O을 설정합니다](../../local-development-environment/development-tools.md#aio-cli).

관련 프로그램 ID 및 환경 ID가 식별되었는지 확인하고 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)을(를) 사용하여 [tail](#aio-cli-tail-logs) 또는 [download](#aio-cli-download-logs) 로그에 사용되는 로그 옵션을 나열합니다.

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

### 테일링 로그{#aio-cli-tail-logs}

Adobe I/O CLI는 [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 명령을 사용하여 AEM as a Cloud Service에서 실시간으로 로그를 추적하는 기능을 제공합니다. 테일링은 AEM as a Cloud Service 환경에서 작업이 수행되므로 실시간 로그 활동을 보는 데 유용합니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

`grep`과(와) 같은 다른 명령줄 도구를 `tail-logs`과(와) 함께 사용하여 다음과 같은 관심 로그 문을 격리할 수 있습니다.

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... `com.example.MySlingModel`에서 생성된 로그 문만 표시하거나 해당 문자열을 포함합니다.

### 로그 다운로드{#aio-cli-download-logs}

Adobe I/O CLI는 [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) 명령을 사용하여 AEM as a Cloud Service에서 로그를 다운로드하는 기능을 제공합니다. 이렇게 하면 Cloud Manager 웹 UI에서 로그를 다운로드하는 것과 동일한 최종 결과를 얻을 수 있습니다. 차이점은 `download-logs` 명령이 요청한 일 수에 따라 여러 날에 걸쳐 로그를 통합한다는 것입니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 로그 이해

AEM as a Cloud Service의 로그에는 로그 문을 기록하는 여러 Pod가 있습니다. 여러 AEM 인스턴스가 동일한 로그 파일에 기록하므로 디버깅하는 동안 분석 방법을 이해하고 소음을 줄이는 것이 중요합니다. 설명을 위해 다음 `aemerror` 로그 코드 조각이 사용됩니다.

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

날짜 및 시간 이후의 데이터 포인트인 Pod ID를 사용하여 Pod 또는 서비스 내의 AEM 인스턴스별로 로그를 수집할 수 있으므로 코드 실행을 더 쉽게 추적하고 이해할 수 있습니다.

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

AEM as a Cloud Service 환경당 로그 수준에 대한 Adobe의 일반적인 지침은 다음과 같습니다.

+ 로컬 개발(AEM SDK): `DEBUG`
+ 개발: `DEBUG`
+ 단계: `WARN`
+ 프로덕션: `ERROR`

각 환경 유형에 가장 적합한 로그 수준을 설정하는 것은 AEM as a Cloud Service을 사용하는 것이며, 로그 수준은 코드에서 유지됩니다

+ Java 로그 구성은 OSGi 구성에서 유지 관리됩니다.
+ Dispatcher 프로젝트의 Apache 웹 서버 및 Dispatcher 로그 수준

...따라서 변경하려면 배포가 필요합니다.

### Java 로그 수준을 설정하는 환경별 변수

각 환경에 대해 잘 알려진 정적 Java 로그 수준을 설정하는 대신 AEM을 Cloud Service의 [환경 특정 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)(으)로 사용하여 로그 수준을 매개 변수화하여 [Cloud Manager 플러그인과 함께 Adobe I/O CLI](#aio-cli)를 통해 값을 동적으로 변경할 수 있습니다.

이렇게 하려면 환경별 변수 자리 표시자를 사용하도록 로깅 OSGi 구성을 업데이트해야 합니다. 로그 수준에 대한 [기본값](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values)은(는) [Adobe 권장 사항](#log-levels)에 따라 설정해야 합니다. 예:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

이 접근 방식에는 다음과 같은 단점이 있습니다.

+ [제한된 수의 환경 변수가 허용됩니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables). 로그 수준을 관리하기 위한 변수를 만들면 하나가 사용됩니다.
+ 환경 변수는 [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 및 [Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)를 통해 프로그래밍 방식으로 관리할 수 있습니다.
+ 환경 변수에 대한 변경 사항은 지원되는 도구에서 수동으로 재설정해야 합니다. 프로덕션과 같은 높은 트래픽 환경을 덜 자세한 로그 수준으로 재설정하는 것을 잊으면 로그가 플러시되고 AEM 성능에 영향을 줄 수 있습니다.

_Apache 웹 서버 또는 Dispatcher 로그 구성은 OSGi 구성을 통해 구성되지 않았으므로 환경별 변수가 작동하지 않습니다._
