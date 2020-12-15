---
title: asset compute 작업자 디버그
description: asset compute 작업자는 간단한 디버그 로그 명령문에서부터 연결된 VS 코드를 원격 디버거로, AEM에서 Cloud Service으로 시작한 Adobe I/O Runtime에서 활성화할 로그를 가져오는 등 다양한 방법으로 디버깅할 수 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# asset compute 작업자 디버그

asset compute 작업자는 간단한 디버그 로그 명령문에서부터 연결된 VS 코드를 원격 디버거로, AEM에서 Cloud Service으로 시작한 Adobe I/O Runtime에서 활성화할 로그를 가져오는 등 다양한 방법으로 디버깅할 수 있습니다.

## 로깅

디버깅 Asset compute 근로자의 가장 기본적인 형식은 작업자 코드에 기존 `console.log(..)` 문을 사용합니다. `console` JavaScript 객체는 암시적 글로벌 개체이므로 모든 컨텍스트에 항상 존재하므로 가져오거나 필요로 할 필요가 없습니다.

이러한 로그 명령문은 Asset compute 근로자의 실행 방법에 따라 다르게 검토할 수 있습니다.

+ `aio app run`에서 로그는 표준 출력 및 [개발 도구의 ](../develop/development-tool.md) 활성화 로그로 인쇄됩니다.
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ `aio app test`에서 로그는 `/build/test-results/test-worker/test.log`로 인쇄됩니다.
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ `wskdebug`을 사용하여 로그 명령문은 VS 코드 디버그 콘솔(보기 > 디버그 콘솔)로 인쇄되며, 표준은 출력
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ `aio app logs`을 사용하여 로그 명령문이 정품 인증 로그 출력으로 인쇄됩니다.

## 연결된 디버거를 통한 원격 디버깅

>[!WARNING]
>
>wskdebug와의 호환성을 위해 Microsoft Visual Studio 코드 1.48.0 이상 사용

[wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈은 VS 코드에서 중단점을 설정하고 코드를 단계별로 진행하는 기능을 포함하여 Asset compute 작업자에 디버거를 첨부할 수 있도록 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_wskdebug를 사용하여 Asset compute 작업자 디버깅의 클릭스루(오디오 없음)_

1. [wskdebug](../set-up/development-environment.md#wskdebug) 및 [ngroup](../set-up/development-environment.md#ngork) npm 모듈이 설치되어 있는지 확인합니다.
1. [Docker Desktop 및 지원 Docker 이미지](../set-up/development-environment.md#docker)이(가) 설치되어 실행되고 있는지 확인합니다.
1. 활성 실행 중인 개발 도구 인스턴스를 모두 닫습니다.
1. `aio app deploy`을(를) 사용하여 최신 코드를 배포하고 배포된 작업 이름(`[...]` 사이의 이름)을 기록합니다. 8단계에서 `launch.json`을(를) 업데이트하는 데 사용됩니다.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. `npx adobe-asset-compute devtool` 명령을 사용하여 Asset compute 개발 도구의 새 인스턴스를 시작합니다.
1. VS 코드에서 왼쪽 탐색 창의 디버그 아이콘을 누릅니다.
   + 메시지가 표시되면 __launch.json 파일 만들기 > Node.js__&#x200B;를 눌러 새 `launch.json` 파일을 만듭니다.
   + 그렇지 않은 경우 __프로그램 시작__ 드롭다운 오른쪽에 있는 __톱니바퀴__ 아이콘을 눌러 편집기에서 기존 `launch.json`을 엽니다.
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

1. 드롭다운에서 새 __wskdebug__&#x200B;을 선택합니다.
1. __wskdebug__ 드롭다운 왼쪽에 있는 녹색 __실행__ 단추를 누릅니다.
1. `/actions/worker/index.js`을 열고 행 번호의 왼쪽에 있는 을 눌러 중단점 1을 추가합니다. 6단계에서 연 Asset compute 개발 도구 웹 브라우저 창으로 이동합니다.
1. __실행__ 단추를 눌러 워커를 실행합니다.
1. VS 코드로 돌아가서 `/actions/worker/index.js`으로 이동하여 코드를 단계별로 진행합니다.
1. 디버그 가능 개발 도구를 종료하려면 6단계에서 `npx adobe-asset-compute devtool` 명령을 실행한 터미널에서 `Ctrl-C`을 탭합니다.

## Adobe I/O Runtime{#aio-app-logs}에서 로그 액세스

[CLOUD SERVICE의 경우 Adobe I/O Runtime에서 직접 호출하면 처리 프로필](../deploy/processing-profiles.md) 을 통해 Asset compute 작업자를 활용합니다. 이러한 호출에는 로컬 개발이 포함되지 않으므로 Asset compute 개발 도구나 wskdebug와 같은 로컬 도구를 사용하여 해당 실행을 디버깅할 수 없습니다. 대신 Adobe I/O CLI를 사용하여 Adobe I/O Runtime의 특정 작업 영역에서 실행되는 작업자로부터 로그를 가져올 수 있습니다.

1. 디버깅이 필요한 작업 영역을 기준으로 [작업 공간별 환경 변수](../deploy/runtime.md)가 `AIO_runtime_namespace` 및 `AIO_runtime_auth`을 통해 설정되었는지 확인합니다.
1. 명령줄에서 `aio app logs` 실행
   + 작업 영역에서 트래픽이 많은 경우 `--limit` 플래그를 통해 활성화 로그 수를 확장합니다.
      `$ aio app logs --limit=25`
1. 가장 최근(제공된 `--limit`까지) 활동 로그는 검토를 위한 명령 출력으로 반환됩니다.

   ![aio 앱 로그](./assets/debug/aio-app-logs.png)

## 문제 해결

+ [디버거가 연결되지 않음](../troubleshooting.md#debugger-does-not-attach)
+ [중단점이 일시 중지되지 않음](../troubleshooting.md#breakpoints-no-pausing)
+ [VS 코드 디버거가 연결되지 않음](../troubleshooting.md#vs-code-debugger-not-attached)
+ [작업자 실행이 시작된 후 연결된 VS 코드 디버거](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [디버깅 시 작업 시간 초과](../troubleshooting.md#worker-times-out-while-debugging)
+ [디버거 프로세스를 종료할 수 없습니다.](../troubleshooting.md#cannot-terminate-debugger-process)
