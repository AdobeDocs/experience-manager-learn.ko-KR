---
title: 자산 계산 작업자 테스트
description: 자산 계산 프로젝트는 자산 계산 작업자의 테스트를 쉽게 만들고 실행하기 위한 패턴을 정의합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 06632b90e5cdaf80b9343e5a69ab9c735d4a70f1
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---


# 자산 계산 작업자 테스트

자산 계산 프로젝트는 자산 계산 작업자의 [테스트를 쉽게 만들고 실행하기 위한 패턴을 정의합니다](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## 노동시험 해부학

자산 계산 작업자의 테스트는 테스트 세트로 분류되며, 각 테스트 세트 내에서 테스트할 조건을 나타내는 하나 이상의 테스트 케이스를 나타냅니다.

자산 계산 프로젝트의 테스트 구조는 다음과 같습니다.

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

각 테스트 캐스트에는 다음 파일이 있을 수 있습니다.

+ `file.<extension>`
   + 테스트할 소스 파일(확장자는 제외 `.link`)
   + 필수
+ `rendition.<extension>`
   + 예상 변환
   + 필수, 오류 테스트 제외
+ `params.json`
   + 단일 변환 JSON 지침
   + 선택 사항입니다
+ `validate`
   + 예상 스크립트와 실제 변환 파일 경로를 인수로 받고 결과가 괜찮으면 종료 코드 0을 반환해야 하며, 유효성 검사나 비교가 실패할 경우 0이 아닌 종료 코드를 반환해야 합니다.
   + 선택 사항, 기본적으로 `diff` 명령
   + 다른 유효성 검사 도구를 사용하기 위해 문서 실행 명령을 래핑하는 셸 스크립트 사용
+ `mock-<host-name>.json`
   + 외부 서비스 호출을 [조롱하기 위한 JSON 형식의 HTTP 응답](https://www.mock-server.com/mock_server/creating_expectations.html).
   + 선택 사항, 작업자 코드가 HTTP 요청을 자체적으로 수행하는 경우에만 사용됩니다.

## 테스트 케이스 작성

이 테스트 케이스는 입력 파일에 대해 매개 변수가 있는 입력(`params.json`)을`file.jpg`어설프게 실행함으로써 예상되는 PNG 변환(`rendition.png`)을 생성합니다.

1. Adobe Worker가 더 이상 원본을 변환에 복사하지 않으므로, 이 `simple-worker` `/test/asset-compute/simple-worker` 가 유효하지 않으므로 먼저 자동 생성된 테스트 케이스를 삭제합니다.
1. PNG 변환을 생성하는 작업자 `/test/asset-compute/worker/success-parameterized` 의 성공적인 실행을 테스트하려면 에 새 테스트 케이스 폴더를 만듭니다.
1. 폴더에서 `success-parameterized` 이 테스트 케이스에 대한 테스트 [입력 파일](./assets/test/success-parameterized/file.jpg) 을 추가하고 이름을 `file.jpg`지정합니다.
1. 폴더에서 `success-parameterized` 작업자의 입력 매개 변수를 정의하는 새 파일 `params.json` 을 추가합니다.

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   키 대신 [개발 툴의 에셋 계산 프로필 정의에 전달된 키/값과](../develop/development-tool.md)같습니다 `worker` .
1. 이 테스트 케이스에 예상 [변환 파일을](./assets/test/success-parameterized/rendition.png) 추가하고 이름을 지정합니다 `rendition.png`. 이 파일은 지정된 입력에 대해 워커의 예상 출력을 나타냅니다 `file.jpg`.
1. 명령줄에서 `aio app test`
   + Docker [Desktop](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작되었는지 확인
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 성공 ](./assets/test/success-parameterized/result.png)

## 오류 검사 테스트 케이스 쓰기

이 테스트 케이스 테스트는 매개 변수가 잘못된 값으로 설정된 경우 작업자가 적절한 오류를 `contrast` 던지도록 합니다.

1. 잘못된 매개 변수 값으로 인해 작업자 `/test/asset-compute/worker/error-contrast` 의 종료 실행을 테스트하기 위해 새 테스트 케이스 폴더를 `contrast` 만듭니다.
1. 폴더에서 `error-contrast` 이 테스트 케이스에 대한 테스트 [입력 파일](./assets/test/error-contrast/file.jpg) 을 추가하고 이름을 `file.jpg`지정합니다. 이 파일의 내용은 이 테스트에 적합하지 않으며, 유효성 검사에 도달하기 위해 이 테스트 케이스의 유효성 검사를 통과하기 위해 &quot;손상된 소스&quot; 검사를 통과해야 합니다. `rendition.instructions`
1. 폴더에서 `error-contrast` 내용을 포함하는 워커의 입력 매개 변수를 정의하는 새 파일 `params.json` 을 추가합니다.

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 매개 변수 `contrast` 를 `10`throw하려면 대비가 -1과 1 사이여야 하므로 잘못된 값을 로 설정합니다 `RenditionInstructionsError`.
   + 예상되는 오류와 관련된 &quot;이유&quot;로 `errorReason` 키를 설정하여 적절한 오류를 테스트에 던집니다. 이 잘못된 대비 매개 변수는 [사용자 지정 오류](../develop/worker.md#errors)를 발생시키므로 `RenditionInstructionsError`이 오류 `errorReason` 의 이유로 설정하거나`rendition_instructions_error` throw됩니다.

1. 종료 실행 중에는 변환이 생성되지 않으므로 `rendition.<extension>` 파일이 필요하지 않습니다.
1. 명령을 실행하여 프로젝트의 루트에서 테스트 세트 실행 `aio app test`
   + Docker [Desktop](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작되었는지 확인
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 오류 대비](./assets/test/error-contrast/result.png)

## 문제 해결

### 생성된 변환 없음

변환을 생성하지 않으면 테스트 케이스가 실패합니다.

+ __오류:__ 실패:생성된 변환이 없습니다.
+ __원인:__ JavaScript 구문 오류와 같은 예기치 않은 오류로 인해 Worker에서 변환을 생성하지 못했습니다.
+ __해결 방법:__ 테스트 실행을 `test.log` 검토합니다 `/build/test-results/test-worker/test.log`. 이 파일에서 실패한 테스트 케이스에 해당하는 섹션을 찾아 오류를 검토합니다.

   ![문제 해결 - 생성된 변환 없음](./assets/test/troubleshooting__no-rendition-generated.png)

### 테스트가 잘못된 변환을 생성합니다.

테스트 케이스가 잘못된 변환을 생성하지 못합니다.

+ __오류:__ 실패:&#39;rendition.xxx&#39; 변환이 예상대로 표시되지 않습니다.
+ __원인:__ 작업자가 테스트 케이스에 제공된 것과 동일하지 않은 변환을 `rendition.<extension>` 출력합니다.
   + 예상 `rendition.<extension>` 파일이 테스트 케이스에서 로컬로 생성된 변환과 동일한 방식으로 만들어지지 않으면 비트 간의 차이가 있기 때문에 테스트가 실패할 수 있습니다. 테스트 케이스의 예상 변환이 Development Tool에서 저장된 경우, 즉 Adobe I/O Runtime 내에서 생성된 비트가 기술적으로 다를 수 있으므로, 인간의 관점에서 예상되는 변환 파일과 실제 변환 파일이 동일하더라도 테스트가 실패할 수 있습니다.
+ __해결 방법:__ 테스트로 이동하여 테스트의 변환 출력을 검토하고 테스트 케이스 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`에서 예상 변환 파일과 비교합니다.
