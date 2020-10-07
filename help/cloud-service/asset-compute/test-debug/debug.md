---
title: 자산 계산 작업자 디버깅
description: 자산 계산 작업자는 간단한 디버그 로그 명령문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며 AEM에서 Cloud Service으로 시작한 Adobe I/O Runtime에서 활성화 로그를 가져올 수도 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# 자산 계산 작업자 디버깅

자산 계산 작업자는 간단한 디버그 로그 명령문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며 AEM에서 Cloud Service으로 시작한 Adobe I/O Runtime에서 활성화 로그를 가져올 수도 있습니다.

## 로깅

가장 기본적인 디버깅 자산 계산 작업자는 작업자 코드의 기존 `console.log(..)` 문을 사용합니다. JavaScript `console` 객체는 암묵적인 글로벌 객체이므로 모든 컨텍스트에 항상 존재하므로 가져오거나 필요로 할 필요가 없습니다.

이러한 로그 문은 자산 계산 근로자의 실행 방법에 따라 다르게 검토할 수 있습니다.

+ 로그 `aio app run`는 인쇄에서 표준으로, [개발 툴의](../develop/development-tool.md) 활성화 로그로
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 로그 `aio app test`는 `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 를 `wskdebug`사용하면 로그 문이 VS 코드 디버그 콘솔(보기 > 디버그 콘솔)로 인쇄되고 표준으로 출력
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 를 `aio app logs`사용하면 로그 문이 활성화 로그 출력에 인쇄됩니다

## 연결된 디버거를 통한 원격 디버깅

>[!WARNING]
>
>wskdebug와의 호환성을 위해 Microsoft Visual Studio 코드 1.48.0 이상 사용

wskdebug [npm](https://www.npmjs.com/package/@openwhisk/wskdebug) 모듈은 VS 코드에서 중단점을 설정하고 코드를 단계별로 진행하는 기능을 포함하여 자산 계산 작업자에 디버거를 첨부할 수 있도록 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)
_wskdebug(오디오 없음)를 사용하여 자산 계산 작업자 디버깅의 클릭스루_

1. wskdebug [및](../set-up/development-environment.md#wskdebug) ngroup [npm 모듈이](../set-up/development-environment.md#ngork) 설치되어 있는지 확인합니다.
1. Docker [Desktop 및 지원 Docker 이미지](../set-up/development-environment.md#docker) 설치 및 실행 확인
1. 활성 실행 중인 개발 도구 인스턴스를 모두 닫습니다.
1. 배포된 작업 이름 `aio app deploy` (이름 사이)을 사용하여 최신 코드를 배포하고 `[...]`기록합니다. 8단계에서 업데이트를 업데이트하는 데 사용됩니다 `launch.json` .

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. 명령을 사용하여 새로운 에셋 계산 개발 도구 인스턴스 시작 `npx adobe-asset-compute devtool`
1. VS 코드에서 왼쪽 탐색 영역에서 디버그 아이콘을 누릅니다
   + 메시지가 표시되면 launch.json 파일 __만들기 > Node.js__ 를 눌러 새 `launch.json` 파일을 만듭니다.
   + 그렇지 않은 경우 프로그램 __시작__ 드롭다운 오른쪽에 있는 기어 ____ 아이콘을 탭하여 편집기에 기존 `launch.json` 을 엽니다.
1. 다음 JSON 개체 구성을 `configurations` 배열에 추가합니다.

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. 드롭다운에서 새 __Swskdebug__ 선택
1. wskdebug __드롭다운__ 왼쪽에 있는 녹색 실행 __단추를__ 누릅니다
1. 줄 번호 왼쪽 `/actions/worker/index.js` 을 열고 눌러 중단점 1을 추가합니다. 6단계에서 연 자산 계산 개발 도구 웹 브라우저 창으로 이동합니다.
1. Run __단추를__ 눌러 Worker를 실행합니다.
1. VS 코드로 돌아가 코드를 `/actions/worker/index.js` 탐색하고 단계별로 진행합니다.
1. 디버그 가능 개발 도구를 종료하려면 6단계 `Ctrl-C` 에서 명령을 실행한 터미널 `npx adobe-asset-compute devtool` 을 누릅니다

## Adobe I/O Runtime에서 로그 액세스{#aio-app-logs}

[AEM은 처리 프로필을 통해](../deploy/processing-profiles.md) 자산 계산 작업자를 Adobe I/O Runtime에서 직접 불러와 활용합니다. 이러한 호출에는 로컬 개발이 포함되지 않으므로 Asset Compute Development Tool 또는 wskdebug와 같은 로컬 도구를 사용하여 실행을 디버깅할 수 없습니다. 대신 Adobe I/O CLI를 사용하여 Adobe I/O Runtime의 특정 작업 영역에서 실행되는 작업자로부터 로그를 가져올 수 있습니다.

1. 디버깅이 필요한 작업 영역을 기반으로 [및](../deploy/runtime.md) 을 통해 `AIO_runtime_namespace` 작업 공간별 환경 변수를 `AIO_runtime_auth`설정할 수 있습니다.
1. 명령줄에서 실행 `aio app logs`
   + 작업 영역에 트래픽이 많은 경우 플래그를 통해 활성화 로그 수를 `--limit` 확장합니다.
      `$ aio app logs --limit=25`
1. 가장 최근(제공된 `--limit`활성화 로그)의 경우 검토 명령 출력으로 반환됩니다.

   ![aio 앱 로그](./assets/debug/aio-app-logs.png)

## 문제 해결

### 디버거가 연결되지 않음

+ __오류__:론치를 처리하는 동안 오류 발생:오류:...에서 디버그 타겟에 연결할 수 없습니다.
+ __원인__:Docker Desktop이 로컬 시스템에서 실행되고 있지 않습니다. VS 코드 디버그 콘솔([보기] > [디버그 콘솔])을 검토하여 이 오류가 보고되었는지 확인합니다.
+ __해상도__:Docker [Desktop을 시작하고 필수 Docker 이미지가 설치되었는지](../set-up/development-environment.md#docker)확인합니다.

### 중단점이 일시 중지되지 않음

+ __오류__:디버그 가능 개발 도구에서 자산 계산 작업자를 실행할 때 VS 코드가 중단점에서 일시 중지되지 않습니다.

#### VS 코드 디버거가 연결되지 않았습니다.

+ __원인:__ VS 코드 디버거가 중지/연결이 끊어졌습니다.
+ __해결 방법:__ VS 코드 디버거를 다시 시작하고 VS 코드 디버그 출력 콘솔(보기 > 디버그 콘솔)을 시청하여 VS 코드 디버그 디버거가 첨부되는지 확인합니다.

#### 작업자 실행이 시작된 후 연결된 VS 코드 디버거

+ __원인:__ VS 코드 디버거가 개발 도구에서 __실행을 탭하기__ 전에 연결되지 않았습니다.
+ __해결 방법:__ VS 코드의 디버그 콘솔(보기 > 디버그 콘솔)을 검토한 다음 개발 도구에서 자산 계산 작업자를 다시 실행하여 디버거가 연결되었는지 확인합니다.

### 디버깅 시 작업 시간 초과

+ __오류__:디버그 콘솔에서는 &quot;작업 시간이 -XXX 밀리초&quot; 또는 [Asset Compute Development Tool의](../develop/development-tool.md) 변환 미리 보기가 무기한 또는
+ __원인__:디버깅 중에 manifest.yml에 정의된 작업자 시간 [이](../develop/manifest.md) 초과되었습니다.
+ __해상도__:manifest.yml에서 작업자의 시간 초과를 [일시적으로 늘리거나 디버깅](../develop/manifest.md) 활동을 가속화합니다.

### 디버거 프로세스를 종료할 수 없습니다.

+ __오류__: `Ctrl-C` 명령줄에서 디버거 프로세스를 종료하지 않습니다(`npx adobe-asset-compute devtool`).
+ __원인__:1.3.x의 버그로 인해 종료 명령으로 인식되지 `@adobe/aio-cli-plugin-asset-compute` `Ctrl-C` 않습니다.
+ __해상도__:버전 1.4.1+ `@adobe/aio-cli-plugin-asset-compute` 로 업데이트

   ```
   $ aio update
   ```

   ![문제 해결 - aio 업데이트](./assets/debug/troubleshooting__terminate.png)