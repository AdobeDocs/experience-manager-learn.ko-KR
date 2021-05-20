---
title: AEM Assets의 Asset compute 확장성 문제 해결
description: 다음은 AEM Assets용 사용자 지정 Asset compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제 및 오류 색인입니다. 해결 방법 과 함께
feature: asset compute 마이크로서비스
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 0%

---


# asset compute 확장성 문제 해결

다음은 AEM Assets용 사용자 지정 Asset compute 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제 및 오류 색인입니다. 해결 방법 과 함께

## 개발{#develop}

### 렌디션이 부분적으로 그리기/손상되어 반환됩니다{#rendition-returned-partially-drawn-or-corrupt}

+ __오류__:렌디션은 이미지를 잘못 렌더링하거나 손상되어 열 수 없습니다.

   ![렌디션이 부분적으로 그려집니다.](./assets/troubleshooting/develop__await.png)

+ __원인__:변환 `renditionCallback` 에 완전히 작성되기 전에 작업자 기능을  `rendition.path`종료합니다.
+ __해결 방법__:사용자 지정 작업자 코드를 검토하고 모든 비동기 호출이 를 사용하여 동기식으로 수행되는지 확인합니다  `await`.

## 개발 도구{#development-tool}

### asset compute 프로젝트{#missing-console-json}에 Console.json 파일이 없습니다.

