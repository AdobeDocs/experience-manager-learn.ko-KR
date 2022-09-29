---
title: asset compute 작업자 디버깅
description: asset compute 작업자는 간단한 디버그 로그 명령문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며 AEM Adobe I/O Runtime에서 시작된 as a Cloud Service에서 활성화 로그를 가져올 수도 있습니다.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# asset compute 작업자 디버깅

asset compute 작업자는 간단한 디버그 로그 명령문에서 원격 디버거로 연결된 VS 코드에 이르기까지 여러 가지 방법으로 디버깅할 수 있으며 AEM Adobe I/O Runtime에서 시작된 as a Cloud Service에서 활성화 로그를 가져올 수도 있습니다.

## 로깅

가장 기본적인 디버깅 Asset compute 작업자는 `console.log(..)` 작업자 코드의 구문입니다. 다음 `console` JavaScript 개체는 암시적 글로벌 개체이므로 모든 컨텍스트에 항상 있으므로 가져오거나 필요로 할 필요가 없습니다.

이러한 로그 문은 Asset compute 작업자 실행 방식에 따라 다르게 검토할 수 있습니다.

+ From `aio app run`로 로그인하면 표준 출력 및 [개발 도구](../develop/development-tool.md) 활성화 로그
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ From `aio app test`, 인쇄 로그 `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 사용 `wskdebug`, 로그 문은 VS 코드 디버그 콘솔(보기 > 디버그 콘솔)에 인쇄되며, 표준은
   ![wskdebug console.log(...](./assets/debug/console-log__wskdebug.png)
+ 사용 `aio app logs`, 로그 문은 활성화 로그 출력에 인쇄됩니다.

## 연결된 디버거를 통한 원격 디버깅

>[!WARNING]
>
>wskdebug와의 호환성을 위해 Microsoft Visual Studio 코드 1.48.0 이상을 사용합니다.

다음 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm 모듈 에서는 VS 코드에서 중단점을 설정하고 코드를 단계별로 수행하는 기능을 포함하여 Asset compute 작업자에게 디버거를 첨부할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_wskdebug(오디오 없음)를 사용하여 Asset compute 작업자를 디버깅하는 클릭스루_

1. 확인 [wskdebug](../set-up/development-environment.md#wskdebug) 및 [ngrok](../set-up/development-environment.md#ngork) npm 모듈이 설치됨
1. 확인 [Docker Desktop 및 지원 Docker 이미지](../set-up/development-environment.md#docker) 설치 및 실행 중
1. 개발 도구의 실행 중인 활성 인스턴스를 모두 닫습니다.
1. 를 사용하여 최신 코드 배포 `aio app deploy`  및 배포된 작업 이름(이름 사이) `[...]`). 이 템플릿은 `launch.json` 8단계에서

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 명령을 사용하여 Asset compute 개발 도구의 새 인스턴스를 시작합니다 `npx adobe-asset-compute devtool`
1. VS 코드에서 왼쪽 탐색에서 디버그 아이콘을 탭합니다
   + 메시지가 표시되면 __launch.json 파일 만들기 > Node.js__ 새 `launch.json` 파일.
   + 그렇지 않은 경우 __기어__ 아이콘 을 클릭합니다. __Launch 프로그램__ 드롭다운을 클릭하여 기존 `launch.json` 참조하십시오.
1. 다음 JSON 개체 구성을 `configurations` 배열:

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

1. 새 항목을 선택합니다 __wskdebug__ 드롭다운에서
1. 녹색 탭 __실행__ 단추 __wskdebug__ 드롭다운
1. 열기 `/actions/worker/index.js` 그리고 줄 번호의 왼쪽에 있는 를 탭하여 중간점 1을 추가합니다. 6단계에서 열린 Asset compute 개발 도구 웹 브라우저 창으로 이동합니다
1. 탭하기 __실행__ 작업자를 실행하는 단추
1. VS 코드로 돌아가 `/actions/worker/index.js` 그리고 코드를 단계별로 수행합니다
1. 디버그 가능 개발 도구를 종료하려면 `Ctrl-C` 터미널에 있는 `npx adobe-asset-compute devtool` 6단계의 명령

## Adobe I/O Runtime에서 로그 액세스{#aio-app-logs}

[AEM as a Cloud Service은 처리 프로필을 통해 Asset compute 작업자를 활용합니다](../deploy/processing-profiles.md) 를 Adobe I/O Runtime에서 직접 호출하여 이러한 호출에는 로컬 개발이 포함되지 않으므로 Asset compute 개발 도구 또는 wskdebug와 같은 로컬 도구를 사용하여 해당 실행을 디버깅할 수 없습니다. 대신 Adobe I/O CLI를 사용하여 Adobe I/O Runtime의 특정 작업 공간에서 실행된 작업자로부터 로그를 가져올 수 있습니다.

1. 다음을 확인합니다. [작업 공간별 환경 변수](../deploy/runtime.md) 를 통해 설정됨 `AIO_runtime_namespace` 및 `AIO_runtime_auth`를 설정하는 것이 좋습니다.
1. 명령줄에서 를 실행합니다 `aio app logs`
   + 작업 공간에서 트래픽이 많이 발생하는 경우 를 통해 활성화 로그 수를 확장합니다. `--limit` 플래그:
      `$ aio app logs --limit=25`
1. 가장 최근(제공된 최대) `--limit`) 활동 로그는 검토를 위한 명령의 출력으로 반환됩니다.

   ![aio 앱 로그](./assets/debug/aio-app-logs.png)

## 문제 해결

+ [디버거가 첨부되지 않음](../troubleshooting.md#debugger-does-not-attach)
+ [중단점 일시 중지 안 함](../troubleshooting.md#breakpoints-no-pausing)
+ [VS 코드 디버거가 첨부되지 않음](../troubleshooting.md#vs-code-debugger-not-attached)
+ [작업자 실행이 시작된 후 첨부된 VS 코드 디버거](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [디버깅하는 동안 작업자 시간 초과](../troubleshooting.md#worker-times-out-while-debugging)
+ [디버거 프로세스를 종료할 수 없습니다.](../troubleshooting.md#cannot-terminate-debugger-process)
