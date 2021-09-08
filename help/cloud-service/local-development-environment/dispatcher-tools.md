---
title: AEM as a Cloud Service 개발용 Dispatcher 도구 설정
description: AEM SDK의 Dispatcher 도구를 사용하면 Dispatcher를 로컬로 설치, 실행 및 문제를 쉽게 처리할 수 있으므로 AEM(Adobe Experience Manager) 프로젝트의 로컬 개발을 쉽게 할 수 있습니다.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 3%

---


# 로컬 Dispatcher 도구 설정

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="로컬 Dispatcher 도구"
>abstract="Dispatcher는 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 세트에 포함되어야 합니다. AEM as a Cloud Service SDK에는 권장되는 Dispatcher 도구 버전이 포함되어 있어 Dispatcher를 로컬에서 구성, 유효성 검사 및 시뮬레이션을 용이하게 합니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="클라우드의 디스패처"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM as a Cloud Service SDK 다운로드"

AEM(Adobe Experience Manager)의 Dispatcher는 CDN과 AEM Publish 계층 간에 보안 및 성능 레이어를 제공하는 Apache HTTP 웹 서버 모듈입니다. Dispatcher는 전체 Experience Manager 아키텍처의 필수적인 부분이며 로컬 개발 세트에 포함되어야 합니다.

AEM as a Cloud Service SDK에는 권장되는 Dispatcher 도구 버전이 포함되어 있어 Dispatcher를 로컬에서 구성, 유효성 검사 및 시뮬레이션을 용이하게 합니다. Dispatcher 도구는 다음과 같이 구성됩니다.

+ `.../dispatcher-sdk-x.x.x/src`에 있는 Apache HTTP Web Server 및 Dispatcher 구성 파일의 기준 집합입니다.
+ `.../dispatcher-sdk-x.x.x/bin/validate`에 있는 구성 유효성 검사기 CLI 도구
+ `.../dispatcher-sdk-x.x.x/bin/validator`에 있는 구성 생성 CLI 툴
+ `.../dispatcher-sdk-x.x.x/bin/docker_run`에 있는 구성 배포 CLI 도구
+ Dispatcher 모듈을 사용하여 Apache HTTP 웹 서버를 실행하는 Docker 이미지

`~`은 사용자 디렉토리의 축약으로서 사용됩니다. Windows에서는 `%HOMEPATH%`에 해당합니다.

>[!NOTE]
>
> 이 페이지의 비디오는 macOS에서 녹화되었습니다. Windows 사용자는 따라할 수 있지만 각 비디오와 함께 제공되는 동등한 Dispatcher 도구 Windows 명령을 사용합니다.

## 전제 조건

