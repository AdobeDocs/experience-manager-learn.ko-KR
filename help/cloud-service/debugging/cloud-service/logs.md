---
title: 로그
description: 로그는 AEMas a Cloud Service 에서 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 277
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# 로그를 사용하여 AEM as a Cloud Service 디버깅

로그는 AEMas a Cloud Service 에서 AEM 응용 프로그램을 디버깅하기 위해 최전방 역할을 하지만 배포된 AEM 응용 프로그램의 적절한 로깅에 의존합니다.

지정된 환경의 AEM 서비스(작성자, 게시/Dispatcher 게시)에 대한 모든 로그 작업은 해당 서비스 내의 다른 Pod에서 로그 문을 생성하는 경우에도 단일 로그 파일로 통합됩니다.

Pod ID는 각 로그 문에 제공되며 로그 문을 필터링하거나 정렬할 수 있습니다. Pod ID의 형식은 다음과 같습니다.

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 예: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 사용자 지정 로그 파일

AEM as a Cloud Service은 사용자 정의 로그 파일을 지원하지 않지만 사용자 정의 로깅을 지원합니다.

Java 로그를 AEM에서 as a Cloud Service으로 사용 가능(를 통해) [Cloud Manager](#cloud-manager) 또는 [ADOBE I/O CLI](#aio)), 사용자 지정 로그 문은 `error.log`. 다음과 같이 사용자 정의 명명된 로그에 기록된 로그 `example.log`는 AEM에서 as a Cloud Service으로 액세스할 수 없습니다.

로그는 `error.log` 응용 프로그램의 Sling LogManager OSGi 구성 속성 사용 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 파일.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Author 및 Publish 서비스 로그

AEM Author 및 Publish 서비스는 모두 AEM 런타임 서버 로그를 제공합니다.

+ `aemerror` 는 Java 오류 로그입니다( 다음에서 찾을 수 있음). `/crx-quickstart/logs/error.log` (AEM SDK 로컬 빠른 시작). 다음은 [권장 로그 수준](#log-levels) 환경 유형별 사용자 지정 로거의 경우:
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemaccess` 세부 정보와 함께 AEM 서비스에 대한 HTTP 요청을 나열합니다.
+ `aemrequest` AEM 서비스에 대한 HTTP 요청과 해당 HTTP 응답을 나열합니다.

## AEM 게시 Dispatcher 로그

AEM Publish Dispatcher만 Apache 웹 서버 및 Dispatcher 로그를 제공합니다. 이러한 측면이 AEM 게시 계층에만 존재하며 AEM 작성자 계층에는 존재하지 않기 때문입니다.

+ `httpdaccess` AEM 서비스의 Apache 웹 서버/Dispatcher에 대한 HTTP 요청을 나열합니다.
+ `httperror`  apache 웹 서버의 로그 메시지를 나열하고 다음과 같이 지원되는 Apache 모듈 디버깅에 대한 도움말을 봅니다. `mod_rewrite`.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`
+ `aemdispatcher` 은 캐시 메시지에서 필터링 및 제공 등 Dispatcher 모듈의 로그 메시지를 나열합니다.
   + 개발: `DEBUG`
   + 단계: `WARN`
   + 프로덕션: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager를 사용하면 환경의 로그 다운로드 작업을 통해 일별로 로그를 다운로드할 수 있습니다.

![Cloud Manager - 로그 다운로드](./assets/logs/download-logs.png)

이러한 로그는 모든 로그 분석 도구를 통해 다운로드 및 검사할 수 있습니다.

## Cloud Manager 플러그인으로 CLI Adobe I/O{#aio}

Adobe Cloud Manager는 를 통해 AEM as a Cloud Service 로그에 액세스를 지원합니다. [ADOBE I/O CLI](https://github.com/adobe/aio-cli) (으)로 [Adobe I/O CLI용 Cloud Manager 플러그인](https://github.com/adobe/aio-cli-plugin-cloudmanager).

첫 번째, [cloud Manager 플러그인으로 Adobe I/O 설정](../../local-development-environment/development-tools.md#aio-cli).

관련 프로그램 ID 및 환경 ID가 식별되었는지 확인하고 다음을 사용합니다 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) 다음에 사용되는 로그 옵션을 나열하려면 [테일](#aio-cli-tail-logs) 또는 [다운로드](#aio-cli-download-logs) 로그.

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

Adobe I/O CLI는 AEMas a Cloud Service 에서 [테일 로그](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 명령입니다. 테일링은 AEM as a Cloud Service 환경에서 작업이 수행되므로 실시간 로그 활동을 보는 데 유용합니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

기타 명령줄 도구, 예: `grep` 과 함께 사용할 수 있습니다. `tail-logs` 관심 있는 로그 설명을 격리하려면 다음을 수행합니다.

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...에서 생성된 로그 문만 표시합니다. `com.example.MySlingModel` 또는 해당 문자열을 포함합니다.

### 로그 다운로드{#aio-cli-download-logs}

Adobe I/O CLI는 AEMas a Cloud Service 에서 로그를 다운로드하는 기능을 제공합니다. [download-log](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) 명령입니다. 이렇게 하면 Cloud Manager 웹 UI에서 로그를 다운로드하는 것과 동일한 최종 결과를 제공하며 차이점은 다음과 같습니다. `download-logs` 명령은 요청한 일 수에 따라 여러 날에 걸쳐 로그를 통합합니다.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 로그 이해

AEMas a Cloud Service 의 로그에는 로그 문을 기록하는 여러 Pod가 있습니다. 여러 AEM 인스턴스가 동일한 로그 파일에 기록하므로 디버깅하는 동안 분석 방법을 이해하고 소음을 줄이는 것이 중요합니다. 이를 설명하기 위해 다음 항목을 참조하십시오 `aemerror` 로그 스니펫이 사용됩니다.

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

각 환경에 대해 잘 알려진 정적 Java 로그 수준을 설정하는 대신 AEM을 Cloud Service으로 사용합니다. [환경별 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) 를 통해 값을 동적으로 변경할 수 있도록 로그 수준을 매개 변수화합니다. [Cloud Manager 플러그인으로 CLI Adobe I/O](#aio-cli).

이렇게 하려면 환경별 변수 자리 표시자를 사용하도록 로깅 OSGi 구성을 업데이트해야 합니다. [기본값](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 의 로그 수준은 다음과 같이 설정되어야 합니다. [Adobe 권장 사항](#log-levels). 예:

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

+ [제한된 수의 환경 변수가 허용됩니다.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)를 추가하고, 로그 수준을 관리하기 위한 변수를 만들면 이 중 하나를 사용합니다.
+ 환경 변수는 를 통해 프로그래밍 방식으로 관리할 수 있습니다. [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [ADOBE I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), 및 [Cloud Manager API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ 환경 변수에 대한 변경 사항은 지원되는 도구에서 수동으로 재설정해야 합니다. 프로덕션과 같은 높은 트래픽 환경을 덜 자세한 로그 수준으로 재설정하는 것을 잊으면 로그가 플러시되고 AEM 성능에 영향을 줄 수 있습니다.

_Apache 웹 서버 또는 Dispatcher 로그 구성은 OSGi 구성을 통해 구성되지 않으므로 환경별 변수가 작동하지 않습니다._
