---
title: asset compute 작업자 디버깅
description: Asset compute 작업자는 간단한 디버그 로그 문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며, as a Cloud Service에서 시작한 Adobe I/O RuntimeAEM 에서 활성화를 위한 로그를 가져올 수도 있습니다.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# asset compute 작업자 디버깅

Asset compute 작업자는 간단한 디버그 로그 문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며, as a Cloud Service에서 시작한 Adobe I/O RuntimeAEM 에서 활성화를 위한 로그를 가져올 수도 있습니다.

## 로깅

디버깅 Asset compute 작업자의 가장 기본적인 형식은 기존 형식을 사용합니다 `console.log(..)` 작업자 코드의 문입니다. 다음 `console` JavaScript 개체는 암시적 전역 개체이므로 모든 컨텍스트에 항상 있으므로 가져오거나 필요로 할 필요가 없습니다.

이러한 로그 문은 Asset compute 작업자가 실행되는 방식에 따라 다르게 검토할 수 있습니다.

+ 출처: `aio app run`, 표준 출력 및 [개발 도구](../develop/development-tool.md) 활성화 로그
   ![aio 앱 실행 console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 출처: `aio app test`, 로그 인쇄 대상 `/build/test-results/test-worker/test.log`
   ![aio 앱 테스트 console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 사용 `wskdebug`, 로그 문은 VS 코드 디버그 콘솔(보기 > 디버그 콘솔)에 인쇄되며 표준 출력
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 사용 `aio app logs`, 로그 문은 활성화 로그 출력으로 인쇄됩니다.

## 연결된 디버거를 통한 원격 디버깅

>[!WARNING]
>
>wskdebug와의 호환성을 위해 Microsoft Visual Studio Code 1.48.0 이상 사용

다음 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈에서는 VS 코드에서 중단점을 설정하고 코드를 단계별로 실행하는 기능을 포함하여 디버거를 Asset compute 작업자에 연결할 수 있도록 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_wskdebug를 사용하여 Asset compute 작업자 디버깅의 클릭스루(오디오 없음)_

1. 확인 [wskdebug](../set-up/development-environment.md#wskdebug) 및 [응록](../set-up/development-environment.md#ngork) npm 모듈 설치
1. 확인 [Docker Desktop 및 지원되는 Docker 이미지](../set-up/development-environment.md#docker) 설치 및 실행 중
1. 실행 중인 개발 도구 인스턴스를 모두 닫습니다.
1. 를 사용하여 최신 코드 배포 `aio app deploy`  및 배포된 작업 이름(다음 사이의 이름)을 기록합니다. `[...]`). 이 확장은 다음을 업데이트하는 데 사용됩니다. `launch.json` 8단계.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 명령을 사용하여 Asset compute 개발 도구의 새 인스턴스 시작 `npx adobe-asset-compute devtool`
1. VS 코드에서 왼쪽 탐색 영역에 있는 디버그 아이콘을 탭합니다
   + 메시지가 표시되면 을 누릅니다. __launch.json 파일 만들기 > Node.js__ 새로 만들려면 `launch.json` 파일.
   + 그렇지 않으면 __톱니바퀴__ 아이콘 의 오른쪽 __프로그램 실행__ 드롭다운을 클릭하여 기존 항목 열기 `launch.json` 를 입력합니다.
1. 에 다음 JSON 개체 구성 추가 `configurations` 배열:

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

1. 새 항목 선택 __wskdebug__ 드롭다운에서
1. 녹색을 탭합니다 __실행__ 의 왼쪽에 있는 버튼 __wskdebug__ 드롭다운
1. 열기 `/actions/worker/index.js` 줄 번호 왼쪽을 탭하여 줄바꿈 지점 1을 추가합니다. 6단계에서 연 Asset compute 개발 도구 웹 브라우저 창으로 이동합니다
1. 탭 __실행__ 작업자를 실행하는 버튼
1. VS 코드로 돌아가서 `/actions/worker/index.js` 코드를 단계별로 실행합니다.
1. 디버그 가능 개발 도구를 종료하려면 다음을 누르십시오. `Ctrl-C` 실행된 터미널에서 `npx adobe-asset-compute devtool` 6단계의 명령

## Adobe I/O Runtime에서 로그 액세스{#aio-app-logs}

[AEM as a Cloud Service에서는 처리 프로필을 통해 Asset compute 작업자를 활용합니다](../deploy/processing-profiles.md) Adobe I/O Runtime에서 직접 호출하여 이러한 호출은 로컬 개발을 포함하지 않으므로 Asset compute 개발 도구 또는 wskdebug와 같은 로컬 도구를 사용하여 실행을 디버깅할 수 없습니다. 대신 Adobe I/O CLI를 사용하여 Adobe I/O Runtime의 특정 작업 영역에서 실행되는 작업자로부터 로그를 가져올 수 있습니다.

1. 다음을 확인합니다. [작업 영역별 환경 변수](../deploy/runtime.md) 다음을 통해 설정됨 `AIO_runtime_namespace` 및 `AIO_runtime_auth`: 디버깅이 필요한 작업 영역을 기반으로 합니다.
1. 명령줄에서 을 실행합니다. `aio app logs`
   + 작업 공간에서 많은 트래픽이 발생하는 경우 다음을 통해 활성화 로그 수를 확장합니다. `--limit` 플래그:
      `$ aio app logs --limit=25`
1. 가장 최근 (제공된 항목까지) `--limit`) 검토를 위해 활성화 로그가 명령의 출력으로 반환됩니다.

   ![aio 앱 로그](./assets/debug/aio-app-logs.png)

## 문제 해결

+ [디버거가 연결되지 않음](../troubleshooting.md#debugger-does-not-attach)
+ [중단점이 일시 중단되지 않음](../troubleshooting.md#breakpoints-no-pausing)
+ [VS 코드 디버거가 연결되지 않음](../troubleshooting.md#vs-code-debugger-not-attached)
+ [작업자 실행이 시작된 후 연결된 VS 코드 디버거](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [디버깅하는 동안 작업자 시간 초과](../troubleshooting.md#worker-times-out-while-debugging)
+ [디버거 프로세스를 종료할 수 없습니다.](../troubleshooting.md#cannot-terminate-debugger-process)
