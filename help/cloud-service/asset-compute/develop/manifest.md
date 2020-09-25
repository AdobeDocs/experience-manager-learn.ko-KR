---
title: 자산 계산 프로젝트의 manifest.yml 구성
description: 자산 계산 프로젝트의 manifest.yml은 이 응용 프로그램의 배포 대상 모든 작업자에 대해 설명합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---


# manifest.html 구성

자산 계산 프로젝트 `manifest.yml`의 루트에 있는 이 페이지에서는 이 프로젝트의 모든 작업자를 배포할 수 있습니다.

![manifest.yml](./assets/manifest/manifest.png)

## 기본 작업자 정의

작업자는 아래의 Adobe I/O Runtime 작업 항목 `actions`으로 정의되며 일련의 구성으로 구성됩니다.

다른 Adobe I/O 통합에 액세스하는 작업자는 개체를 `annotations -> require-adobe-auth` 통해 근로자의 Adobe I/O 자격 증명을 `true` 노출하므로 [속성을 로 설정해야](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) `params.auth` 합니다. 이는 일반적으로 근로자가 Adobe Photoshop, Lightroom 또는 Sensei API와 같은 Adobe I/O API를 호출하고 작업자별로 전환할 수 있는 경우에 필요합니다.

1. 자동 생성된 작업자를 열고 검토합니다 `manifest.yml`. 여러 자산 계산 작업자를 포함하는 프로젝트는 배열 아래에 있는 각 작업자에 대한 항목을 정의해야 `actions` 합니다.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## 제한 정의

각 작업자는 Adobe I/O Runtime에서 실행 컨텍스트에 대한 [제한을](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) 구성할 수 있습니다. 이러한 값은 작업자가 수행하는 작업의 유형뿐만 아니라 계산할 자산의 볼륨, 비율 및 유형에 따라 작업자에 대한 최적의 크기 조정을 제공하도록 조정되어야 합니다.

제한을 설정하기 전에 [Adobe 크기](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) 지침을 검토하십시오. 에셋 계산 작업자는 에셋을 처리할 때 메모리가 부족하여 Adobe I/O Runtime 실행이 중단되므로 워커의 크기가 모든 후보 에셋을 처리하기 위해 적절하게 조정되었는지 확인합니다.

1. 새 `inputs` 작업 항목에 섹션을 `wknd-asset-compute` 추가합니다. 이를 통해 자산 계산 근로자의 전반적인 성능 및 리소스 할당을 조정할 수 있습니다.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## 완료된 manifest.yml

최종 `manifest.yml` 결과는 다음과 같습니다.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.html 유효성 확인

생성된 에셋 계산 `manifest.yml` 이 업데이트되면 로컬 개발 도구를 실행하고 업데이트된 `manifest.yml` 설정으로 성공적으로 시작할 수 있습니다.

자산 계산 프로젝트에 대한 자산 계산 개발 도구를 시작하려면

1. 자산 계산 프로젝트 루트에서 명령줄을 열고(VS 코드에서는 터미널 > 새 터미널을 통해 IDE에서 직접 열 수 있음) 명령을 실행합니다.

   ```
   $ aio app run
   ```

1. 로컬 에셋 컴퓨팅 개발 도구가 기본 웹 브라우저(http://localhost:9000)에서 __열립니다__.

   ![aio 앱 실행](assets/environment-variables/aio-app-run.png)

1. 개발 도구가 초기화되면 명령줄 출력 및 웹 브라우저에서 오류 메시지를 확인합니다.
1. 자산 계산 개발 도구를 중지하려면, 실행된 창 `Ctrl-C` 에서 을 탭하여 프로세스를 `aio app run` 종료합니다.

## 문제 해결

### 잘못된 YAML 들여쓰기

+ __오류:__ YAMLException:라인 X, 열 Y:(표준 출력 명령을 통해)에서 매핑 항목의 잘못된 들여쓰기 `aio app run` 가
+ __원인:__ Yaml 파일은 흰색 간격에 민감하므로 들여쓰기가 올바르지 않을 수 있습니다.
+ __해결 방법:__ 문서를 검토하고 `manifest.yml` 모든 들여쓰기가 올바른지 확인하십시오.

### memorySize 제한이 너무 낮게 설정되어 있습니다.

+ __오류:__ 로컬 개발 서버 OpenWhiskError:PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true이(가) HTTP 400을 반환했습니다(잘못된 요청) —> &quot;요청 컨텐츠의 형식이 잘못되었습니다. 요구 사항 실패:메모리 64MB가 허용된 임계값 134217728B&quot;보다 낮음
+ __원인:__ 매니페스트의 `memorySize` 제한이 오류 메시지에 의해 보고되는 최소 허용 임계값(바이트)보다 낮게 설정되었습니다.
+ __해결 방법:__ 제한 `memorySize` 을 검토하고 `manifest.yml` 모두 허용된 최소 임계값보다 커야 합니다.