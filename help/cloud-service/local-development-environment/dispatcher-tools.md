---
title: AEM as a Cloud Service 개발을 위한 Dispatcher 도구 설정
description: AEM SDK의 Dispatcher 도구를 사용하면 Dispatcher를 로컬로 설치, 실행 및 문제를 쉽게 처리할 수 있으므로 AEM(Adobe Experience Manager) 프로젝트의 로컬 개발을 쉽게 할 수 있습니다.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 9%

---

# 로컬 Dispatcher 도구 설정 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="로컬 Dispatcher 도구"
>abstract="Dispatcher는 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 설정의 일부여야 합니다. AEM as a Cloud Service SDK에는 Dispatcher를 로컬에서 쉽게 구성, 유효성 검사 및 시뮬레이션할 수 있는 권장 Dispatcher 도구 버전이 포함되어 있습니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="클라우드의 Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM as a Cloud Service SDK 다운로드"

AEM(Adobe Experience Manager)의 Dispatcher는 CDN과 AEM Publish 계층 간에 보안 및 성능 레이어를 제공하는 Apache HTTP 웹 서버 모듈입니다. Dispatcher는 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 설정의 일부여야 합니다.

AEM as a Cloud Service SDK에는 Dispatcher를 로컬에서 쉽게 구성, 유효성 검사 및 시뮬레이션할 수 있는 권장 Dispatcher 도구 버전이 포함되어 있습니다. Dispatcher 도구는 다음과 같이 구성됩니다.

+ Apache HTTP 웹 서버 및 Dispatcher 구성 파일의 기본 집합이며 `.../dispatcher-sdk-x.x.x/src`
+ 구성 유효성 검사기 CLI 툴은 `.../dispatcher-sdk-x.x.x/bin/validate`
+ 구성 생성 CLI 툴은 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 구성 배포 CLI 툴은 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 변경할 수 없는 구성 파일이 CLI 툴을 덮어쓸 때 `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ Dispatcher 모듈을 사용하여 Apache HTTP 웹 서버를 실행하는 Docker 이미지

참고 사항 `~` 는 사용자 디렉토리의 축약어로 사용됩니다. Windows에서는 다음과 같습니다 `%HOMEPATH%`.

>[!NOTE]
>
> 이 페이지의 비디오는 macOS에 기록됩니다. Windows 사용자는 따라할 수 있지만 각 비디오와 함께 제공되는 동등한 Dispatcher 도구 Windows 명령을 사용합니다.

## 사전 요구 사항

1. Windows 사용자는 Windows 10 Professional(또는 Docker를 지원하는 버전)을 사용해야 합니다
1. 설치 [Experience Manager 게시 Quickstart Jar](./aem-runtime.md) 로컬 개발 시스템에서

+ 최신 버전을 설치할 수도 있습니다 [AEM 참조 웹 사이트](https://github.com/adobe/aem-guides-wknd/releases) 추가 작업이 필요할 경우 이 웹 사이트는 이 자습서에서 작업 Dispatcher를 시각화하는 데 사용됩니다.

1. 최신 버전 설치 및 시작 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) 를 로컬 개발 시스템에서 사용할 수 있습니다.

## Dispatcher 도구 다운로드 (AEM SDK의 일부)

AEM as a Cloud Service SDK 또는 AEM SDK에는 개발을 위해 Dispatcher 모듈과 함께 Apache HTTP 웹 서버를 로컬로 실행하는 데 사용되는 Dispatcher 도구 및 호환되는 QuickStart Jar가 포함되어 있습니다.

AEM as a Cloud Service SDK가에 이미 다운로드된 경우 [로컬 AEM 런타임 설정](./aem-runtime.md)를 재다운로드할 필요는 없습니다.

1. 에 로그인합니다. [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atologing&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) Adobe ID 사용
   + Adobe 조직 __반드시__ AEM as a Cloud Service SDK를 다운로드할 수 있는 AEM as a Cloud Service이 제공됩니다
1. 최신 항목을 클릭합니다. __AEM SDK__ 다운로드 결과 행

## AEM SDK zip에서 Dispatcher 도구 추출

>[!TIP]
>
> Windows 사용자는 로컬 Dispatcher 도구가 포함된 폴더의 경로에 공백이나 특수 문자를 사용할 수 없습니다. 경로에 공백이 있으면 `docker_run.cmd` 실패.

Dispatcher 도구 버전은 AEM SDK의 버전과 다릅니다. Dispatcher 도구 버전이 AEM as a Cloud Service 버전과 일치하는 AEM SDK 버전을 통해 제공되는지 확인하십시오.

1. 다운로드한 파일의 압축을 해제합니다 `aem-sdk-xxx.zip` 파일
1. Dispatcher 도구의 압축을 해제합니다. `~/aem-sdk/dispatcher`

+ Windows: 압축 해제 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 변환 `C:\Users\<My User>\aem-sdk\dispatcher` (필요에 따라 누락된 폴더 만들기)
+ macOS Linux®: 함께 제공되는 셸 스크립트 실행 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` dispatcher 도구의 압축을 풀려면
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

