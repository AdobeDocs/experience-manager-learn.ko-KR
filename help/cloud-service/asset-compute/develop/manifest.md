---
title: asset compute 프로젝트의 manifest.yml 구성
description: asset compute 프로젝트의 manifest.yml은 이 프로젝트의 배포 대상 모든 작업자에 대해 설명합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# manifest.yml 구성

asset compute 프로젝트의 루트에 있는 `manifest.yml`은 이 프로젝트의 배포 대상 모든 작업자를 설명합니다.

![manifest.yml](./assets/manifest/manifest.png)

## 기본 작업자 정의

워커는 `actions` 아래에 Adobe I/O Runtime 작업 항목으로 정의되며 일련의 구성으로 구성됩니다.

다른 Adobe I/O 통합에 액세스하는 작업자는 `annotations -> require-adobe-auth` 속성을 `true`으로 설정해야 합니다. 이 [는 `params.auth` 개체를 통해 워커의 Adobe I/O 자격 증명](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)을 노출합니다. 이는 일반적으로 워커가 Adobe Photoshop, Lightroom 또는 Sensei API와 같은 Adobe I/O API를 호출하고 작업자별로 전환할 수 있는 경우에 필요합니다.

1. 자동 생성된 작업자 `manifest.yml`를 열고 검토합니다. 여러 Asset compute 작업자를 포함하는 프로젝트는 `actions` 배열 아래에 있는 각 워커에 대한 항목을 정의해야 합니다.

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

## 한도 정의

각 워커는 Adobe I/O Runtime의 실행 컨텍스트에 대해 [limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)을 구성할 수 있습니다. 이러한 값은 작업 유형 뿐만 아니라 계산할 자산의 볼륨, 비율 및 유형에 따라 작업자에게 최적의 크기 조정을 제공하도록 조정되어야 합니다.

제한을 설정하기 전에 [Adobe 크기 조정 지침](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers)을 검토하십시오. asset compute 근로자는 에셋을 처리할 때 메모리가 부족하여 Adobe I/O Runtime 실행이 중단되므로 워커가 모든 후보 에셋을 처리하기 위해 적절한 크기를 갖도록 합니다.

1. 새 `wknd-asset-compute` 작업 항목에 `inputs` 섹션을 추가합니다. 이렇게 하면 Asset compute 근로자의 전반적인 성능 및 리소스 할당을 조정할 수 있습니다.

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

최종 `manifest.yml`은(는) 다음과 같습니다.

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

## github의 manifest.yml

최종 `.manifest.yml`은(는) Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.yml의 유효성을 검사하는 중

생성된 Asset compute `manifest.yml`이 업데이트되면 로컬 개발 도구를 실행하고 업데이트된 `manifest.yml` 설정으로 성공적으로 시작되는지 확인합니다.

asset compute 프로젝트에 대한 Asset compute 개발 도구를 시작하려면 다음을 수행하십시오.

1. asset compute 프로젝트 루트에서 명령줄을 열고(VS 코드에서는 터미널 > 새 터미널을 통해 IDE에서 직접 열 수 있음) 명령을 실행합니다.

   ```
   $ aio app run
   ```

1. 로컬 Asset compute 개발 도구는 __http://localhost:9000__&#x200B;의 기본 웹 브라우저에서 열립니다.

   ![aio 앱 실행](assets/environment-variables/aio-app-run.png)

1. 개발 도구가 초기화되면 명령줄 출력 및 웹 브라우저에서 오류 메시지를 확인합니다.
1. asset compute 개발 도구를 중지하려면 `aio app run`을(를) 실행한 창에서 `Ctrl-C`을(를) 눌러 프로세스를 종료합니다.

## 문제 해결

+ [잘못된 YAML 들여쓰기](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize 제한이 너무 낮게 설정되어 있습니다.](../troubleshooting.md#memorysize-limit-is-set-too-low)
