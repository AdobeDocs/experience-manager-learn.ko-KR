---
title: AEM as a Cloud Service 개발을 위한 Dispatcher 도구 설정
description: AEM SDK의 Dispatcher 도구를 사용하면 로컬에서 Dispatcher을 쉽게 설치, 실행 및 해결할 수 있으므로 Adobe Experience Manager(AEM) 프로젝트의 로컬 개발을 용이하게 할 수 있습니다.
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 4%

---

# 로컬 Dispatcher 도구 설정 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="로컬 Dispatcher 도구"
>abstract="Dispatcher는 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 설정의 일부여야 합니다. AEM as a Cloud Service SDK에는 Dispatcher를 로컬에서 쉽게 구성, 유효성 검사 및 시뮬레이션할 수 있는 권장 Dispatcher 도구 버전이 포함되어 있습니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=ko" text="클라우드의 Dispatcher"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=ko" text="AEM as a Cloud Service SDK 다운로드"

Adobe Experience Manager(AEM)의 Dispatcher은 CDN과 AEM Publish 계층 간에 보안 및 성능 계층을 제공하는 Apache HTTP 웹 서버 모듈입니다. Dispatcher은 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 설정의 일부여야 합니다.

AEM as a Cloud Service SDK에는 Dispatcher을 로컬에서 구성 및 시뮬레이트할 수 있도록 해주는 권장 Dispatcher 도구 버전이 포함되어 있습니다. Dispatcher 도구는 다음과 같이 구성됩니다.

+ `.../dispatcher-sdk-x.x.x/src`에 있는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일의 기본 집합
+ `.../dispatcher-sdk-x.x.x/bin/validate`에 있는 구성 검사기 CLI 도구
+ `.../dispatcher-sdk-x.x.x/bin/validator`에 있는 구성 생성 CLI 도구
+ `.../dispatcher-sdk-x.x.x/bin/docker_run`에 있는 구성 배포 CLI 도구
+ `.../dispatcher-sdk-x.x.x/bin/update_maven`에 있는 CLI 도구를 덮어쓰는 변경 불가능한 구성 파일
+ Dispatcher 모듈로 Apache HTTP 웹 서버를 실행하는 도커 이미지

`~`은(는) 사용자 디렉터리의 약어로 사용됩니다. Windows에서는 `%HOMEPATH%`에 해당합니다.

>[!NOTE]
>
> 이 페이지의 비디오는 macOS에 기록되었습니다. Windows 사용자는 따를 수 있지만 각 비디오와 함께 제공되는 동등한 Dispatcher 도구 Windows 명령을 사용합니다.

## 사전 요구 사항

1. Windows 사용자는 Windows 10 Professional(또는 Docker를 지원하는 버전)을 사용해야 합니다
1. 로컬 개발 컴퓨터에 [Experience Manager Publish Quickstart Jar](./aem-runtime.md)을(를) 설치합니다.

