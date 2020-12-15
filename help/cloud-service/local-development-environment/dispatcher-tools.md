---
title: AEM용 디스패처 도구를 Cloud Service 개발으로 설정
description: AEM SDK의 Dispatcher Tools를 사용하면 Dispatcher를 로컬에서 손쉽게 설치, 실행 및 해결할 수 있으므로 AEM(Adobe Experience Manager) 프로젝트의 로컬 개발을 수월하게 수행할 수 있습니다.
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
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# 로컬 디스패처 도구 설정

Adobe Experience Manager(AEM)의 Dispatcher는 CDN과 AEM 게시 계층 사이의 보안 및 성능 레이어를 제공하는 Apache HTTP 웹 서버 모듈입니다. 디스패처는 전체 Experience Manager 아키텍처의 필수 부분이며 로컬 개발 설정의 일부여야 합니다.

Cloud Service SDK로 AEM에는 Dispatcher 로컬에서 구성, 유효성 검사 및 시뮬레이션을 수행하는 데 유용한 권장 Dispatcher 도구 버전이 포함되어 있습니다. Dispatcher 도구는 다음과 같이 구성됩니다.

+ `.../dispatcher-sdk-x.x.x/src`에 있는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일의 기준선 집합
+ `.../dispatcher-sdk-x.x.x/bin/validate`에 있는 구성 유효성 검사기 CLI 도구(Dispatcher SDK 2.0.29+)
+ `.../dispatcher-sdk-x.x.x/bin/validator`에 있는 구성 생성 CLI 도구
+ `.../dispatcher-sdk-x.x.x/bin/docker_run`에 있는 구성 배포 CLI 도구
+ Dispatcher 모듈에서 Apache HTTP 웹 서버를 실행하는 Docker 이미지

`~`은(는) 사용자 디렉토리의 축약형으로 사용됩니다. Windows에서는 `%HOMEPATH%`과 같습니다.

>[!NOTE]
>
> 이 페이지의 비디오는 macOS에서 레코딩되었습니다. Windows 사용자는 각 비디오에 포함된 Dispatcher 도구 Windows 명령을 사용할 수 있습니다.

## 전제 조건