+ __오류:__ 오류:유효성 검사(...)에 필요한 파일이 없습니다./node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY(비동기 setupAssetCompute(...)/node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __원인:__ Asset compute  `console.json` 프로젝트의 루트에 파일이 없습니다
+ __해결 방법:__ Adobe I/O  `console.json` 프로젝트에서 새 양식을 다운로드합니다
   1. console.adobe.io에서 Asset compute 프로젝트가 사용하도록 구성된 Adobe I/O 프로젝트를 엽니다
   1. 오른쪽 상단에 있는 __다운로드__ 단추를 누릅니다
   1. 파일 이름 `console.json`을 사용하여 다운로드한 파일을 Asset compute 프로젝트의 루트에 저장합니다

### manifest.html{#incorrect-yaml-indentation}에 잘못된 YAML 들여쓰기를 추가했습니다.

+ __오류:__ YAMLException:줄 X, 열 Y:(명령에서 표준 출력)의 매핑 항목 들여쓰기 `aio app run` 가 잘못되었습니다.
+ __원인:__  Yaml 파일은 흰색으로 구분되어 있으므로 들여쓰기가 올바르지 않을 수 있습니다.
+ __해결 방법:__ 를 검토하고  `manifest.yml` 모든 들여쓰기를 올바르게 확인합니다.

### memorySize 제한이 {#memorysize-limit-is-set-too-low}에 너무 낮게 설정되어 있습니다.

+ __오류:__  로컬 개발 서버 OpenWhiskError:PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true에서 HTTP 400을 반환했습니다(잘못된 요청) —> &quot;요청 콘텐츠의 형식이 잘못되었습니다. 요구 사항 실패:메모리 64MB가 허용되는 134217728 B&quot; 임계값보다 낮음
+ __원인:__ 의  `memorySize` 작업자에 대한 제한 `manifest.yml` 이 오류 메시지에서 보고한 최소 허용 임계값(바이트)보다 낮게 설정되었습니다.
+ __해결 방법:__  의 제한 `memorySize` 을 검토하고  `manifest.yml` 모두 허용되는 최소 임계값보다 커야 합니다.

### private.key{#missing-private-key} 누락으로 인해 개발 도구를 시작할 수 없습니다.

+ __오류:__ 로컬 개발 서버 오류:validatePrivateKeyFile에 필요한 파일이 없습니다.... ( `aio app run` 명령에서 standard를 통해)
+ __원인:__ 파일 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 의  `.env` 값이  `private.key` 을 가리키거나 현재 사용자 `private.key` 가 읽을 수 없습니다.
+ __해결 방법:__ 파일 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 의 값 `.env` 을 검토하고 파일 시스템의 전체 경로를  `private.key` 포함해야 합니다.

### 소스 파일 드롭다운이 잘못되었습니다.{#source-files-dropdown-incorrect}

asset compute 개발 도구는 오래된 데이터를 가져오는 상태를 입력할 수 있으며, 잘못된 항목을 표시하는 __소스 파일__ 드롭다운에서 가장 잘 보입니다.

+ __오류:__ 소스 파일 드롭다운에 잘못된 항목이 표시됩니다.
+ __원인:__ 오래된 캐시된 브라우저 상태로 인해
+ __해결 방법:__  브라우저에서 브라우저 탭의 &quot;애플리케이션 상태&quot;, 브라우저 캐시, 로컬 저장소 및 서비스 작업자를 완전히 지웁니다.

### devToolToken 쿼리 매개 변수가 없거나 잘못되었습니다.{#missing-or-invalid-devtooltoken-query-parameter}

+ __오류:__  Asset compute 개발 도구의 &quot;권한 없음&quot; 알림
+ __원인:__ `devToolToken` 이(가) 없거나 잘못되었습니다.
+ __해결 방법:__ Asset compute 개발 도구 브라우저 창을 닫고,  `aio app run` 명령을 통해 시작된 실행 중인 개발 도구 프로세스를 종료한 다음( `aio app run` 사용) 개발 도구를 다시 시작합니다.

### 소스 파일을 제거할 수 없습니다.{#unable-to-remove-source-files}

+ __오류:__ 개발 도구 UI에서 추가된 소스 파일을 제거할 방법이 없습니다
+ __원인:__ 이 기능이 구현되지 않았습니다
+ __해결 방법:__ 에 정의된 자격 증명을 사용하여 클라우드 스토리지 공급자에  `.env`로그인합니다. 개발 도구(`.env`에도 지정됨)에서 사용하는 컨테이너를 찾고 __source__ 폴더로 이동한 다음 소스 이미지를 삭제합니다. 삭제된 소스 파일이 개발 도구 &quot;응용 프로그램 상태&quot;에서 로컬로 캐시될 수 있으므로 드롭다운에 계속 표시되는 경우 [소스 파일 드롭다운에 설명된 단계를 수행해야 할 수 있습니다](#source-files-dropdown-incorrect).

   ![Microsoft Azure Blob 저장소](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 테스트{#test}

### 테스트 실행 중에 생성된 변환 없음{#test-no-rendition-generated}

+ __오류:__ 실패:생성된 표현물이 없습니다.
+ __원인:__  JavaScript 구문 오류와 같은 예기치 않은 오류로 인해 작업자가 변환을 생성하지 못했습니다.
+ __해결 방법:__ 테스트 실행의 위치 `test.log` 를  `/build/test-results/test-worker/test.log`검토합니다. 실패한 테스트 사례에 해당하는 이 파일에서 섹션을 찾아 오류를 검토하십시오.

   ![문제 해결 - 생성된 변환 없음](./assets/troubleshooting/test__no-rendition-generated.png)

### 테스트가 실패하도록 하는 잘못된 표현물을 생성합니다.{#tests-generates-incorrect-rendition}

+ __오류:__ 실패:변환 &#39;rendition.xxx&#39;가 예상대로 표시되지 않습니다.
+ __원인:__ 작업자가 테스트 사례에  `rendition.<extension>` 제공된 것과 같지 않은 변환을 출력합니다.
   + 예상 `rendition.<extension>` 파일이 테스트 케이스에서 로컬로 생성된 표현물과 정확히 동일한 방식으로 생성되지 않으면 비트에 차이가 있을 수 있으므로 테스트가 실패할 수 있습니다. 예를 들어, Asset compute 작업자가 API를 사용하여 대비를 변경하고, 예상되는 결과가 Adobe Photoshop CC의 대비를 조정하여 만들어지는 경우 파일은 동일하게 표시될 수 있지만 비트의 작은 변형은 다를 수 있습니다.
+ __해결 방법:__ 로 이동하여 테스트의 변환 출력을 검토하고  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`테스트 케이스의 예상 변환 파일과 비교합니다. 예상 자산을 정확히 만들려면 다음을 수행하십시오.
   + 변환 생성, 올바른 유효성 검사 및 변환 파일로 사용하려면 개발 도구를 사용하십시오
   + 또는 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`에서 테스트 생성 파일의 유효성을 확인하고 올바른 파일인지 확인한 다음 예상 변환 파일로 사용합니다

## 디버그

### 디버거가{#debugger-does-not-attach}을 연결하지 않음

+ __오류__:시작 처리 오류:오류:디버그 대상에 연결할 수 없습니다...
+ __원인__:로컬 시스템에서 Docker Desktop이 실행되고 있지 않습니다. VS 코드 디버그 콘솔(보기 > 디버그 콘솔)을 검토하여 이 오류가 보고되는지 확인하여 확인합니다.
+ __해결 방법__:Docker  [Desktop을 시작하고 필수 Docker 이미지가 설치되었는지](./set-up/development-environment.md#docker) 확인합니다.

### 중단점이 일시 중지되지 않음{#breakpoints-no-pausing}

+ __오류__:디버그 가능 개발 도구에서 Asset compute 작업자를 실행할 때 VS 코드가 중단점에서 일시 중지되지 않습니다.

#### VS 코드 디버거가 첨부되지 않음{#vs-code-debugger-not-attached}

+ __원인:__ VS 코드 디버거가 중지/연결이 끊어졌습니다.
+ __해결 방법:__ VS 코드 디버거를 다시 시작하고 VS 코드 디버그 출력 콘솔(보기 > 디버그 콘솔)을 시청하여 VS 코드 디버거가 연결되었는지 확인합니다.

#### 작업자 실행이 시작된 후 첨부된 VS 코드 디버거{#vs-code-debugger-attached-after-worker-execution-began}

+ __원인:__ Runin 개발 도구를 탭하기 전에 VS 코드  ____ 디버거가 첨부되지 않았습니다.
+ __해결 방법:__ VS 코드의 디버그 콘솔(보기 > 디버그 콘솔)을 검토한 다음 개발 도구에서 Asset compute 작업자를 다시 실행하여 디버거가 연결되었는지 확인합니다.

### 디버깅하는 동안 작업자 시간이 초과되었습니다.{#worker-times-out-while-debugging}

+ __오류__:디버그 콘솔 보고서 &quot;작업 시간 제한 -XXX 밀리초&quot; 또는  [Asset compute 개발 도구의 ](./develop/development-tool.md) 변환 미리 보기가 무기한 또는
+ __원인__:디버깅하는 동안 manifest에 정의된  [작업자 시간](./develop/manifest.md) 이 초과되었습니다.
+ __해결 방법__:매니페스트에서 작업자 시간 제한을 일시적으로  [늘립니다.](./develop/manifest.md) ymer는 디버깅 활동을 가속화합니다.

### 디버거 프로세스{#cannot-terminate-debugger-process}을(를) 종료할 수 없습니다.

+ __오류__: `Ctrl-C` 명령줄에서 은(는) 디버거 프로세스(`npx adobe-asset-compute devtool`)를 종료하지 않습니다.
+ __원인__:1. `@adobe/aio-cli-plugin-asset-compute` 3.x의 버그로 인해 종료 명령으로  `Ctrl-C` 인식되지 않습니다.
+ __해결 방법__:버전  `@adobe/aio-cli-plugin-asset-compute` 1.4.1+로 업데이트

   ```
   $ aio update
   ```

   ![문제 해결 - aio 업데이트](./assets/troubleshooting/debug__terminate.png)

## 배포{#deploy}

### AEM{#custom-rendition-missing-from-asset}의 자산에서 사용자 지정 표현물이 없습니다.

+ __오류:__ 새 자산 및 재처리된 자산을 성공적으로 처리했지만 사용자 지정 표현물이 없습니다

#### 처리 프로필이 상위 폴더에 적용되지 않았습니다.

+ __원인:__  사용자 지정 작업자를 사용하는 처리 프로필이 있는 폴더에 자산이 없습니다
+ __해결 방법:__ 자산의 상위 폴더에 처리 프로필 적용

#### 처리 프로필은 낮은 처리 프로필로 대체됨

+ __원인:__ 사용자 정의 작업자 처리 프로필이 적용된 폴더 아래에 자산이 있지만, 고객 작업자를 사용하지 않는 다른 처리 프로필이 해당 폴더와 자산 간에 적용되었습니다.
+ __해결 방법:__ 두 처리 프로필을 조합하거나 다른 방식으로 조정하고 중간 처리 프로필을 제거합니다

### AEM{#asset-processing-fails}에서 자산 처리가 실패합니다.

+ __오류:__ 자산에 표시된 자산 처리 실패 배지
+ __원인:__  사용자 정의 작업자를 실행하는 동안 오류가 발생했습니다
+ __해결 방법:__ 을 사용하여  [Adobe I/O Runtime 활동 디버깅에 대한 ](./test-debug/debug.md#aio-app-logs) 지침을 따릅니다  `aio app logs`.


