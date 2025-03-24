---
title: AEM Assets의 Asset Compute 확장성 문제 해결
description: 다음은 AEM Assets용 사용자 지정 Asset Compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제와 오류 및 해결 방법에 대한 색인입니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# Asset Compute 확장성 문제 해결

다음은 AEM Assets용 사용자 지정 Asset Compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제와 오류 및 해결 방법에 대한 색인입니다.

## 개발{#develop}

### 렌디션이 부분적으로 그려지거나 손상되었습니다.{#rendition-returned-partially-drawn-or-corrupt}

+ __오류__: 렌디션이 잘못 렌더링되거나(이미지의 경우) 손상되어 열 수 없습니다.

  ![렌디션이 부분적으로 그려졌습니다](./assets/troubleshooting/develop__await.png)

+ __원인__: 렌디션을 `rendition.path`에 완전히 쓰기 전에 작업자의 `renditionCallback` 함수를 종료하는 중입니다.
+ __해결 방법__: 사용자 지정 작업자 코드를 검토하고 `await`을(를) 사용하여 모든 비동기 호출이 동기화되는지 확인하십시오.

## 개발 도구{#development-tool}

### Asset Compute 프로젝트에서 Console.json 파일 누락{#missing-console-json}

+ __오류:__ 오류: 비동기 setupAssetCompute(`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)에 유효성 검사(`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`)에 필요한 파일이 없습니다.
+ __원인:__ Asset Compute 프로젝트의 루트에 `console.json` 파일이 없습니다.
+ __해결 방법:__ Adobe I/O 프로젝트에서 새 `console.json` 다운로드
   1. console.adobe.io에서 Asset Compute 프로젝트가 사용하도록 구성된 Adobe I/O 프로젝트를 엽니다
   1. 오른쪽 상단에서 __다운로드__ 단추를 탭합니다.
   1. 파일 이름 `console.json`을(를) 사용하여 다운로드한 파일을 Asset Compute 프로젝트의 루트에 저장합니다.

### manifest.yml의 잘못된 YAML 들여쓰기{#incorrect-yaml-indentation}

+ __오류:__ YAMLException: `aio app run` 명령에서 표준 출력을 통해 X행, Y열에 있는 매핑 항목의 들여쓰기가 잘못되었습니다.
+ __원인:__ Yaml 파일은 공백이 흰색이어서 들여쓰기가 잘못되었을 수 있습니다.
+ __해결 방법:__ `manifest.yml`을(를) 검토하고 들여쓰기가 모두 올바른지 확인하십시오.

### memorySize 제한이 너무 낮게 설정되어 있습니다.{#memorysize-limit-is-set-too-low}

+ __오류:__ 로컬 개발 서버 OpenWhskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true에서 HTTP 400(잘못된 요청)을 반환했습니다. —> &quot;요청 컨텐츠가 잘못되었습니다. 요구 사항이 실패했습니다. 메모리 64MB가 134217728 B의 허용 임계값 이하입니다.&quot;
+ __원인:__ `manifest.yml`의 작업자에 대한 `memorySize` 제한이 바이트 단위의 오류 메시지에서 보고한 대로 최소 허용 임계값 이하로 설정되었습니다.
+ __해결 방법:__ `manifest.yml`에서 `memorySize` 제한을 검토하고 모두 허용된 최소 임계값보다 큰지 확인하십시오.

### private.key가 누락되어 개발 도구를 시작할 수 없습니다.{#missing-private-key}

+ __오류:__ 로컬 개발 서버 오류: validatePrivateKeyFile에 필수 파일이 없습니다.... (`aio app run` 명령에서 표준 출력 사용)
+ __원인:__ `.env` 파일의 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 값이 `private.key`을(를) 가리키지 않거나 현재 사용자가 `private.key`을(를) 읽을 수 없습니다.
+ __해결 방법:__ `.env` 파일의 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 값을 검토하고 파일 시스템의 `private.key`에 대한 전체 절대 경로를 포함하는지 확인하십시오.

### Source 파일 드롭다운이 잘못됨{#source-files-dropdown-incorrect}

Asset Compute 개발 도구는 오래된 데이터를 가져오는 상태로 들어갈 수 있으며 잘못된 항목을 표시하는 __Source 파일__ 드롭다운에서 가장 잘 보입니다.

+ __오류:__ Source 파일 드롭다운에 잘못된 항목이 표시됩니다.
+ __원인:__ 오래된 캐시된 브라우저 상태로 인해
+ __해결 방법:__ 브라우저에서 브라우저 탭의 &quot;응용 프로그램 상태&quot;, 브라우저 캐시, 로컬 저장소 및 서비스 작업자를 완전히 지웁니다.

### 누락되었거나 잘못된 devToolToken 쿼리 매개 변수{#missing-or-invalid-devtooltoken-query-parameter}

+ __오류:__ Asset Compute 개발 도구의 &quot;승인되지 않은&quot; 알림
+ __원인:__ `devToolToken`이(가) 없거나 잘못되었습니다.
+ __해결 방법:__ Asset Compute 개발 도구 브라우저 창을 닫고 `aio app run` 명령을 통해 시작된 실행 중인 개발 도구 프로세스를 종료한 다음 `aio app run`을(를) 사용하여 개발 도구를 다시 시작합니다.

### 소스 파일을 제거할 수 없음{#unable-to-remove-source-files}

+ __오류:__ 개발 도구 UI에서 추가된 소스 파일을 제거할 방법이 없습니다.
+ __원인:__ 이 기능은 구현되지 않았습니다
+ __해결 방법:__ `.env`에 정의된 자격 증명을 사용하여 클라우드 저장소 공급자에 로그인합니다. 개발 도구(`.env`에도 지정됨)에서 사용하는 컨테이너를 찾아 __source__ 폴더로 이동한 다음 원본 이미지를 삭제합니다. 삭제된 소스 파일이 개발 도구 &quot;응용 프로그램 상태&quot;에서 로컬로 캐시될 수 있으므로 드롭다운에 계속 표시되는 경우 [Source 파일 드롭다운에 설명된 단계를 잘못](#source-files-dropdown-incorrect)해야 할 수 있습니다.

  ![Microsoft Azure Blob 저장소](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 테스트{#test}

### 테스트 실행 중에 생성된 렌디션 없음{#test-no-rendition-generated}

+ __오류:__ 실패: 생성된 렌디션이 없습니다.
+ __원인:__ JavaScript 구문 오류와 같은 예기치 않은 오류로 인해 작업자가 렌디션을 생성하지 못했습니다.
+ __해결 방법:__ `/build/test-results/test-worker/test.log`에서 테스트 실행의 `test.log`을(를) 검토하십시오. 이 파일에서 실패한 테스트 사례에 해당하는 섹션을 찾아 오류를 검토합니다.

  ![문제 해결 - 생성된 렌디션이 없음](./assets/troubleshooting/test__no-rendition-generated.png)

### 테스트가 잘못된 렌디션을 생성하여 테스트 실패{#tests-generates-incorrect-rendition}

+ __오류:__ 오류: 렌디션 &#39;rendition.xxx&#39;가 예상과 다릅니다.
+ __원인:__ 작업자가 테스트 사례에 제공된 `rendition.<extension>`과(와) 다른 렌디션을 출력했습니다.
   + 예상 `rendition.<extension>` 파일이 테스트 사례에서 로컬로 생성된 렌디션과 정확히 동일한 방식으로 만들어지지 않은 경우 비트마다 차이가 있을 수 있으므로 테스트가 실패할 수 있습니다. 예를 들어 Asset Compute 작업자가 API를 사용하여 대비를 변경하고 Adobe Photoshop CC의 대비를 조정하여 예상된 결과를 만드는 경우 파일이 동일하게 표시될 수 있지만 비트의 작은 변형이 다를 수 있습니다.
+ __해결 방법:__ `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`(으)로 이동하여 테스트의 렌디션 출력을 검토하고 테스트 사례의 예상 렌디션 파일과 비교합니다. 정확한 예상 에셋을 만들려면 다음 중 하나를 수행하십시오.
   + 개발 도구를 사용하여 렌디션을 생성하고 올바른 렌디션인지 확인한 다음 이를 예상 렌디션 파일로 사용합니다
   + 또는 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`에서 테스트 생성 파일의 유효성을 검사하고 올바른 파일의 유효성을 검사한 다음 필요한 렌디션 파일로 사용하십시오

## 디버그

### 디버거가 연결되지 않음{#debugger-does-not-attach}

+ __오류__: 시작을 처리하는 동안 오류가 발생했습니다. 오류: 디버그 대상에 연결할 수 없습니다.
+ __원인__: Docker Desktop이 로컬 시스템에서 실행되고 있지 않습니다. VS 코드 디버그 콘솔(보기 > 디버그 콘솔)을 검토하여 이 오류를 확인합니다.
+ __해결 방법__: [Docker 데스크톱을 시작하고 필요한 Docker 이미지가 설치되었는지 확인](./set-up/development-environment.md#docker).

### 중단점이 일시 중단되지 않음{#breakpoints-no-pausing}

+ __오류__: 디버그 가능한 개발 도구에서 Asset Compute 작업자를 실행할 때 VS 코드가 중단점에서 일시 중지되지 않습니다.

#### VS 코드 디버거가 연결되지 않음{#vs-code-debugger-not-attached}

+ __원인:__ VS 코드 디버거가 중지/연결 해제되었습니다.
+ __해결 방법:__ VS 코드 디버거를 다시 시작하고 VS 코드 디버그 출력 콘솔(보기 > 디버그 콘솔)을 확인하여 연결되어 있는지 확인합니다.

#### 작업자 실행이 시작된 후 연결된 VS 코드 디버거{#vs-code-debugger-attached-after-worker-execution-began}

+ __원인:__ 개발 도구에서 __실행__&#x200B;을 탭하기 전에 VS 코드 디버거가 연결되지 않았습니다.
+ __해결 방법:__ VS 코드의 디버그 콘솔(보기 > 디버그 콘솔)을 검토하여 디버거가 연결되었는지 확인한 다음 개발 도구에서 Asset Compute 작업자를 다시 실행하십시오.

### 디버깅하는 동안 작업자 시간 초과{#worker-times-out-while-debugging}

+ __오류__: Debug Console 보고서 &quot;작업이 -XXX밀리초 내에 시간 초과됩니다&quot; 또는 [Asset Compute 개발 도구의](./develop/development-tool.md) 렌디션 미리 보기가 무기한 회전되거나
+ __원인__: 디버깅하는 동안 [manifest.yml](./develop/manifest.md)에 정의된 작업자 시간 제한이 초과되었습니다.
+ __해결 방법__: [manifest.yml](./develop/manifest.md)에서 작업자 시간 제한을 일시적으로 늘리거나 디버깅 활동을 가속화합니다.

### 디버거 프로세스를 종료할 수 없습니다.{#cannot-terminate-debugger-process}

+ 명령줄에서 __오류__: `Ctrl-C`이(가) 디버거 프로세스(`npx adobe-asset-compute devtool`)를 종료하지 않습니다.
+ __원인__: `@adobe/aio-cli-plugin-asset-compute` 1.3.x의 버그로 인해 `Ctrl-C`이(가) 종료 명령으로 인식되지 않습니다.
+ __해결 방법__: `@adobe/aio-cli-plugin-asset-compute`을(를) 버전 1.4.1+로 업데이트합니다.

  ```
  $ aio update
  ```

  ![문제 해결 - aio 업데이트](./assets/troubleshooting/debug__terminate.png)

## 배포{#deploy}

### AEM의 에셋에서 사용자 정의 렌디션이 누락됨{#custom-rendition-missing-from-asset}

+ __오류:__ 새 자산 및 다시 처리된 자산이 처리되었지만 사용자 지정 렌디션이 없습니다

#### 처리 프로필이 상위 폴더에 적용되지 않음

+ __원인:__ 사용자 지정 작업자를 사용하는 처리 프로필이 있는 폴더에 자산이 없습니다.
+ __해결 방법:__ 자산의 상위 폴더에 처리 프로필 적용

#### 처리 프로필이 하위 처리 프로필로 대체됨

+ __원인:__ 사용자 지정 작업자 처리 프로필이 적용된 폴더 아래에 자산이 있지만, 해당 폴더와 자산 사이에 고객 작업자를 사용하지 않는 다른 처리 프로필이 적용되었습니다.
+ __해결 방법:__ 두 처리 프로필을 결합 또는 조정하고 중간 처리 프로필을 제거합니다.

### AEM에서 자산 처리 실패{#asset-processing-fails}

+ __오류:__ 에셋에 표시된 에셋 처리 실패 배지
+ __원인:__ 사용자 지정 작업자 실행에서 오류가 발생했습니다
+ __해결 방법:__ `aio app logs`을(를) 사용하여 [Adobe I/O Runtime 활성화 디버깅](./test-debug/debug.md#aio-app-logs)에 대한 지침을 따르십시오.
