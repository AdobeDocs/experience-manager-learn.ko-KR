---
title: asset compute 작업자 테스트
description: asset compute 프로젝트는 Asset compute 작업자의 테스트를 쉽게 만들고 실행하기 위한 패턴을 정의합니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 205
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# asset compute 작업자 테스트

asset compute 프로젝트는 쉽게 만들고 실행하기 위한 패턴을 정의합니다 [asset compute 작업자 테스트](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## 작업자 테스트의 해부학

Asset compute 작업자의 테스트는 테스트 세트로 구분되며, 각 테스트 세트 내에서 하나 이상의 테스트 케이스가 테스트할 조건을 어설션합니다.

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
   + 테스트할 소스 파일(확장자는 다음 중 하나일 수 있음: `.link`)
   + 필수
+ `rendition.<extension>`
   + 예상 렌디션
   + 오류 테스트 제외 필수
+ `params.json`
   + 단일 렌디션 JSON 지침
   + 옵션
+ `validate`
   + 예상 및 실제 렌디션 파일 경로를 인수로 가져오고 결과가 양호한 경우 종료 코드 0을 반환해야 하며, 유효성 검사나 비교가 실패한 경우 0이 아닌 종료 코드를 반환해야 하는 스크립트.
   + 선택 사항이며 기본값은 `diff` 명령
   + 다른 유효성 검사 도구를 사용하기 위해 도커 실행 명령을 래핑하는 셸 스크립트를 사용합니다
+ `mock-<host-name>.json`
   + 다음에 대한 JSON 형식의 HTTP 응답 [외부 서비스 통화 조롱](https://www.mock-server.com/mock_server/creating_expectations.html).
   + 선택 사항이며, 작업자 코드가 자체의 HTTP 요청을 하는 경우에만 사용됩니다

## 테스트 사례 작성

이 테스트 사례는 매개 변수가 있는 입력(`params.json`) 입력 파일(`file.jpg`) 예상 PNG 렌디션을 생성합니다(`rendition.png`).

1. 먼저 자동 생성된 를 삭제합니다. `simple-worker` 테스트 사례 위치: `/test/asset-compute/simple-worker` 이 작업은 무효이므로 작업자는 더 이상 소스를 렌디션에 복사하지 않습니다.
1. 다음 위치에 새 테스트 사례 폴더를 만듭니다. `/test/asset-compute/worker/success-parameterized` PNG 렌디션을 생성하는 작업자의 성공적인 실행을 테스트합니다.
1. 다음에서 `success-parameterized` 폴더, 테스트 추가 [입력 파일](./assets/test/success-parameterized/file.jpg) 이 테스트 사례에 대해 이름을 지정합니다. `file.jpg`.
1. 다음에서 `success-parameterized` 폴더, 이름이 인 새 파일 추가 `params.json` 작업자의 입력 매개변수를 정의합니다.

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   이 값은에 전달된 것과 동일한 키/값입니다 [개발 도구의 Asset compute 프로필 정의](../develop/development-tool.md), 을(를) 줄입니다. `worker` 키.

1. 예상 항목 추가 [렌디션 파일](./assets/test/success-parameterized/rendition.png) 이 테스트 사례에 추가하고 이름을 지정합니다. `rendition.png`. 이 파일은 주어진 입력에 대한 작업자의 예상 출력을 나타냅니다 `file.jpg`.
1. 명령줄에서 을 실행하여 프로젝트 루트 테스트를 실행합니다. `aio app test`
   + 확인 [Docker 데스크탑](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작됨
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 성공 ](./assets/test/success-parameterized/result.png)

## 오류 검사 테스트 사례 작성

이 테스트 사례는 다음 경우에 작업자가 적절한 오류를 발생시키는지 확인하기 위해 테스트합니다. `contrast` 매개 변수가 잘못된 값으로 설정되어 있습니다.

1. 다음 위치에 새 테스트 사례 폴더를 만듭니다. `/test/asset-compute/worker/error-contrast` 잘못된 작업으로 인한 작업자 오류 실행을 테스트하려면 `contrast` 매개 변수 값입니다.
1. 다음에서 `error-contrast` 폴더, 테스트 추가 [입력 파일](./assets/test/error-contrast/file.jpg) 이 테스트 사례에 대해 이름을 지정합니다. `file.jpg`. 이 파일의 내용은 이 테스트에 중요하지 않습니다. &quot;손상된 소스&quot; 검사를 통과하기 위해 존재해야 합니다. `rendition.instructions` 유효성을 검사하여 이 테스트 사례가 유효한지 확인합니다.
1. 다음에서 `error-contrast` 폴더, 이름이 인 새 파일 추가 `params.json` 는 내용이 있는 작업자의 입력 매개변수를 정의합니다.

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 설정 `contrast` 매개 변수 `10`를 throw하려면 대비가 -1과 1 사이여야 하므로 잘못된 값입니다. `RenditionInstructionsError`.
   + 를 설정하여 테스트에서 적절한 오류가 발생했음을 어설션합니다. `errorReason` 예상 오류와 연결된 &quot;이유&quot;에 대한 키입니다. 이 잘못된 대비 매개 변수는 [사용자 지정 오류](../develop/worker.md#errors), `RenditionInstructionsError`를 선택한 다음 `errorReason` 이 오류의 원인에 대해 또는`rendition_instructions_error` 를 어설션하려면 를 throw합니다.

1. 오류가 발생한 실행 중에는 렌디션이 생성되지 않으므로 `rendition.<extension>` 파일이 필요합니다.
1. 명령을 실행하여 프로젝트의 루트에서 테스트 세트를 실행합니다. `aio app test`
   + 확인 [Docker 데스크탑](../set-up/development-environment.md#docker) 및 지원 Docker 이미지가 설치 및 시작됨
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 오류 대비](./assets/test/error-contrast/result.png)

## Github의 테스트 사례

최종 테스트 사례는 Github의 다음 위치에서 확인할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 문제 해결

+ [테스트 실행 중에 생성된 렌디션 없음](../troubleshooting.md#test-no-rendition-generated)
+ [테스트에서 잘못된 렌디션을 생성합니다.](../troubleshooting.md#tests-generates-incorrect-rendition)