1. Windows 사용자는 Windows 10 Professional을 사용해야 합니다.
1. 로컬 개발 컴퓨터에 [Experience Manager 게시 빠른 시작 jar](./aem-runtime.md)을 설치합니다.
   + 원할 경우, 로컬 AEM 게시 서비스에 최신 [AEM 참조 웹 사이트](https://github.com/adobe/aem-guides-wknd/releases)를 설치하십시오. 이 웹 사이트는 이 자습서에서 작업 중인 디스패처를 시각화하는 데 사용됩니다.
1. 로컬 개발 컴퓨터에 최신 버전의 [Docker](https://www.docker.com/)(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)를 설치하고 시작합니다.

## 디스패처 도구 다운로드(AEM SDK의 일부로)

Cloud Service SDK 또는 AEM SDK인 AEM에는 개발을 위해 Dispatcher 모듈에서 Apache HTTP 웹 서버를 로컬로 실행하는 데 사용되는 Dispatcher Tools와 호환되는 QuickStart Jar가 포함되어 있습니다.

Cloud Service SDK로 AEM이 이미 [로컬 AEM 런타임 설정](./aem-runtime.md)에 다운로드된 경우 다시 다운로드할 필요가 없습니다.

1. Adobe ID을 사용하여 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=을 참조하십시오.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AssoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%3AlastModified&amp;derby.derby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)에 로그인합니다.
   + AEM을 Cloud Service SDK로 다운로드하려면 Adobe 조직 __이(가) Cloud Service으로 제공되어야 합니다.__
1. 다운로드할 최신 __AEM SDK__ 결과 행을 클릭합니다.
   + 다운로드 설명에 AEM SDK의 Dispatcher 도구 v2.0.29+이 명시되어 있는지 확인

## AEM SDK zip에서 Dispatcher 도구 추출

>[!TIP]
>
> Windows 사용자는 로컬 디스패처 도구가 들어 있는 폴더의 경로에 공백이나 특수 문자를 사용할 수 없습니다. 경로에 공백이 있으면 `docker_run.cmd`이(가) 실패합니다.

Dispatcher 도구 버전은 AEM SDK의 버전과 다릅니다. AEM SDK 버전을 통해 Dispatcher Tools의 버전이 AEM 버전과 일치하는 Cloud Service 버전을 제공하는지 확인합니다.

1. 다운로드한 `aem-sdk-xxx.zip` 파일의 압축을 해제합니다.
1. Dispatcher 도구를 `~/aem-sdk/dispatcher`에 압축 해제
   + Windows:`aem-sdk-dispatcher-tools-x.x.x-windows.zip`을(를) `C:\Users\<My User>\aem-sdk\dispatcher`에 압축 해제(필요한 경우 누락된 폴더 만들기)
   + macOS / Linux:함께 제공되는 셸 스크립트 `aem-sdk-dispatcher-tools-x.x.x-unix.sh`을 실행하여 발송자 도구의 압축을 해제합니다.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

아래에 실행된 모든 명령은 현재 작업 디렉토리에 확장 중인 디스패처 도구 내용이 포함되어 있다고 가정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*이 비디오에서는 일러스트레이션 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 비슷한 결과를 얻을 수 있습니다*

## Dispatcher 구성 파일 이해

>[!TIP]
> [AEM Project Maven Tranype](https://github.com/adobe/aem-project-archetype)에서 만든 Experience Manager 프로젝트는 이 Dispatcher 구성 파일 세트가 미리 채워지므로 Dispatcher 도구 src 폴더에서 복사할 필요가 없습니다.

Dispatcher 도구는 로컬 개발을 포함하여 모든 환경에 대한 동작을 정의하는 Apache HTTP 웹 서버 및 Dispatcher 구성 파일 세트를 제공합니다.

이 파일은 Experience Manager Maven 프로젝트에 아직 존재하지 않는 경우 Experience Manager Maven 프로젝트에 `dispatcher/src` 폴더로 복사되도록 만들어졌습니다.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*이 비디오에서는 일러스트레이션 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 비슷한 결과를 얻을 수 있습니다*

구성 파일에 대한 전체 설명은 압축을 푼 디스패처 도구에서 `dispatcher-sdk-x.x.x/docs/Config.html`으로 사용할 수 있습니다.

## 구성 유효성 확인

원할 경우, Dispatcher 및 Apache 웹 서버 구성(`httpd -t`을 통해)은 `validate` 스크립트를 사용하여 유효성을 검사할 수 있습니다(`validator` 실행 파일과 혼동하지 않도록 함).

+ 사용량:
   + Windows: `bin\validate src`
   + macOS / Linux:`./bin/validate ./src`

## 로컬에서 Dispatcher 실행

Dispatcher를 로컬로 실행하려면 Dispatcher 도구의 `validator` CLI 도구를 사용하여 Dispatcher 구성 파일을 생성해야 합니다.

+ 사용량:
   + Windows:`bin\validator full -d out src`
   + macOS / Linux:`./bin/validator full -d ./out ./src`

이 명령은 구성을 Docker 컨테이너의 Apache HTTP 웹 서버와 호환되는 파일 세트로 변환합니다.

생성된 투명도 구성은 Docker 컨테이너에서 로컬로 Dispatcher를 실행합니다. 유효성 검사기의 `-d` 옵션을 사용하여 `validate` __및__ 출력을 사용하여 최신 구성의 유효성을 검사해야 합니다.

+ 사용량:
   + Windows:`bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux:`./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host`은(는) 호스트 컴퓨터의 IP로 확인되는 특수 DNS 이름 Docker가 컨테이너에 제공하는 `host.docker.internal`로 설정할 수 있습니다. `host.docker.internal`이(가) 확인되지 않으면 아래의 [문제 해결](#troubleshooting-host-docker-internal) 섹션을 참조하십시오.

예를 들어 발송자 도구에서 제공하는 기본 구성 파일을 사용하여 발송자 문서 컨테이너를 시작하는 경우:

1. 구성을 변경할 때마다 규칙에 따라 `out`이라는 `deployment-folder`을 새로 생성합니다.

   + Windows:`del /Q out && bin\validator full -d out src`
   + macOS / Linux:`rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (다시 시작)배포 폴더에 대한 경로를 제공하는 Dispatcher Docker 컨테이너를 시작합니다.

   + Windows:`bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux:`./bin/docker_run.sh ./out host.docker.internal:4503 8080`

포트 4503에서 로컬로 실행되는 Cloud Service SDK의 게시 서비스로 AEM은 `http://localhost:8080`에서 Dispatcher를 통해 사용할 수 있습니다.

Experience Manager 프로젝트의 Dispatcher 구성에 대해 Dispatcher 도구를 실행하려면 프로젝트의 `dispatcher/src` 폴더를 사용하여 `deployment-folder`만 생성하면 됩니다.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*이 비디오에서는 일러스트레이션 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 비슷한 결과를 얻을 수 있습니다*

## 발송자 도구 로그

디스패처 로그는 로컬 개발 중에 HTTP 요청이 차단되었는지 여부와 그 이유를 파악하는 데 유용합니다. 환경 매개 변수를 사용하여 `docker_run`의 실행을 미리 수정하여 로그 수준을 설정할 수 있습니다.

`docker_run`이(가) 실행될 때 발송되는 발송자 도구 로그는 표준 아웃으로 전달됩니다.

Dispatcher 디버깅에 유용한 매개 변수는 다음과 같습니다.

+ `DISP_LOG_LEVEL=Debug` 디버그 수준에 대한 발송자 모듈 로깅을 설정합니다.
   + 기본값은 다음과 같습니다.`Warn`
+ `REWRITE_LOG_LEVEL=Debug` 디버그 수준에 대한 Apache HTTP 웹 서버 다시 작성 모듈 로깅을 설정합니다.
   + 기본값은 다음과 같습니다.`Warn`
+ `DISP_RUN_MODE` Dispatcher 환경의 &quot;실행 모드&quot;를 설정하여 해당 실행 모드 Dispatcher 구성 파일을 로드합니다.
   + 기본값은 입니다.`dev`
+ 유효한 값:`dev`, `stage` 또는 `prod`

하나 이상의 매개 변수를 `docker_run`에 전달할 수 있습니다.

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*이 비디오에서는 일러스트레이션 목적으로 macOS를 사용합니다. 동일한 Windows/Linux 명령을 사용하여 비슷한 결과를 얻을 수 있습니다*

## Dispatcher 도구를 업데이트하는 경우{#dispatcher-tools-version}

발송자 도구 버전은 Experience Manager보다 자주 증가하므로 발송자 도구는 로컬 개발 환경에서 더 적은 업데이트를 필요로 합니다.

권장되는 Dispatcher 도구 버전은 Experience Manager 버전과 일치하는 Cloud Service SDK로 AEM에 번들로 제공되는 버전입니다. Cloud Service의 AEM 버전은 [클라우드 관리자](https://my.cloudmanager.adobe.com/)를 통해 찾을 수 있습니다.

+ __Cloud Manager > 환경__,  __AEM__ 릴리스 레이블에서 지정한 환경별

![Experience Manager 버전](./assets/dispatcher-tools/aem-version.png)

_디스패처 도구 버전 자체가 Experience Manager 버전과 일치하지 않습니다._

## 문제 해결

### docker_run 결과 &#39;host.docker.internal을 사용할 수 있을 때까지 대기 중&#39; 메시지{#troubleshooting-host-docker-internal}

`host.docker.internal` 는 호스트로 확인되는 doker에 제공된 호스트 이름입니다. docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/))당:

> Docker 18.03부터는 호스트가 사용하는 내부 IP 주소로 확인되는 특수 DNS 이름 host.docker.internal에 연결하는 것이 좋습니다

`bin/docker_run out host.docker.internal:4503 8080`이(가) __host.docker.internal을 사용할 수 있을 때까지 대기 중__&#x200B;이라는 메시지가 표시되는 경우 다음을 수행합니다.

1. 설치된 Docker 버전이 18.03 이상인지 확인
2. `host.docker.internal` 이름의 등록/해상도를 방해하는 로컬 컴퓨터 설정이 있을 수 있습니다. 대신 로컬 IP를 사용하십시오.
   + Windows:
      + 명령 프롬프트에서 `ipconfig`을 실행하고 호스트 컴퓨터의 __IPv4 주소__&#x200B;를 기록합니다.
      + 그런 다음 다음 다음 IP 주소를 사용하여 `docker_run`을 실행합니다.
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + 터미널에서 `ifconfig`을(를) 실행하고 호스트 __inet__ IP 주소(일반적으로 __en0__ 장치)를 기록합니다.
      + 그런 다음 호스트 IP 주소를 사용하여 `docker_run`을 실행합니다.
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### 예제 오류

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run results &#39;** error:배포 폴더를 찾을 수 없음&#39;

`docker_run.cmd`을(를) 실행할 때 __** 오류를 표시하는 오류가 표시됩니다.배포 폴더를 찾을 수 없습니다:__. 이 오류는 일반적으로 경로에 공백이 있기 때문에 발생합니다. 가능하면 폴더의 공백을 제거하거나 `aem-sdk` 폴더를 공백이 없는 경로로 이동합니다.

예를 들어, Windows 사용자 폴더는 종종 `<First name> <Last name>`이며 그 사이에 공백이 있습니다. 아래 예에서 폴더 `...\My User\...`에는 로컬 Dispatcher 도구의 `docker_run` 실행을 방해하는 공백이 포함됩니다. 공백이 Windows 사용자 폴더에 있는 경우 Windows가 중단되므로 이 폴더의 이름을 변경하지 말고 `aem-sdk` 폴더를 사용자가 완전히 수정할 수 있는 새 위치로 이동합니다. `aem-sdk` 폴더가 사용자의 홈 디렉토리에 있다고 가정할 지침은 새 위치로 조정해야 합니다.

#### 예제 오류

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run이 Windows{#troubleshooting-windows-compatible}에서 시작하지 못했습니다.

Windows에서 `docker_run`을(를) 실행하면 다음 오류가 발생하여 디스패처가 시작되지 않을 수 있습니다. 이 문제는 Windows의 Dispatcher에서 보고된 문제이며 향후 릴리스에서 해결됩니다.

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
+ [문서 다운로드](https://www.docker.com/)
+ [AEM 참조 웹 사이트(WKND) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager 발송자 설명서](https://docs.adobe.com/content/help/ko-KR/experience-manager-dispatcher/using/dispatcher.html)
