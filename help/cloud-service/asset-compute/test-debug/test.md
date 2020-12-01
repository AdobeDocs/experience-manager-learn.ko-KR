---
title: asset compute 작업자 테스트
description: asset compute 프로젝트는 Asset compute 작업자의 테스트를 쉽게 만들고 실행하기 위한 패턴을 정의합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# asset compute 작업자 테스트

asset compute 프로젝트는 Asset compute 근로자의 [테스트를 쉽게 만들고 실행하기 위한 패턴을 정의합니다.](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## 노동시험 해부학

asset compute 작업자의 테스트는 테스트 세트로 분류되며 각 테스트 세트 내에서 테스트할 조건을 나타내는 하나 이상의 테스트 케이스를 참조합니다.

asset compute 프로젝트의 테스트 구조는 다음과 같습니다.

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

각 테스트 캐스트에는 다음 파일이 있을 수 있습니다.

+ `file.<extension>`
   + 테스트할 원본 파일(확장자는 `.link`을 제외한 모든 것 가능)
   + 필수
+ `rendition.<extension>`
   + 예상 변환
   + 필수, 오류 테스트 제외
+ `params.json`
   + 단일 변환 JSON 지침
   + 선택 사항입니다
+ `validate`
   + 예상 스크립트와 실제 변환 파일 경로를 인수로 받고 결과가 괜찮으면 종료 코드 0을 반환해야 하며, 유효성 검사나 비교가 실패할 경우 0이 아닌 종료 코드를 반환해야 합니다.
   + 선택 사항, 기본값은 `diff` 명령입니다.
   + 다른 유효성 검사 도구를 사용하기 위해 문서 실행 명령을 래핑하는 셸 스크립트 사용
+ `mock-<host-name>.json`
   + 외부 서비스 호출[을 조롱하는 JSON 형식의 HTTP 응답입니다.](https://www.mock-server.com/mock_server/creating_expectations.html)
   + 선택 사항, 작업자 코드가 HTTP 요청을 자체적으로 수행하는 경우에만 사용됩니다.

## 테스트 케이스 작성

이 테스트 케이스는 입력 파일(`file.jpg`)에 대해 매개 변수가 있는 입력(`params.json`)이 필요한 PNG 변환(`rendition.png`)을 생성합니다.

1. Adobe Worker가 더 이상 원본을 변환으로 복사하지 않으므로 먼저 `/test/asset-compute/simple-worker`에서 자동 생성된 `simple-worker` 테스트 케이스를 삭제합니다.
1. PNG 변환을 생성하는 작업자의 성공적인 실행을 테스트하려면 `/test/asset-compute/worker/success-parameterized`에서 새 테스트 케이스 폴더를 만듭니다.
1. `success-parameterized` 폴더에서 이 테스트 케이스에 대한 테스트 [입력 파일](./assets/test/success-parameterized/file.jpg)을 추가하고 이름을 `file.jpg`으로 지정합니다.
1. `success-parameterized` 폴더에서 작업자의 입력 매개 변수를 정의하는 `params.json`이라는 새 파일을 추가합니다.

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   이는 `worker` 키를 제외한 [개발 도구의 Asset compute 프로필 정의](../develop/development-tool.md)에 전달된 것과 동일한 키/값입니다.
1. 이 테스트 케이스에 필요한 [변환 파일](./assets/test/success-parameterized/rendition.png)을 추가하고 이름을 `rendition.png`로 지정합니다. 이 파일은 지정된 입력 `file.jpg`에 대해 워커의 예상 출력을 나타냅니다.
1. 명령줄에서 `aio app test`을 실행하여 프로젝트 루트를 실행합니다.
   + [Docker Desktop](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작되었는지 확인합니다.
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 성공  ](./assets/test/success-parameterized/result.png)

## 오류 검사 테스트 케이스 쓰기

이 테스트 케이스 테스트는 `contrast` 매개 변수가 잘못된 값으로 설정되어 있을 때 작업자가 적절한 오류를 throw하는지 확인합니다.

1. 잘못된 `contrast` 매개 변수 값으로 인해 작업자의 잘못된 실행을 테스트하려면 `/test/asset-compute/worker/error-contrast`에 새 테스트 케이스 폴더를 만듭니다.
1. `error-contrast` 폴더에서 이 테스트 케이스에 대한 테스트 [입력 파일](./assets/test/error-contrast/file.jpg)을 추가하고 이름을 `file.jpg`으로 지정합니다. 이 파일의 내용이 이 테스트에 적합하지 않으므로, 이 테스트 케이스의 유효성 검사에 도달하기 위해 &quot;손상된 소스&quot; 검사를 통과해야 합니다.`rendition.instructions`
1. `error-contrast` 폴더에서 내용을 포함하는 워커의 입력 매개 변수를 정의하는 `params.json`이라는 새 파일을 추가합니다.

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + `contrast` 매개 변수를 `10`로 설정합니다. 대수는 -1과 1 사이여야 합니다. `RenditionInstructionsError`을(를) throw합니다.
   + 필요한 오류와 관련된 `errorReason` 키를 &quot;reason&quot;으로 설정하여 적절한 오류를 테스트에서 throw합니다. 이 잘못된 대비 매개 변수는 [사용자 지정 오류](../develop/worker.md#errors), `RenditionInstructionsError`를 throw하므로 `errorReason`을 이 오류의 이유로 설정하거나, 또는 `rendition_instructions_error`를 설정하여 throw됩니다.

1. 종료 실행 중에는 변환이 생성되지 않으므로 `rendition.<extension>` 파일이 필요하지 않습니다.
1. `aio app test` 명령을 실행하여 프로젝트의 루트에서 테스트 세트 실행
   + [Docker Desktop](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작되었는지 확인합니다.
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 오류 대비](./assets/test/error-contrast/result.png)

## Github에서 테스트 케이스

최종 테스트 사례는 다음 위치의 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 문제 해결

+ [테스트 실행 중 생성된 변환 없음](../troubleshooting.md#test-no-rendition-generated)
+ [테스트가 잘못된 변환을 생성합니다.](../troubleshooting.md#tests-generates-incorrect-rendition)
