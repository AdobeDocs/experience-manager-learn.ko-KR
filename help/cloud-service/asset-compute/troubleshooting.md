---
title: AEM Assets의 자산 계산 확장성 문제 해결
description: 다음은 AEM Assets용 사용자 지정 자산 계산 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제 및 오류에 대한 색인입니다.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# 자산 계산 확장성 문제 해결

다음은 AEM Assets용 사용자 지정 자산 계산 작업자를 개발 및 배포할 때 발생할 수 있는 일반적인 문제 및 오류에 대한 색인입니다.

## 개발{#develop}

### 변환이 부분적으로 그림/손상되어 반환됩니다.{#rendition-returned-partially-drawn-or-corrupt}

+ __오류__:변환은 이미지를 완전히 렌더링하거나 손상되어 열 수 없습니다.

   ![변환이 부분적으로 그려집니다.](./assets/troubleshooting/develop__await.png)

+ __원인__:변환을 완전히 쓰기 전에 근로자의 `renditionCallback` 기능이 종료된다 `rendition.path`.
+ __해상도__:사용자 지정 작업자 코드를 검토하고 모든 비동기 호출이 를 사용하여 동기식으로 수행되었는지 확인합니다 `await`.

## 개발 도구{#development-tool}

### manifest.yml의 YAML 들여쓰기가 잘못되었습니다.{#incorrect-yaml-indentation}

+ __오류:__ YAMLException:라인 X, 열 Y:(표준 출력 명령을 통해)에서 매핑 항목의 잘못된 들여쓰기 `aio app run` 가
+ __원인:__ Yaml 파일은 흰색 간격에 민감하므로 들여쓰기가 올바르지 않을 수 있습니다.
+ __해결 방법:__ 문서를 검토하고 `manifest.yml` 모든 들여쓰기가 올바른지 확인하십시오.

### memorySize 제한이 너무 낮게 설정되어 있습니다.{#memorysize-limit-is-set-too-low}

+ __오류:__ 로컬 개발 서버 OpenWhiskError:PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true이(가) HTTP 400을 반환했습니다(잘못된 요청) —> &quot;요청 컨텐츠의 형식이 잘못되었습니다. 요구 사항 실패:메모리 64MB가 허용된 임계값 134217728B&quot;보다 낮음
+ __원인:__ 작업자 `memorySize` 에 대한 제한 `manifest.yml` 이 오류 메시지에 의해 보고되는 최소 허용 임계값(바이트)보다 낮게 설정되었습니다.
+ __해결 방법:__ 제한 `memorySize` 을 검토하고 `manifest.yml` 모두 허용된 최소 임계값보다 커야 합니다.

### private.key가 누락되어 개발 도구를 시작할 수 없습니다.{#missing-private-key}

+ __오류:__ 로컬 개발 서버오류:validatePrivateKeyFile에 필요한 파일이 없습니다.... (표준에서 나온 명령 `aio app run` 사용)
+ __원인:__ 파일 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 의 값이 `.env` 가리켜지지 `private.key` 않거나 현재 사용자가 읽을 수 `private.key` 없습니다.
+ __해결 방법:__ 파일의 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 값을 `.env` 검토하고 파일 시스템에 대한 전체 절대 경로를 포함하는지 `private.key` 확인합니다.

### 소스 파일 드롭다운이 잘못되었습니다.{#source-files-dropdown-incorrect}

자산 계산 개발 도구는 오래된 데이터를 가져오는 상태에 들어갈 수 있으며, 소스 파일 ____ 드롭다운에서 잘못된 항목을 표시하는 것이 가장 눈에 띄었습니다.

+ __오류:__ 소스 파일 드롭다운에 잘못된 항목이 표시됩니다.
+ __원인:__ 오래된 캐시된 브라우저 상태로 인해
+ __해결 방법:__ 브라우저에서 브라우저 탭의 &quot;응용 프로그램 상태&quot;, 브라우저 캐시, 로컬 저장소 및 서비스 작업자를 완전히 지웁니다.

### devToolToken 쿼리 매개 변수가 없거나 잘못되었습니다.{#missing-or-invalid-devtooltoken-query-parameter}

