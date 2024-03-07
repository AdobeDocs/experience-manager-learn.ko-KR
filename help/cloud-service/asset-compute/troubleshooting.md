---
title: AEM Assets의 Asset compute 확장성 문제 해결
description: 다음은 AEM Assets용 사용자 지정 Asset compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제와 오류 및 해결 방법에 대한 색인입니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 335
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# asset compute 확장성 문제 해결

다음은 AEM Assets용 사용자 지정 Asset compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제와 오류 및 해결 방법에 대한 색인입니다.

## 개발{#develop}

### 렌디션이 부분적으로 그려지거나 손상되었습니다.{#rendition-returned-partially-drawn-or-corrupt}

+ __오류__: 렌디션이 잘못 렌더링되거나(이미지 시) 손상되어 열 수 없습니다.

  ![렌디션이 부분적으로 그려진 상태로 반환됨](./assets/troubleshooting/develop__await.png)

+ __원인__: 작업자의 `renditionCallback` 표현물을 완전히 쓰기 전에 함수를 종료하는 중입니다. `rendition.path`.
+ __해결 방법__: 사용자 지정 작업자 코드를 검토하고 다음을 사용하여 모든 비동기 호출이 동기화되도록 합니다. `await`.

## 개발 도구{#development-tool}

### asset compute 프로젝트에 Console.json 파일 누락{#missing-console-json}

