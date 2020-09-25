---
title: AEM용 디스패처 도구를 Cloud Service 개발용으로 설정
description: AEM SDK의 Dispatcher Tools를 사용하면 Dispatcher 로컬에서 손쉽게 설치, 실행 및 문제를 해결할 수 있으므로 Adobe Experience Manager(AEM) 프로젝트의 로컬 개발을 수월하게 할 수 있습니다.
sub-product: foundation
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 2%

---


# 로컬 발송자 도구 설정

Adobe Experience Manager(AEM)의 Dispatcher는 CDN과 AEM 게시 계층 간에 보안 및 성능 레이어를 제공하는 Apache HTTP 웹 서버 모듈입니다. 디스패처는 전체 Experience Manager 아키텍처의 필수 부분이며 로컬 개발 설정의 일부여야 합니다.

Cloud Service SDK로 AEM에는 발송자를 로컬에 구성, 유효성 검사 및 시뮬레이트하는 데 도움이 되는 권장 발송자 도구 버전이 포함되어 있습니다. 발송자 도구는 다음과 같이 구성됩니다.

+ Apache HTTP Web Server 및 Dispatcher 구성 파일의 기본 집합 `.../dispatcher-sdk-x.x.x/src`
+ 구성 유효성 검사기 CLI 도구( `.../dispatcher-sdk-x.x.x/bin/validator`
+ 구성 배포 CLI 툴( `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ Dispatcher 모듈과 함께 Apache HTTP 웹 서버를 실행하는 Docker 이미지

사용자 디렉토리 `~` 의 약칭으로 사용됩니다. Windows의 경우 이 값은 `%HOMEPATH%`

>[!NOTE]
>
> 이 페이지의 비디오는 macOS에서 기록되었습니다. Windows 사용자는 각 비디오와 함께 제공되는 Dispatcher 도구 Windows 명령을 사용할 수 있습니다.

## 전제 조건

1. Windows 사용자는 Windows 10 Professional을 사용해야 합니다.
1. 로컬 개발 컴퓨터에 [Experience Manager 게시](./aem-runtime.md) QuickStart를 설치합니다.
   + 원할 경우, 로컬 AEM 게시 서비스에 최신 [AEM 참조](https://github.com/adobe/aem-guides-wknd/releases) 웹 사이트를 설치합니다. 이 웹 사이트는 이 자습서에서 작업 중인 디스패처를 시각화하는 데 사용됩니다.
1. 로컬 개발 컴퓨터에 최신 버전의 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+/Docker Engine v19.03.9+)를 설치하고 시작합니다.

## 디스패처 도구 다운로드(AEM SDK의 일부로)

Cloud Service SDK 또는 AEM SDK인 AEM에는 개발을 위해 Dispatcher 모듈과 함께 Apache HTTP 웹 서버를 로컬로 실행하는 데 사용되는 Dispatcher 도구 및 호환되는 QuickStart Jar가 포함되어 있습니다.

로컬 AEM 런타임을 [설정하기 위해 AEM을 Cloud Service SDK로 이미 다운로드한 경우에는](./aem-runtime.md)다시 다운로드할 필요가 없습니다.

1. Adobe ID으로 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) 로그인
   + AEM을 Cloud Service SDK로 다운로드하려면 Adobe 조직이 Cloud Service으로 프로비저닝되어야 ____ 합니다.
1. Cloud Service 탭으로 __AEM으로__ 이동
1. 게시된 __날짜별로 내림차순__ __으로__ 정렬
1. 최신 __AEM SDK__ 결과 행을 클릭합니다.
1. EULA를 검토하고 승인한 후 __다운로드__ 단추를 누릅니다
1. AEM SDK의 Dispatcher Tools v2.0.21+가 사용되는지 확인

## AEM SDK zip에서 Dispatcher 도구 추출

>[!TIP]
>
> Windows 사용자는 로컬 디스패처 도구가 들어 있는 폴더의 경로에 공백이나 특수 문자를 사용할 수 없습니다. 경로에 공백이 있으면 `docker_run.cmd` 실패합니다.

디스패처 도구 버전은 AEM SDK의 버전과 다릅니다. AEM과 일치하는 AEM SDK 버전을 통해 Dispatcher 도구 버전이 Cloud Service 버전으로 제공되는지 확인합니다.

1. 다운로드한 파일의 압축을 `aem-sdk-xxx.zip` 해제합니다.
1. Dispatcher 도구의 압축을 `~/aem-sdk/dispatcher`
   + Windows:압축 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 해제 `C:\Users\<My User>\aem-sdk\dispatcher` (필요한 경우 누락된 폴더 만들기)
   + macOS/Linux:함께 제공되는 셸 스크립트를 실행하여 발송자 도구 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 의 압축을 해제합니다.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

아래에 실행된 모든 명령은 현재 작업 디렉토리에 확장 중인 Dispatcher 도구 내용이 포함되어 있다고 가정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)
*이 비디오에서는 실례가 되는 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다*

## Dispatcher 구성 파일 이해

>[!TIP]
> AEM Project Maven [Tranype에서 만든 Experience Manager 프로젝트는 이 발송자 구성 파일](https://github.com/adobe/aem-project-archetype) 세트가 미리 채워지므로 Dispatcher Tools src 폴더에서 복사할 필요가 없습니다.

Dispatcher 도구는 로컬 개발을 포함하여 모든 환경에 대한 동작을 정의하는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일 집합을 제공합니다.

이러한 파일은 Experience Manager Maven 프로젝트에 아직 존재하지 않는 경우 Experience Manager Maven 프로젝트로 `dispatcher/src` 복사됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)
*이 비디오에서는 실례가 되는 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다*

구성 파일에 대한 전체 설명은 압축을 푼 Dispatcher 도구에서 사용할 수 있습니다 `dispatcher-sdk-x.x.x/docs/Config.html`.

## 로컬에서 Dispatcher 실행

Dispatcher를 로컬로 실행하려면 Dispatcher 도구의 CLI 도구를 사용하여 Dispatcher 구성 파일의 유효성을 검사해야 `validator` 합니다.

+ 사용량:
   + Windows: `bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

유효성 검사는 이중 용도로만 사용됩니다.

+ Apache HTTP 웹 서버 및 Dispatcher 구성 파일의 유효성을 확인합니다.
+ 구성을 Docker 컨테이너의 Apache HTTP Web Server와 호환되는 파일 세트로 변환합니다.

유효성이 확인되면 투명도 구성은 Docker 컨테이너에서 로컬로 Dispatcher를 실행합니다. 유효성 검사기의 옵션을 사용하여 최신 구성의 유효성을 ____ 확인하고 출력해야 `-d` 합니다.

+ 사용량:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

호스트 컴퓨터의 IP로 `aem-publish-host` `host.docker.internal`확인되는 특수 DNS 이름 Docker가 컨테이너에 제공하는 특수 DNS 이름을 로 설정할 수 있습니다. 해결되지 `host.docker.internal` 않는 경우 아래의 문제 [해결](#troubleshooting-host-docker-internal) 섹션을 참조하십시오.

예를 들어 Dispatcher 도구에서 제공하는 기본 구성 파일을 사용하여 Dispatcher Docker 컨테이너를 시작하려면 다음을 수행하십시오.

1. 구성을 변경할 때마다 `deployment-folder`규칙에 따라 이름 `out` 을 지정한 항목을 새로 생성합니다.

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (다시 시작)배포 폴더에 대한 경로를 제공하는 Dispatcher Docker 컨테이너를 시작합니다.

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

4503 포트에서 로컬로 실행되는 Cloud Service SDK의 게시 서비스인 AEM은 Dispatcher에서 사용할 수 있습니다 `http://localhost:8080`.

Experience Manager 프로젝트의 Dispatcher 구성에 대해 Dispatcher 도구를 실행하려면 프로젝트의 폴더를 `deployment-folder` `dispatcher/src` 사용하여 생성하기만 하면 됩니다.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)
*이 비디오에서는 실례가 되는 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다*

## 발송자 도구 로그

디스패처 로그는 로컬 개발 중에 HTTP 요청이 차단되었는지 및 그 이유를 파악하는 데 유용합니다. 환경 매개 변수를 사용하여 실행을 미리 수정하여 로그 수준 `docker_run` 을 설정할 수 있습니다.

Dispatcher 도구 로그는 실행 시 표준 출력 `docker_run` 으로 전송됩니다.

Dispatcher 디버깅에 유용한 매개 변수는 다음과 같습니다.

+ `DISP_LOG_LEVEL=Debug` 디버그 수준에 대한 발송자 모듈 로깅 설정
   + Default value is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 디버그 수준에 대한 Apache HTTP 웹 서버 재작성 모듈 로깅을 설정합니다.
   + Default value is: `Warn`
+ `DISP_RUN_MODE` Dispatcher 환경의 &quot;실행 모드&quot;를 설정하여 해당 실행 모드 Dispatcher 구성 파일을 로드합니다.
   + 기본값은 입니다.`dev`
+ 유효한 값: `dev`, `stage`또는 `prod`

하나 이상의 매개 변수를 `docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)
*이 비디오에서는 실례가 되는 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 유사한 결과를 얻을 수 있습니다*

## Dispatcher 도구 업데이트 시기{#dispatcher-tools-version}

Dispatcher 도구 버전은 Experience Manager보다 자주 증가하므로 Dispatcher 도구를 사용하는 경우 로컬 개발 환경의 업데이트 수가 줄어듭니다.

권장되는 Dispatcher 도구 버전은 Experience Manager 버전과 일치하는 Cloud Service SDK로 AEM에 번들로 제공되는 버전입니다. AEM의 Cloud Service 버전은 [Cloud Manager를 통해 찾을 수 있습니다](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > 환경__, __AEM 릴리스__ 레이블에서 지정한 환경별

![Experience Manager 버전](./assets/dispatcher-tools/aem-version.png)

_디스패처 도구 버전 자체가 Experience Manager 버전과 일치하지 않습니다._

## 문제 해결

### docker_run results in &#39;host.docker.internal is available&#39; message{#troubleshooting-host-docker-internal}

`host.docker.internal` 은 호스트로 확인되는 문서 포함에 제공된 호스트 이름입니다. docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/))당:

> Docker 18.03부터 권장 사항은 호스트가 사용하는 내부 IP 주소로 확인되는 특수 DNS 이름 host.docker.internal에 연결하는 것입니다

이 경우 host.docker.internal을 사용할 수 `bin/docker_run out host.docker.internal:4503 8080` 있을 때까지 대기 중 ____&#x200B;메시지가 표시되는 경우 다음을 수행합니다.

1. 설치된 Docker 버전이 18.03 이상인지 확인
2. 이름 등록/확인을 방해하는 로컬 컴퓨터 설정이 있을 수 `host.docker.internal` 있습니다. 대신 로컬 IP를 사용하십시오.
   + Windows:
      + 명령 프롬프트에서 호스트 컴퓨터의 호스트 `ipconfig`IPv4 주소 ____ 를 실행하고 기록합니다.
      + 그런 다음 이 IP 주소를 `docker_run` 사용하여 실행합니다.
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + Terminal에서 호스트 `ifconfig` inet __IP 주소(일반적으로__ en0 __장치)를 실행하고__ 기록합니다.
      + 그런 다음 호스트 IP 주소 `docker_run` 를 사용하여 실행합니다.
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### 예제 오류

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run 결과 &#39;** 오류:배포 폴더를 찾을 수 없습니다.&#39;

실행 `docker_run.cmd`시 __** 오류 메시지가 표시됩니다.배포 폴더를 찾을 수 없습니다:__. 일반적으로 경로에 공백이 있기 때문에 발생합니다. 가능하면 폴더의 공백을 제거하거나, 공백이 없는 경로로 `aem-sdk` 폴더를 이동합니다.

예를 들어 Windows 사용자 폴더는 `<First name> <Last name>`사이에 공백이 있는 경우가 많습니다. 아래 예에서 폴더 `...\My User\...` 에는 로컬 발송자 도구의 실행을 방해하는 공백이 `docker_run` 있습니다. 공백이 Windows 사용자 폴더에 있는 경우 Windows가 중단되므로 이 폴더의 이름을 변경하지 말고 사용자가 완전히 수정할 수 있는 권한을 가진 새 위치로 `aem-sdk` 폴더를 이동합니다. 폴더가 사용자의 홈 디렉토리에 있다고 `aem-sdk` 가정하는 지침은 새 위치로 조정되어야 합니다.

#### 예제 오류

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run이 Windows에서 시작되지 않음{#troubleshooting-windows-compatible}

Windows `docker_run` 에서 실행하면 다음 오류가 발생하여 발송자를 시작할 수 없습니다. 이 문제는 Windows에서 Dispatcher에 보고된 문제이며 향후 릴리스에서 해결됩니다.

#### 예제 오류

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

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
+ [Adobe 클라우드 관리자](https://my.cloudmanager.adobe.com/)
+ [Docker 다운로드](https://www.docker.com/)
+ [AEM Reference Website(WKND) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager 발송자 설명서](https://docs.adobe.com/content/help/ko-KR/experience-manager-dispatcher/using/dispatcher.html)