1. Windows 사용자는 Windows 10 Professional(또는 Docker를 지원하는 버전)을 사용해야 합니다
1. 로컬 개발 컴퓨터에 [Experience Manager Publish Quickstart Jar](./aem-runtime.md)를 설치합니다.
   + 선택 사항으로, 로컬 AEM 게시 서비스에 최신 [AEM 참조 웹 사이트](https://github.com/adobe/aem-guides-wknd/releases)를 설치합니다. 이 웹 사이트는 이 자습서에서 작업 Dispatcher를 시각화하는 데 사용됩니다.
1. 로컬 개발 시스템에서 최신 버전의 [Docker](https://www.docker.com/)(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)를 설치하고 시작합니다.

## Dispatcher 도구 다운로드 (AEM SDK의 일부)

AEM as a Cloud Service SDK 또는 AEM SDK에는 개발을 위해 Dispatcher 모듈과 함께 Apache HTTP 웹 서버를 로컬로 실행하는 데 사용되는 Dispatcher 도구와 호환되는 QuickStart Jar가 포함되어 있습니다.

AEM as a Cloud Service SDK가 이미 [로컬 AEM 런타임 설정](./aem-runtime.md)에 다운로드된 경우 다시 다운로드할 필요가 없습니다.

1. Adobe ID을 사용하여 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atologing&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)에 로그인합니다
   + AEM을 Cloud Service SDK로 다운로드하려면 Adobe 조직 __AEM에 Cloud Service으로__&#x200B;이 제공되어야 합니다
1. 최신 __AEM SDK__ 결과 행을 클릭하여 다운로드합니다

## AEM SDK zip에서 Dispatcher 도구 추출

>[!TIP]
>
> Windows 사용자는 로컬 Dispatcher 도구가 포함된 폴더의 경로에 공백이나 특수 문자를 사용할 수 없습니다. 경로에 공백이 있으면 `docker_run.cmd`이(가) 실패합니다.

Dispatcher 도구 버전은 AEM SDK의 버전과 다릅니다. AEM SDK 버전을 Cloud Service 버전으로 AEM과 일치하는 Dispatcher 도구 버전을 통해 제공하는지 확인하십시오.

1. 다운로드한 `aem-sdk-xxx.zip` 파일의 압축을 해제합니다
1. Dispatcher 도구의 짐을 `~/aem-sdk/dispatcher`에 넣습니다.
   + Windows: `aem-sdk-dispatcher-tools-x.x.x-windows.zip`을 `C:\Users\<My User>\aem-sdk\dispatcher`에 압축 해제(필요한 경우 누락된 폴더 만들기)
   + macOS / Linux: 함께 셸 스크립트 `aem-sdk-dispatcher-tools-x.x.x-unix.sh`를 실행하여 Dispatcher 도구의 압축을 해제합니다
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

아래 실행된 모든 명령은 현재 작업 디렉터리에 확장된 Dispatcher 도구 콘텐츠가 포함되어 있다고 가정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*이 비디오에서는 실례가 되는 목적으로 macOS를 사용합니다. 동등한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다*

## Dispatcher 구성 파일 이해

>[!TIP]
> [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)에서 만든 Experience Manager 프로젝트는 이 Dispatcher 구성 파일 세트를 미리 채우므로 Dispatcher 도구 src 폴더에서 복사할 필요가 없습니다.

Dispatcher 도구는 로컬 개발을 포함하여 모든 환경에 대한 동작을 정의하는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일 세트를 제공합니다.

이러한 파일은 Experience Manager Maven 프로젝트에 아직 없는 경우 Maven 프로젝트에 `dispatcher/src` 폴더에 복사되기 위한 것입니다.

구성 파일에 대한 전체 설명은 압축을 푼 Dispatcher 도구에서 `dispatcher-sdk-x.x.x/docs/Config.html`로 사용할 수 있습니다.

## 구성 유효성 검사

선택적으로, `validate` 스크립트를 사용하여 Dispatcher 및 Apache 웹 서버 구성(`httpd -t`을 통해)의 유효성을 확인할 수 있습니다( `validator` 실행 파일과 혼동하지 않도록). `validate` 스크립트는 `validator`의 [3단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode)를 편리하게 실행할 수 있도록 해줍니다.

+ 사용량:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## 로컬에서 Dispatcher 실행

AEM Dispatcher는 `src` Dispatcher 및 Apache 웹 서버 구성 파일에 대해 Docker를 사용하여 로컬에서 실행됩니다.

+ 사용량:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`<aem-publish-host>`은(는) Docker가 호스트 컴퓨터의 IP로 확인되는 컨테이너에 제공하는 특수 DNS 이름인 `host.docker.internal`로 설정할 수 있습니다. `host.docker.internal`이 해결되지 않으면 아래의 [문제 해결](#troubleshooting-host-docker-internal) 섹션을 참조하십시오.

예를 들어 Dispatcher 도구에서 제공하는 기본 구성 파일을 사용하여 Dispatcher Docker 컨테이너를 시작하려면 다음을 수행하십시오.

Dispatcher 구성 src 폴더에 대한 경로를 제공하는 Dispatcher Docker 컨테이너를 시작합니다.

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

포트 4503에서 로컬로 실행되는 AEM as a Cloud Service SDK의 게시 서비스는 `http://localhost:8080`에 있는 Dispatcher를 통해 사용할 수 있습니다.

Experience Manager 프로젝트의 Dispatcher 구성에 대해 Dispatcher 도구를 실행하려면 프로젝트의 `dispatcher/src` 폴더를 가리킵니다.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher 도구 로그

Dispatcher 로그는 로컬 개발 중에 HTTP 요청이 차단되었는지 및 왜 차단되는지를 이해하는 데 유용합니다. 환경 매개 변수를 사용하여 `docker_run` 실행을 미리 수정하여 로그 수준을 설정할 수 있습니다.

Dispatcher 도구 로그는 `docker_run`이(가) 실행될 때 표준으로 내보내집니다.

Dispatcher 디버깅에 유용한 매개 변수는 다음과 같습니다.

+ `DISP_LOG_LEVEL=Debug` Dispatcher 모듈 로깅을 디버그 수준으로 설정합니다
   + 기본값은 다음과 같습니다. `Warn`
+ `REWRITE_LOG_LEVEL=Debug` apache HTTP 웹 서버 재작성 모듈 로깅을 디버그 수준으로 설정합니다.
   + 기본값은 다음과 같습니다. `Warn`
+ `DISP_RUN_MODE` dispatcher 환경의 &quot;실행 모드&quot;를 설정하여 해당 실행 모드 Dispatcher 구성 파일을 로드합니다.
   + 기본값은 입니다.`dev`
+ 유효한 값: `dev`, `stage` 또는 `prod`

하나 이상의 매개 변수를 `docker_run`에 전달할 수 있습니다.

+ Windows:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### 로그 파일 액세스

Apache 웹 서버 및 AEM Dispatcher 로그는 Docker 컨테이너에서 직접 액세스할 수 있습니다.

+ [Docker 컨테이너의 로그 액세스](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Docker 로그를 로컬 파일 시스템에 복사](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher 도구를 업데이트하는 경우{#dispatcher-tools-version}

Dispatcher 도구 버전은 Experience Manager보다 자주 증가하지 않으므로 Dispatcher 도구는 로컬 개발 환경에서 더 적은 업데이트를 필요로 합니다.

권장되는 Dispatcher 도구 버전은 Cloud Service 버전으로 Experience Manager과 일치하는 Cloud Service SDK로서 AEM과 번들로 제공됩니다. AEM as a Cloud Service 버전은 [Cloud Manager](https://my.cloudmanager.adobe.com/)를 통해 찾을 수 있습니다.

+ __Cloud Manager > 환경__( __AEM 릴리스__ 레이블에서 지정한 환경별)

![Experience Manager 버전](./assets/dispatcher-tools/aem-version.png)

_Dispatcher 도구 버전 자체는 Experience Manager 버전과 일치하지 않습니다._

## 문제 해결

### docker_run 결과 &#39;host.docker.internal을 사용할 수 있을 때까지 대기 중&#39; 메시지{#troubleshooting-host-docker-internal}

`host.docker.internal` 은 Docker에 제공된 호스트 이름이며 은 호스트에 확인됩니다. docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/))당:

> Docker 18.03에서는 호스트에서 사용하는 내부 IP 주소로 확인되는 특수 DNS 이름 host.docker.internal에 연결하는 것이 좋습니다

`bin/docker_run src host.docker.internal:4503 8080` 결과 __host.docker.internal을 사용할 수 있을 때까지 대기 중__ 메시지가 표시되면 다음을 수행합니다.

1. 설치된 Docker 버전이 18.03 이상인지 확인합니다.
2. `host.docker.internal` 이름의 등록/확인을 방지하는 로컬 컴퓨터 설정이 있을 수 있습니다. 대신 로컬 IP를 사용하십시오.
   + Windows:
      + 명령 프롬프트에서 `ipconfig`을 실행하고 호스트 컴퓨터의 __IPv4 주소__&#x200B;를 기록합니다.
      + 그런 다음 이 IP 주소를 사용하여 `docker_run`을 실행합니다.
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux:
      + 터미널에서 `ifconfig` 을 실행하고 호스트 __inet__ IP 주소(일반적으로 __en0__ 장치)를 기록합니다.
      + 그런 다음 호스트 IP 주소를 사용하여 `docker_run`을 실행합니다.
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 예제 오류

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run이 Windows에서 시작되지 않음{#troubleshooting-windows-compatible}

Windows에서 `docker_run`을 실행하면 다음 오류가 발생하여 Dispatcher가 시작되지 않을 수 있습니다. 이 문제는 Windows에서 Dispatcher에 보고된 문제이며 향후 릴리스에서 수정됩니다.

#### 예제 오류

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## 추가 리소스

+ [AEM SDK 다운로드](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker 다운로드](https://www.docker.com/)
+ [AEM 참조 웹 사이트(WKND) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher 설명서](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR)
