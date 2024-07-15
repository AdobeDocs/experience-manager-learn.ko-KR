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
duration: 142
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# asset compute 작업자 테스트

asset compute 프로젝트는 Asset compute 작업자의 [테스트](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html)를 쉽게 만들고 실행하기 위한 패턴을 정의합니다.

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
   + 테스트할 Source 파일(확장명은 `.link`을(를) 제외한 다른 형식일 수 있음)
   + 필수
+ `rendition.<extension>`
   + 예상 렌디션
   + 오류 테스트 제외 필수
+ `params.json`
   + 단일 렌디션 JSON 지침
   + 옵션
+ `validate`
   + 예상 및 실제 렌디션 파일 경로를 인수로 가져오고 결과가 양호한 경우 종료 코드 0을 반환해야 하며, 유효성 검사나 비교가 실패한 경우 0이 아닌 종료 코드를 반환해야 하는 스크립트.
   + 선택 사항이며 기본값은 `diff` 명령입니다.
   + 다른 유효성 검사 도구를 사용하기 위해 도커 실행 명령을 래핑하는 셸 스크립트를 사용합니다
+ `mock-<host-name>.json`
   + [외부 서비스 호출 조롱](https://www.mock-server.com/mock_server/creating_expectations.html)에 대한 JSON 형식의 HTTP 응답입니다.
   + 선택 사항이며, 작업자 코드가 자체의 HTTP 요청을 하는 경우에만 사용됩니다

## 테스트 사례 작성

이 테스트 사례에서는 입력 파일(`file.jpg`)에 대해 매개 변수가 있는 입력(`params.json`)을 어설션하여 예상 PNG 렌디션(`rendition.png`)을 생성합니다.

1. 작업자가 더 이상 소스를 변환에 복사하지 않으므로 `/test/asset-compute/simple-worker`에서 자동 생성된 `simple-worker` 테스트 사례를 먼저 삭제하십시오.
1. PNG 렌디션을 생성하는 작업자의 성공적인 실행을 테스트하려면 `/test/asset-compute/worker/success-parameterized`에 새 테스트 사례 폴더를 만드십시오.
1. `success-parameterized` 폴더에서 이 테스트 사례에 대한 테스트 [입력 파일](./assets/test/success-parameterized/file.jpg)을(를) 추가하고 이름을 `file.jpg`(으)로 지정합니다.
1. `success-parameterized` 폴더에 작업자의 입력 매개 변수를 정의하는 이름이 `params.json`인 새 파일을 추가합니다.

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   [개발 도구의 Asset compute 프로필 정의](../develop/development-tool.md)에 전달된 동일한 키/값이며 `worker` 키를 줄입니다.

1. 예상 [렌디션 파일](./assets/test/success-parameterized/rendition.png)을(를) 이 테스트 사례에 추가하고 이름을 `rendition.png`로 지정합니다. 이 파일은 지정된 입력 `file.jpg`에 대한 작업자의 예상 출력을 나타냅니다.
1. 명령줄에서 `aio app test`을(를) 실행하여 프로젝트 루트를 테스트합니다.
   + [Docker 데스크톱](../set-up/development-environment.md#docker) 및 지원되는 Docker 이미지가 설치 및 시작되었는지 확인
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 성공 ](./assets/test/success-parameterized/result.png)

## 오류 검사 테스트 사례 작성

이 테스트 사례는 `contrast` 매개 변수가 잘못된 값으로 설정된 경우 작업자가 적절한 오류를 발생시키는지 확인하기 위해 테스트합니다.

1. `/test/asset-compute/worker/error-contrast`에 새 테스트 사례 폴더를 만들어 잘못된 `contrast` 매개 변수 값으로 인해 작업자 오류 실행을 테스트하십시오.
1. `error-contrast` 폴더에서 이 테스트 사례에 대한 테스트 [입력 파일](./assets/test/error-contrast/file.jpg)을(를) 추가하고 이름을 `file.jpg`(으)로 지정합니다. 이 파일의 내용은 이 테스트에 중요하지 않습니다. 이 테스트 케이스가 유효성을 검사하는 `rendition.instructions` 유효성 검사에 도달하려면 &quot;손상된 원본&quot; 검사를 통과하기 위한 파일만 있으면 됩니다.
1. `error-contrast` 폴더에 내용이 포함된 작업자의 입력 매개 변수를 정의하는 이름이 `params.json`인 새 파일을 추가합니다.

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + `contrast` 매개 변수를 `10`(으)로 설정하십시오. `RenditionInstructionsError`을(를) throw하려면 대비가 -1에서 1 사이여야 하므로 잘못된 값입니다.
   + `errorReason` 키를 예상 오류와 연결된 &quot;reason&quot;으로 설정하여 테스트에서 적절한 오류가 throw되었음을 어설션합니다. 이 잘못된 대비 매개 변수는 [사용자 지정 오류](../develop/worker.md#errors), `RenditionInstructionsError`을(를) throw하므로 `errorReason`을(를) 이 오류의 이유로 설정하거나 `rendition_instructions_error`을(를) throw한다고 어설션합니다.

1. 오류 실행 중에는 렌디션을 생성하지 않아야 하므로 `rendition.<extension>` 파일이 필요하지 않습니다.
1. `aio app test` 명령을 실행하여 프로젝트의 루트에서 테스트 도구 모음을 실행합니다.
   + [Docker 데스크톱](../set-up/development-environment.md#docker) 및 지원되는 Docker 이미지가 설치 및 시작되었는지 확인
   + 실행 중인 개발 도구 인스턴스 종료

![테스트 - 오류 대비](./assets/test/error-contrast/result.png)

## Github의 테스트 사례

최종 테스트 사례는 Github의 다음 위치에서 확인할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 문제 해결

+ [테스트 실행 중에 생성된 렌디션 없음](../troubleshooting.md#test-no-rendition-generated)
+ [테스트에서 잘못된 렌디션을 생성합니다.](../troubleshooting.md#tests-generates-incorrect-rendition)