+ __오류:__ 자산 계산 개발 도구의 &quot;권한 없음&quot; 알림
+ __원인:__`devToolToken` 없거나 잘못되었습니다.
+ __해결 방법:__ 자산 계산 개발 도구 브라우저 창을 닫고, 명령을 통해 시작된 모든 실행 중인 개발 도구 프로세스를 `aio app run` 종료하고 개발 도구를 다시 시작합니다(사용 `aio app run`).

### 소스 파일을 제거할 수 없습니다.{#unable-to-remove-source-files}

+ __오류:__ 개발 도구 UI에서 추가된 소스 파일을 제거할 방법이 없습니다.
+ __원인:__ 이 기능은 구현되지 않았습니다.
+ __해결 방법:__ 에 정의된 자격 증명을 사용하여 클라우드 스토리지 공급자에 로그인합니다 `.env`. 개발 도구(에 지정됨)에서 사용하는 컨테이너 `.env`를 찾아 __소스__ 폴더로 이동한 다음 소스 이미지를 삭제합니다. 삭제된 소스 파일이 개발 도구 &quot;응용 프로그램 상태&quot;에서 로컬에 캐싱될 수 있으므로 드롭다운에 계속 표시되는 경우 [소스 파일 드롭다운에](#source-files-dropdown-incorrect) 설명된 단계를 수행해야 할 수 있습니다.

   ![Microsoft Azure Blob 저장소](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 테스트{#test}

### 테스트 실행 중 생성된 변환 없음{#test-no-rendition-generated}

+ __오류:__ 실패:생성된 변환이 없습니다.
+ __원인:__ JavaScript 구문 오류와 같은 예기치 않은 오류로 인해 Worker에서 변환을 생성하지 못했습니다.
+ __해결 방법:__ 테스트 실행을 `test.log` 검토합니다 `/build/test-results/test-worker/test.log`. 이 파일에서 실패한 테스트 케이스에 해당하는 섹션을 찾아 오류를 검토합니다.

   ![문제 해결 - 생성된 변환 없음](./assets/troubleshooting/test__no-rendition-generated.png)

### 테스트가 실패하게 하는 잘못된 변환을 테스트합니다.{#tests-generates-incorrect-rendition}

+ __오류:__ 실패:&#39;rendition.xxx&#39; 변환이 예상대로 표시되지 않습니다.
+ __원인:__ 작업자가 테스트 케이스에 제공된 것과 동일하지 않은 변환을 `rendition.<extension>` 출력합니다.
   + 예상 `rendition.<extension>` 파일이 테스트 케이스에서 로컬로 생성된 변환과 동일한 방식으로 만들어지지 않으면 비트 간의 차이가 있기 때문에 테스트가 실패할 수 있습니다. 예를 들어, 자산 계산 작업자가 API를 사용하여 대비를 변경하고 예상 결과가 Adobe Photoshop CC에서 대비를 조정하여 만드는 경우 파일이 동일하게 나타날 수 있지만 비트의 작은 변형은 다를 수 있습니다.
+ __해결 방법:__ 테스트로 이동하여 테스트의 변환 출력을 검토하고 테스트 케이스 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`에서 예상 변환 파일과 비교합니다. 예상 자산을 정확히 만들려면 다음 중 하나를 수행하십시오.
   + 개발 도구를 사용하여 변환을 생성하고, 맞는지 확인하고, 이를 예상 변환 파일로 사용합니다
   + 또는 에서 테스트 생성 파일의 유효성을 확인하고, `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`올바른지 확인하고, 필요한 변환 파일로 사용합니다.

## 디버그


### 디버거가 연결되지 않음{#debugger-does-not-attach}

+ __오류__:론치를 처리하는 동안 오류 발생:오류:...에서 디버그 타겟에 연결할 수 없습니다.
+ __원인__:Docker Desktop이 로컬 시스템에서 실행되고 있지 않습니다. VS 코드 디버그 콘솔([보기] > [디버그 콘솔])을 검토하여 이 오류가 보고되었는지 확인합니다.
+ __해상도__:Docker [Desktop을 시작하고 필수 Docker 이미지가 설치되었는지](./set-up/development-environment.md#docker)확인합니다.

### 중단점이 일시 중지되지 않음{#breakpoints-no-pausing}

+ __오류__:디버그 가능 개발 도구에서 자산 계산 작업자를 실행할 때 VS 코드가 중단점에서 일시 중지되지 않습니다.

#### VS 코드 디버거가 연결되지 않음{#vs-code-debugger-not-attached}

+ __원인:__ VS 코드 디버거가 중지/연결이 끊어졌습니다.
+ __해결 방법:__ VS 코드 디버거를 다시 시작하고 VS 코드 디버그 출력 콘솔(보기 > 디버그 콘솔)을 시청하여 VS 코드 디버그 디버거가 첨부되는지 확인합니다.

#### 작업자 실행이 시작된 후 연결된 VS 코드 디버거{#vs-code-debugger-attached-after-worker-execution-began}

+ __원인:__ VS 코드 디버거가 개발 도구에서 __실행을 탭하기__ 전에 연결되지 않았습니다.
+ __해결 방법:__ VS 코드의 디버그 콘솔(보기 > 디버그 콘솔)을 검토한 다음 개발 도구에서 자산 계산 작업자를 다시 실행하여 디버거가 연결되었는지 확인합니다.

### 디버깅 시 작업 시간 초과{#worker-times-out-while-debugging}

+ __오류__:디버그 콘솔에서는 &quot;작업 시간이 -XXX 밀리초&quot; 또는 [Asset Compute Development Tool의](./develop/development-tool.md) 변환 미리 보기가 무기한 또는
+ __원인__:디버깅 중에 manifest.yml에 정의된 작업자 시간 [이](./develop/manifest.md) 초과되었습니다.
+ __해상도__:manifest.yml에서 작업자의 시간 초과를 [일시적으로 늘리거나 디버깅](./develop/manifest.md) 활동을 가속화합니다.

### 디버거 프로세스를 종료할 수 없습니다.{#cannot-terminate-debugger-process}

+ __오류__: `Ctrl-C` 명령줄에서 디버거 프로세스를 종료하지 않습니다(`npx adobe-asset-compute devtool`).
+ __원인__:1.3.x의 버그로 인해 종료 명령으로 인식되지 `@adobe/aio-cli-plugin-asset-compute` `Ctrl-C` 않습니다.
+ __해상도__:버전 1.4.1+ `@adobe/aio-cli-plugin-asset-compute` 로 업데이트

   ```
   $ aio update
   ```

   ![문제 해결 - aio 업데이트](./assets/troubleshooting/debug__terminate.png)

## Deploy{#deploy}

### AEM의 자산에서 사용자 지정 변환이 없습니다.{#custom-rendition-missing-from-asset}

+ __오류:__ 새 자산 및 재처리된 에셋이 성공적으로 처리되었지만 사용자 지정 변환이 없습니다.

#### 상위 폴더에 처리 프로필이 적용되지 않음

+ __원인:__ 사용자 지정 작업자를 사용하는 처리 프로필의 폴더 아래에 자산이 없습니다.
+ __해결 방법:__ 자산의 상위 폴더에 처리 프로필 적용

#### 처리 프로필은 낮은 처리 프로필로 대체됨

+ __원인:__ 자산은 사용자 지정 작업자 처리 프로필이 적용된 폴더 아래에 있지만, 고객 작업자를 사용하지 않는 다른 처리 프로필은 해당 폴더와 자산 사이에 적용되었습니다.
+ __해결 방법:__ 두 처리 프로필 결합 또는 다른 방법으로 대사하고 중간 처리 프로필 제거

### AEM에서 자산 처리 실패{#asset-processing-fails}

+ __오류:__ 자산에 표시된 자산 처리 실패 배지
+ __원인:__ 사용자 지정 작업자를 실행하는 동안 오류가 발생했습니다.
+ __해결 방법:__ 를 사용하여 Adobe I/O Runtime 활동 [디버깅에 대한 지침을](./test-debug/debug.md#aio-app-logs) 따릅니다 `aio app logs`.