아래 실행된 모든 명령은 현재 작업 디렉터리에 확장된 Dispatcher 도구 콘텐츠가 포함되어 있다고 가정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*이 비디오에서는 macOS을 예시적인 용도로 사용합니다. 동등한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다.*

## Dispatcher 구성 파일 이해

>[!TIP]
> 에서 만든 프로젝트 Experience Manager [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) 는 이 Dispatcher 구성 파일 세트를 미리 채우므로 Dispatcher 도구 src 폴더에서 복사할 필요가 없습니다.

Dispatcher 도구는 로컬 개발을 포함하여 모든 환경에 대한 동작을 정의하는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일 세트를 제공합니다.

이러한 파일은 Experience Manager Maven 프로젝트에 `dispatcher/src` 폴더를 삭제해도 됩니다.

구성 파일에 대한 전체 설명은 압축을 푼 Dispatcher 도구에서 사용할 수 있습니다. `dispatcher-sdk-x.x.x/docs/Config.html`.

## 구성 유효성 검사

선택적으로, Dispatcher 및 Apache 웹 서버 구성( 를 통해) `httpd -t`)은 `validate` 스크립트(와 혼동하지 않음) `validator` 실행 가능). 다음 `validate` 스크립트를 사용하면 [3단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) 의 `validator`.

+ 사용:
   + Windows: `bin\validate src`
   + macOS Linux®: `./bin/validate.sh ./src`

## 로컬에서 Dispatcher 실행

AEM Dispatcher는 Docker를 사용하여 로컬에서 `src` Dispatcher 및 Apache 웹 서버 구성 파일.

+ 사용:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