+ __오류:__ 오류: 유효성 검사 시 필수 파일 누락(`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`비동기 setupAssetCompute( )에서`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)
+ __원인:__ 다음 `console.json` asset compute 프로젝트의 루트에 파일이 없습니다.
+ __해결 방법:__ 새 항목 다운로드 `console.json` Adobe I/O 프로젝트 구성
   1. console.adobe.io에서 Asset compute 프로젝트가 사용하도록 구성된 Adobe I/O 프로젝트를 엽니다
   1. 탭 __다운로드__ 오른쪽 상단의 단추
   1. 파일 이름을 사용하여 다운로드한 파일을 Asset compute 프로젝트의 루트에 저장합니다 `console.json`

### manifest.yml의 잘못된 YAML 들여쓰기{#incorrect-yaml-indentation}

+ __오류:__ YAMLException: 라인 X, 열 Y:(에서 표준 아웃을 통해)에 있는 매핑 항목의 잘못된 들여쓰기 `aio app run` 명령)
+ __원인:__ Yaml 파일은 공백이 흰색이어서 들여쓰기가 잘못되었을 수 있습니다.
+ __해결 방법:__ 검토 `manifest.yml` 모든 들여쓰기가 올바른지 확인합니다.

### memorySize 제한이 너무 낮게 설정되어 있습니다.{#memorysize-limit-is-set-too-low}

+ __오류:__  로컬 개발 서버 OpenWhskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true에서 HTTP 400(잘못된 요청) —> &quot;요청 컨텐츠가 잘못되었습니다. 요구 사항이 실패했습니다. 메모리 64MB가 134217728 B의 허용 임계값 미만입니다.&quot;
+ __원인:__ A `memorySize` 의 작업자에 대한 제한 `manifest.yml` 오류 메시지(바이트)에 의해 보고된 대로 이 최소 허용 임계값 이하로 설정되었습니다.
+ __해결 방법:__  리뷰 `memorySize` 의 제한 `manifest.yml` 모두 최소 허용 임계값보다 큰지 확인합니다.

### private.key가 누락되어 개발 도구를 시작할 수 없습니다.{#missing-private-key}

+ __오류:__ 로컬 개발 서버 오류: validatePrivateKeyFile에 필수 파일이 없습니다.... (에서 표준 아웃을 통해) `aio app run` 명령)
+ __원인:__ 다음 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 값: `.env` 파일, 다음을 가리키지 않음 `private.key` 또는 `private.key` 은(는) 현재 사용자가 읽을 수 없습니다.
+ __해결 방법:__ 리뷰 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 값: `.env` 파일에 대한 전체 절대 경로가 포함되어 있는지 확인합니다. `private.key` 을 클릭합니다.

### 소스 파일 드롭다운이 잘못됨{#source-files-dropdown-incorrect}

Asset compute 개발 도구는 오래된 데이터를 가져오는 상태로 들어갈 수 있으며 __소스 파일__ 드롭다운에 잘못된 항목이 표시됩니다.

+ __오류:__ 소스 파일 드롭다운에 잘못된 항목이 표시됩니다.
+ __원인:__ 오래된 캐시된 브라우저 상태로 인해
+ __해결 방법:__ 브라우저에서 브라우저 탭의 &quot;애플리케이션 상태&quot;, 브라우저 캐시, 로컬 저장소 및 서비스 작업자를 완전히 지웁니다.

### 누락되었거나 잘못된 devToolToken 쿼리 매개 변수{#missing-or-invalid-devtooltoken-query-parameter}

+ __오류:__ asset compute 개발 도구의 &quot;승인되지 않은&quot; 알림
+ __원인:__ `devToolToken` 이(가) 누락되었거나 잘못되었습니다.
+ __해결 방법:__ asset compute 개발 도구 브라우저 창을 닫고 다음을 통해 시작된 실행 중인 개발 도구 프로세스를 종료합니다. `aio app run` 명령 및 다시 시작 개발 도구(사용 `aio app run`).

### 소스 파일을 제거할 수 없음{#unable-to-remove-source-files}

+ __오류:__ 개발 도구 UI에서 추가된 소스 파일을 제거할 방법은 없습니다
+ __원인:__ 이 기능은 구현되지 않았습니다
+ __해결 방법:__ 에 정의된 자격 증명을 사용하여 클라우드 스토리지 공급자에 로그인합니다. `.env`. 개발 도구에서 사용하는 컨테이너를 찾습니다( 에도 지정됨). `.env`)에서 로 이동합니다. __소스__ 폴더를 만들고 소스 이미지를 삭제합니다. 다음에 설명된 단계를 수행해야 할 수 있습니다 [소스 파일 드롭다운이 잘못됨](#source-files-dropdown-incorrect) 삭제된 소스 파일이 개발 도구 &quot;애플리케이션 상태&quot;에서 로컬로 캐시될 수 있으므로 드롭다운에 계속 표시되는 경우.

  ![Microsoft Azure Blob 저장소](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 테스트{#test}

### 테스트 실행 중에 생성된 렌디션 없음{#test-no-rendition-generated}

+ __오류:__ 실패: 생성된 렌디션이 없습니다.
+ __원인:__ JavaScript 구문 오류와 같은 예기치 않은 오류로 인해 작업자가 렌디션을 생성하지 못했습니다.
+ __해결 방법:__ 테스트 실행 검토 `test.log` 위치: `/build/test-results/test-worker/test.log`. 이 파일에서 실패한 테스트 사례에 해당하는 섹션을 찾아 오류를 검토합니다.

  ![문제 해결 - 생성된 렌디션이 없음](./assets/troubleshooting/test__no-rendition-generated.png)

### 테스트가 잘못된 렌디션을 생성하여 테스트 실패{#tests-generates-incorrect-rendition}

+ __오류:__ 실패: 렌디션 &#39;rendition.xxx&#39;가 예상과 다릅니다.
+ __원인:__ 작업자가 표현물과 같지 않은 표현물을 출력합니다. `rendition.<extension>` 테스트 사례에 제공됩니다.
   + 예상한 경우 `rendition.<extension>` 파일이 테스트 사례에서 로컬로 생성된 렌디션과 정확히 동일한 방식으로 작성되지 않은 경우 비트에 약간의 차이가 발생할 수 있으므로 테스트가 실패할 수 있습니다. 예를 들어 Asset compute 작업자가 API를 사용하여 대비를 변경하고, Adobe Photoshop CC의 대비를 조정하여 예상 결과를 만드는 경우 파일이 동일하게 표시될 수 있지만 비트의 사소한 변형이 다를 수 있습니다.
+ __해결 방법:__ 로 이동하여 테스트의 렌디션 출력 검토 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`를 누르고 테스트 사례에서 예상되는 렌디션 파일과 비교합니다. 정확한 예상 에셋을 만들려면 다음 중 하나를 수행하십시오.
   + 개발 도구를 사용하여 렌디션을 생성하고 올바른 렌디션인지 확인한 다음 이를 예상 렌디션 파일로 사용합니다
   + 또는 다음 위치에서 테스트 생성 파일의 유효성을 검사합니다. `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`를 클릭하고, 올바른지 확인한 다음, 이를 예상 렌디션 파일로 사용합니다

## 디버그

### 디버거가 연결되지 않음{#debugger-does-not-attach}

+ __오류__: 시작을 처리하는 동안 오류 발생: 오류: 디버그 대상에 연결할 수 없음...
+ __원인__: Docker Desktop이 로컬 시스템에서 실행되고 있지 않습니다. VS 코드 디버그 콘솔(보기 > 디버그 콘솔)을 검토하여 이 오류를 확인합니다.
+ __해결 방법__: 시작 [Docker Desktop 및 필수 Docker 이미지가 설치되었는지 확인](./set-up/development-environment.md#docker).

### 중단점이 일시 중단되지 않음{#breakpoints-no-pausing}

+ __오류__: 디버그 가능한 개발 도구에서 Asset compute 작업자를 실행할 때 VS 코드가 중단점에서 일시 중지되지 않습니다.

#### VS 코드 디버거가 연결되지 않음{#vs-code-debugger-not-attached}

+ __원인:__ VS 코드 디버거가 중지/연결 해제되었습니다.
+ __해결 방법:__ VS 코드 디버거를 다시 시작하고 VS 코드 디버그 출력 콘솔(보기 > 디버그 콘솔)을 확인하여 연결되어 있는지 확인합니다.

#### 작업자 실행이 시작된 후 연결된 VS 코드 디버거{#vs-code-debugger-attached-after-worker-execution-began}

+ __원인:__ VS 코드 디버거가 탭하기 전에 첨부되지 않았습니다. __실행__ 개발 도구에서 참조할 수 있습니다.
+ __해결 방법:__ VS 코드의 디버그 콘솔(보기 > 디버그 콘솔)을 검토하여 디버거가 연결되었는지 확인한 다음 개발 도구에서 Asset compute 작업자를 다시 실행합니다.

### 디버깅하는 동안 작업자 시간 초과{#worker-times-out-while-debugging}

+ __오류__: 디버그 콘솔 보고서 &quot;작업이 -XXX밀리초 내에 시간 초과됩니다.&quot; 또는 [Asset compute 개발 도구](./develop/development-tool.md) 렌디션 미리 보기가 무기한 회전되거나
+ __원인__: 다음에 정의된 작업자 시간 초과 [manifest.yml](./develop/manifest.md) 디버깅하는 동안 이 초과되었습니다.
+ __해결 방법__: 에서 작업자의 시간 제한을 일시적으로 늘립니다. [manifest.yml](./develop/manifest.md) 또는 디버깅 활동을 가속화하십시오.

### 디버거 프로세스를 종료할 수 없습니다.{#cannot-terminate-debugger-process}

+ __오류__: `Ctrl-C` 명령줄에서 디버거 프로세스를 종료하지 않습니다(`npx adobe-asset-compute devtool`).
+ __원인__: 의 버그 `@adobe/aio-cli-plugin-asset-compute` 1.3.x, 결과: `Ctrl-C` 종료 명령으로 인식되지 않습니다.
+ __해결 방법__: 업데이트 `@adobe/aio-cli-plugin-asset-compute` 대상 버전 1.4.1+

  ```
  $ aio update
  ```

  ![문제 해결 - aio 업데이트](./assets/troubleshooting/debug__terminate.png)

## 배포{#deploy}

### AEM의 에셋에서 사용자 정의 렌디션 누락{#custom-rendition-missing-from-asset}

+ __오류:__ 새 자산 및 다시 처리된 자산이 처리되었지만 사용자 지정 렌디션이 누락되었습니다

#### 처리 프로필이 상위 폴더에 적용되지 않음

+ __원인:__ 사용자 지정 작업자를 사용하는 처리 프로필이 있는 폴더에 자산이 없습니다.
+ __해결 방법:__ 자산의 상위 폴더에 처리 프로필 적용

#### 처리 프로필이 하위 처리 프로필로 대체됨

+ __원인:__ 에셋은 사용자 정의 작업자 처리 프로필이 적용된 폴더 아래에 있지만, 고객 작업자를 사용하지 않는 다른 처리 프로필이 해당 폴더와 에셋 사이에 적용되었습니다.
+ __해결 방법:__ 두 처리 프로필을 결합하거나 조정하며 중간 처리 프로필을 제거합니다.

### AEM에서 자산 처리 실패{#asset-processing-fails}

+ __오류:__ 자산에 표시된 자산 처리 실패 배지
+ __원인:__ 사용자 정의 작업자를 실행하는 동안 오류가 발생했습니다.
+ __해결 방법:__ 다음 지침을 따르십시오. [Adobe I/O Runtime 활성화 디버깅](./test-debug/debug.md#aio-app-logs) 사용 `aio app logs`.