+ 로컬 AEM Publish 서비스에 최신 [AEM 참조 웹 사이트](https://github.com/adobe/aem-guides-wknd/releases)를 설치할 수도 있습니다. 이 웹 사이트는 이 자습서에서 작동하는 Dispatcher을 시각화하는 데 사용됩니다.

1. 로컬 개발 컴퓨터에 최신 버전의 [Docker](https://www.docker.com/)&#x200B;(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)을 설치하고 시작합니다.

## Dispatcher 도구 다운로드 (AEM SDK의 일부)

AEM as a Cloud Service SDK 또는 AEM SDK에는 개발을 위해 로컬에서 Dispatcher 모듈을 사용하여 Apache HTTP 웹 서버를 실행하는 데 사용되는 Dispatcher 도구와 호환되는 QuickStart Jar가 포함되어 있습니다.

AEM as a Cloud Service SDK이 [로컬 AEM 런타임을 설정](./aem-runtime.md)하는 데 이미 다운로드된 경우 다시 다운로드할 필요가 없습니다.

1. Adobe ID으로 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=입니다.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)에 로그인합니다.
   + AEM as a Cloud Service SDK을 다운로드하려면 Adobe 조직 __must__&#x200B;을(를) AEM as a Cloud Service에 대해 프로비저닝해야 합니다.
1. 다운로드할 최신 __AEM SDK__ 결과 행을 클릭합니다.

## AEM SDK zip에서 Dispatcher 도구 추출

>[!TIP]
>
> Windows 사용자는 로컬 Dispatcher 도구가 들어 있는 폴더의 경로에 공백이나 특수 문자를 사용할 수 없습니다. 경로에 공백이 있으면 `docker_run.cmd`이(가) 실패합니다.

Dispatcher 도구 버전은 AEM SDK과 다릅니다. Dispatcher 도구 버전이 AEM as a Cloud Service 버전과 일치하는 AEM SDK 버전을 통해 제공되는지 확인합니다.

1. 다운로드한 `aem-sdk-xxx.zip` 파일 압축 풀기
1. `~/aem-sdk/dispatcher`에 Dispatcher 도구 압축 풀기

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

`aem-sdk-dispatcher-tools-x.x.x-windows.zip`의 압축을 `C:\Users\<My User>\aem-sdk\dispatcher`에 풉니다(필요한 경우 누락된 폴더를 만듭니다).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

아래에 실행된 모든 명령은 현재 작업 디렉터리에 확장 중인 Dispatcher 도구 내용이 포함되어 있다고 가정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*이 비디오에서는 설명 목적으로 macOS을 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다.*

## Dispatcher 구성 파일 이해

>[!TIP]
> [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)에서 만든 Experience Manager 프로젝트는 이 Dispatcher 구성 파일 집합에 미리 채워져 있으므로 Dispatcher 도구 src 폴더에서 복사할 필요가 없습니다.

Dispatcher 도구는 로컬 개발을 포함하여 모든 환경에 대한 동작을 정의하는 일련의 Apache HTTP 웹 서버 및 Dispatcher 구성 파일을 제공합니다.

이러한 파일은 Experience Manager Maven 프로젝트에 없는 경우 `dispatcher/src` 폴더로 Experience Manager Maven 프로젝트에 복사됩니다.

구성 파일에 대한 자세한 설명은 압축을 푼 Dispatcher 도구에서 `dispatcher-sdk-x.x.x/docs/Config.html`(으)로 사용할 수 있습니다.

## 구성 유효성 검사

선택적으로 `httpd -t`을(를) 통해 Dispatcher 및 Apache 웹 서버 구성을 `validate` 스크립트를 사용하여 확인할 수 있습니다(`validator` 실행 파일과 혼동하지 않도록 함). `validate` 스크립트는 `validator`의 [3단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=ko)를 편리하게 실행할 수 있는 방법을 제공합니다.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## 로컬에서 Dispatcher 실행

AEM Dispatcher은 `src` Dispatcher 및 Apache 웹 서버 구성 파일에 대해 Docker를 사용하여 로컬로 실행됩니다.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run`을(를) 수동으로 종료했다가 다시 시작하지 않아도 구성 파일이 변경될 때 다시 로드되므로 `docker_run_hot_reload` 실행 파일이 `docker_run`보다 선호됩니다. 또는 `docker_run`을(를) 사용할 수 있지만 구성 파일이 변경되면 `docker_run`을(를) 수동으로 종료했다가 다시 시작해야 합니다.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run`을(를) 수동으로 종료했다가 다시 시작하지 않아도 구성 파일이 변경될 때 다시 로드되므로 `docker_run_hot_reload` 실행 파일이 `docker_run`보다 선호됩니다. 또는 `docker_run`을(를) 사용할 수 있지만 구성 파일이 변경되면 `docker_run`을(를) 수동으로 종료했다가 다시 시작해야 합니다.

>[!ENDTABS]

`<aem-publish-host>`을(를) `host.docker.internal`(으)로 설정할 수 있습니다. 특수 DNS 이름 Docker는 호스트 컴퓨터의 IP로 확인되는 컨테이너에 제공됩니다. `host.docker.internal`이(가) 해결되지 않으면 아래의 [문제 해결](#troubleshooting-host-docker-internal) 섹션을 참조하십시오.

예를 들어 Dispatcher 도구에서 제공하는 기본 구성 파일을 사용하여 Dispatcher Docker 컨테이너를 시작하려면 다음을 수행하십시오.

Dispatcher 구성 src 폴더의 경로를 제공하는 Dispatcher Docker 컨테이너를 시작합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

포트 4503에서 로컬로 실행되는 AEM as a Cloud Service SDK의 게시 서비스는 `http://localhost:8080`의 Dispatcher을 통해 사용할 수 있습니다.

Experience Manager 프로젝트의 Dispatcher 구성에 대해 Dispatcher 도구를 실행하려면 프로젝트의 `dispatcher/src` 폴더를 가리킵니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher 도구 로그

Dispatcher 로그는 HTTP 요청이 차단되었는지 여부와 그 이유를 로컬 개발 중에 이해하는 데 유용합니다. 환경 매개 변수를 사용하여 `docker_run`의 실행을 접두사로 지정하여 로그 수준을 설정할 수 있습니다.

`docker_run`이(가) 실행되면 Dispatcher 도구 로그가 표준으로 내보내집니다.

Dispatcher 디버깅에 유용한 매개 변수는 다음과 같습니다.

+ `DISP_LOG_LEVEL=Debug` Dispatcher 모듈 로깅을 디버그 수준으로 설정합니다.
   + 기본값은 `Warn`입니다.
+ `REWRITE_LOG_LEVEL=Debug`이(가) Apache HTTP 웹 서버 재작성 모듈 로깅을 디버그 수준으로 설정합니다.
   + 기본값은 `Warn`입니다.
+ `DISP_RUN_MODE`은(는) Dispatcher 환경의 &quot;실행 모드&quot;를 설정하여 해당 실행 모드 Dispatcher 구성 파일을 로드합니다.
   + 기본값은 `dev`입니다.
+ 유효한 값: `dev`, `stage` 또는 `prod`

하나 이상의 매개 변수를 `docker_run`에 전달할 수 있습니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### 로그 파일 액세스

Apache 웹 서버 및 AEM Dispatcher 로그는 Docker 컨테이너에서 직접 액세스할 수 있습니다.

+ [Docker 컨테이너의 로그 액세스](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [로컬 파일 시스템에 Docker 로그 복사](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher 도구를 업데이트해야 하는 경우{#dispatcher-tools-version}

Dispatcher Tools 버전은 Experience Manager보다 덜 자주 증가하므로 Dispatcher Tools는 로컬 개발 환경에서 더 적은 업데이트를 필요로 합니다.

권장되는 Dispatcher 도구 버전은 Experience Manager as a Cloud Service 버전과 일치하는 AEM as a Cloud Service SDK과 번들로 제공되는 버전입니다. AEM as a Cloud Service 버전은 [Cloud Manager](https://my.cloudmanager.adobe.com/)을 통해 찾을 수 있습니다.

+ __Cloud Manager > 환경__, __AEM 릴리스__ 레이블에 지정된 환경당

![Experience Manager 버전](./assets/dispatcher-tools/aem-version.png)

*Dispatcher 도구 버전이 Experience Manager 버전과 일치하지 않습니다.*

## Apache 및 Dispatcher 구성의 기본 세트를 업데이트하는 방법

Apache 및 Dispatcher 구성의 기본 세트가 정기적으로 향상되고 AEM as a Cloud Service SDK 버전과 함께 릴리스됩니다. 기본 구성 개선 사항을 AEM 프로젝트에 통합하고 [로컬 유효성 검사](#validate-configurations) 및 Cloud Manager 파이프라인 오류를 방지하는 것이 좋습니다. `.../dispatcher-sdk-x.x.x/bin` 폴더의 `update_maven.sh` 스크립트를 사용하여 업데이트합니다.

>[!VIDEO](https://video.tv.adobe.com/v/32987?quality=12&learn=on&captions=kor)

*이 비디오에서는 설명 목적으로 macOS을 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다.*


이전에 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을(를) 사용하여 AEM 프로젝트를 만들었다고 가정해 보겠습니다. 기본 Apache 및 Dispatcher 구성은 최신 버전입니다. 이러한 기준 구성을 사용하여 `dispatcher/src/conf.d` 및 `dispatcher/src/conf.dispatcher.d` 폴더에서 `*.vhost`, `*.conf`, `*.farm` 및 `*.any`과(와) 같은 파일을 다시 사용하고 복사하여 프로젝트별 구성을 만들었습니다. 로컬 Dispatcher 유효성 검사 및 Cloud Manager 파이프라인이 제대로 작동하고 있었습니다.

한편, 새로운 기능, 보안 수정 및 최적화와 같은 다양한 이유로 인해 기본 Apache 및 Dispatcher 구성이 개선되었습니다. AEM as a Cloud Service 릴리스의 일부로 Dispatcher Tools 의 최신 버전을 통해 릴리스됩니다.

이제, 최신 Dispatcher 도구 버전에 대해 프로젝트별 Dispatcher 구성의 유효성을 검사할 때 오류가 발생하기 시작합니다. 이 문제를 해결하려면 아래 단계를 사용하여 기준 구성을 업데이트해야 합니다.

+ 최신 Dispatcher 도구 버전에 대한 유효성 검사가 실패하는지 확인

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ `update_maven.sh` 스크립트를 사용하여 변경할 수 없는 파일 업데이트

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ `dispatcher_vhost.conf`, `default.vhost` 및 `default.farm`과(와) 같이 변경할 수 없는 업데이트된 파일을 확인하고 필요한 경우 이러한 파일에서 파생된 사용자 지정 파일을 변경합니다.

+ 구성을 재확인합니다. 이 경우 전달됩니다.

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ 변경 사항을 로컬로 확인한 후 업데이트된 구성 파일을 커밋합니다.

## 문제 해결

### docker_run 결과 &#39;host.docker.internal을 사용할 수 있을 때까지 대기 중&#39; 메시지{#troubleshooting-host-docker-internal}

`host.docker.internal`은(는) 호스트로 확인되는 Docker에 제공된 호스트 이름입니다. docs.docker.com에 따라([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> Docker 18.03 이상에서는 특별 DNS 이름 host.docker.internal에 연결하는 것이 좋습니다. 이 이름은 호스트가 사용하는 내부 IP 주소로 확인됩니다

`bin/docker_run src host.docker.internal:4503 8080`에서 __host.docker.internal을 사용할 수 있을 때까지 대기 중__&#x200B;이라는 메시지가 표시되면 다음을 수행합니다.

1. 설치된 Docker 버전이 18.03 이상인지 확인합니다.
1. `host.docker.internal` 이름의 등록/확인을 방해하는 로컬 컴퓨터가 설정되어 있을 수 있습니다. 대신 로컬 IP를 사용하십시오.

>[!BEGINTABS]

>[!TAB macOS]

+ 터미널에서 `ifconfig`을(를) 실행하고 호스트 __inet__ IP 주소(일반적으로 __en0__ 장치)를 기록합니다.
+ 그런 다음 호스트 IP 주소 `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`을(를) 사용하여 `docker_run`을(를) 실행하십시오.

>[!TAB Windows]

+ 명령 프롬프트에서 `ipconfig`을(를) 실행하고 호스트 컴퓨터의 __IPv4 주소__&#x200B;를 기록하십시오.
+ 그런 다음 IP 주소 `$ bin\docker_run src <HOST IP>:4503 8080`을(를) 사용하여 `docker_run`을(를) 실행하십시오.

>[!TAB Linux®]

+ 터미널에서 `ifconfig`을(를) 실행하고 호스트 __inet__ IP 주소(일반적으로 __en0__ 장치)를 기록합니다.
+ 그런 다음 호스트 IP 주소 `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`을(를) 사용하여 `docker_run`을(를) 실행하십시오.

>[!ENDTABS]

#### 오류 예

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## 추가 리소스

+ [AEM SDK 다운로드](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [도커 다운로드](https://www.docker.com/)
+ [WKND(AEM 참조 웹 사이트) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher 설명서](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR)