다음 `<aem-publish-host>` 를 로 설정할 수 있습니다. `host.docker.internal`인 경우 Docker가 호스트 시스템의 IP로 확인되는 컨테이너에 제공하는 특수 DNS 이름 입니다. 만약 `host.docker.internal` 확인할 수 없습니다. [문제 해결](#troubleshooting-host-docker-internal) 섹션을 참조하십시오.

예를 들어 Dispatcher 도구에서 제공하는 기본 구성 파일을 사용하여 Dispatcher Docker 컨테이너를 시작하려면 다음을 수행하십시오.

Dispatcher 구성 src 폴더에 대한 경로를 제공하는 Dispatcher Docker 컨테이너를 시작합니다.

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

포트 4503에서 로컬로 실행되는 AEM as a Cloud Service SDK의 게시 서비스는 의 Dispatcher를 통해 사용할 수 있습니다. `http://localhost:8080`.

Experience Manager 프로젝트의 Dispatcher 구성에 대해 Dispatcher 도구를 실행하려면 프로젝트의 `dispatcher/src` 폴더를 입력합니다.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher 도구 로그

Dispatcher 로그는 로컬 개발 중에 HTTP 요청이 차단되었는지 및 왜 차단되는지를 이해하는 데 유용합니다. 로그 레벨은 `docker_run` 환경 매개 변수와 함께 사용할 수 있습니다.

디스패처 도구 로그는 다음과 같은 경우 표준으로 내보내집니다 `docker_run` 가 실행됩니다.

Dispatcher 디버깅에 유용한 매개 변수는 다음과 같습니다.

+ `DISP_LOG_LEVEL=Debug` Dispatcher 모듈 로깅을 디버그 수준으로 설정합니다
   + 기본값은 다음과 같습니다. `Warn`
+ `REWRITE_LOG_LEVEL=Debug` apache HTTP 웹 서버 재작성 모듈 로깅을 디버그 수준으로 설정합니다.
   + 기본값은 다음과 같습니다. `Warn`
+ `DISP_RUN_MODE` dispatcher 환경의 &quot;실행 모드&quot;를 설정하여 해당 실행 모드 Dispatcher 구성 파일을 로드합니다.
   + 기본값은 입니다.`dev`
+ 유효한 값: `dev`, `stage`, 또는 `prod`

하나 이상의 매개 변수를에 전달할 수 있습니다 `docker_run`

+ Windows:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### 로그 파일 액세스

Apache 웹 서버 및 AEM Dispatcher 로그는 Docker 컨테이너에서 직접 액세스할 수 있습니다.

+ [Docker 컨테이너의 로그 액세스](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Docker 로그를 로컬 파일 시스템에 복사](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher 도구를 업데이트하는 경우{#dispatcher-tools-version}

Dispatcher 도구 버전은 Experience Manager보다 자주 증가하지 않으므로 Dispatcher 도구는 로컬 개발 환경에서 더 적은 업데이트를 필요로 합니다.

권장되는 Dispatcher 도구 버전은 Experience Manager as a Cloud Service 버전과 일치하는 AEM as a Cloud Service SDK와 번들로 제공됩니다. AEM as a Cloud Service 버전은 을 통해 찾을 수 있습니다. [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > 환경__, __AEM 릴리스__ 레이블

![Experience Manager 버전](./assets/dispatcher-tools/aem-version.png)

*Dispatcher 도구 버전이 Experience Manager 버전과 일치하지 않습니다.*

## Apache 및 Dispatcher 구성의 기본 세트를 업데이트하는 방법

Apache 및 Dispatcher 구성의 기본 세트가 정기적으로 향상되며 AEM as a Cloud Service SDK 버전과 함께 릴리스됩니다. 기본 구성 개선 사항을 AEM 프로젝트에 통합하고 방지하는 것이 가장 좋습니다 [로컬 유효성 검사](#validate-configurations) 및 Cloud Manager 파이프라인 오류가 발생했습니다. 를 사용하여 업데이트합니다. `update_maven.sh` 스크립트에서 `.../dispatcher-sdk-x.x.x/bin` 폴더를 입력합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*이 비디오에서는 macOS을 예시적인 용도로 사용합니다. 동등한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다.*


이전에 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype): 기준선 Apache 및 Dispatcher 구성이 최신 상태입니다. 이러한 기준 구성을 사용하면 프로젝트 특정 구성을 재사용하고 다음과 같은 파일을 복사하여 만들었습니다 `*.vhost`, `*.conf`, `*.farm` 및 `*.any` 에서 `dispatcher/src/conf.d` 및 `dispatcher/src/conf.dispatcher.d` 폴더. 로컬 Dispatcher 유효성 검사 및 Cloud Manager 파이프라인이 제대로 작동했습니다.

한편, 새로운 기능, 보안 수정 및 최적화와 같은 다양한 이유로 기준 Apache 및 Dispatcher 구성이 향상되었습니다. 최신 버전의 AEM as a Cloud Service 도구를 통해 릴리스됩니다.

이제 최신 Dispatcher 도구 버전에 대해 프로젝트별 Dispatcher 구성을 확인할 때 오류가 발생합니다. 이 문제를 해결하려면 아래 단계를 사용하여 기준 구성을 업데이트해야 합니다.

+ 최신 Dispatcher 도구 버전에 대해 유효성 검사가 실패하는지 확인합니다

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ 를 사용하여 변경할 수 없는 파일을 업데이트합니다. `update_maven.sh` script

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

+ 다음과 같이 업데이트된 변경할 수 없는 파일을 확인합니다. `dispatcher_vhost.conf`, `default.vhost`, 및 `default.farm` 필요한 경우 이러한 파일에서 파생된 사용자 지정 파일을 적절하게 변경합니다.

+ 구성을 재검증하려면 다음을 수행해야 합니다

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

+ 변경 내용을 로컬로 확인한 후 업데이트된 구성 파일을 커밋합니다

## 문제 해결

### docker_run 결과 &#39;host.docker.internal을 사용할 수 있을 때까지 대기 중&#39; 메시지{#troubleshooting-host-docker-internal}

다음 `host.docker.internal` 은 Docker에 제공된 호스트 이름이며 은 호스트에 확인됩니다. docs.docker.com당 ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> Docker 18.03에서는 호스트에서 사용하는 내부 IP 주소로 확인되는 특수 DNS 이름 host.docker.internal에 연결하는 것이 좋습니다

When `bin/docker_run src host.docker.internal:4503 8080` 결과: 메시지 __host.docker.internal을 사용할 수 있을 때까지 대기 중__, 그 다음:

1. 설치된 Docker 버전이 18.03 이상인지 확인합니다.
2. 로컬 컴퓨터를 설정하여 `host.docker.internal` 이름. 대신 로컬 IP를 사용하십시오.
   + Windows:
   + 명령 프롬프트에서 다음을 실행합니다 `ipconfig`, 및에 호스트 __IPv4 주소__ 호스트 시스템의 이름입니다.
   + 그런 다음 를 실행합니다 `docker_run` 다음 IP 주소 사용:
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®:
   + 터미널에서 실행 `ifconfig` 호스트 __inet__ IP 주소(일반적으로 __en0__ 장치.
   + 그런 다음 실행합니다 `docker_run` 호스트 IP 주소 사용:
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 예제 오류

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
+ [Docker 다운로드](https://www.docker.com/)
+ [AEM 참조 웹 사이트(WKND) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher 설명서](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR)
