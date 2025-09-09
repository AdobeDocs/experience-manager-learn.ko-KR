---
title: Asset Compute 프로젝트의 manifest.yml 구성
description: Asset Compute 프로젝트의 manifest.yml은 배포할 이 프로젝트의 모든 작업자에 대해 설명합니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# manifest.html 구성

Asset Compute 프로젝트의 루트에 있는 `manifest.yml`은(는) 배포할 이 프로젝트의 모든 작업자를 설명합니다.

![manifest.yml](./assets/manifest/manifest.png)

## 기본 작업자 정의

작업자는 `actions` 아래에 Adobe I/O Runtime 작업 항목으로 정의되어 있으며 일련의 구성으로 구성되어 있습니다.

다른 Adobe I/O 통합에 액세스하는 작업자는 `annotations -> require-adobe-auth` 속성을 `true`(으)로 설정해야 합니다. 이 [은(는) ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) 개체를 통해 작업자의 Adobe I/O 자격 증명`params.auth`을(를) 노출합니다. 이 작업은 일반적으로 작업자가 Adobe Photoshop 또는 Lightroom API와 같은 Adobe I/O API를 호출할 때 필요하며 작업자별로 전환할 수 있습니다.

1. 자동 생성된 작업자 `manifest.yml`을(를) 열고 검토합니다. 여러 Asset Compute 작업자가 포함된 프로젝트는 `actions` 배열에서 각 작업자에 대한 항목을 정의해야 합니다.

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
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, or Lightroom.
```

## 제한 정의

각 작업자는 Adobe I/O Runtime에서 해당 실행 컨텍스트에 대해 [제한](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)을 구성할 수 있습니다. 이러한 값은 작업 수행 유형뿐만 아니라 작업자가 계산할 자산의 양, 속도 및 유형에 따라 작업자에게 최적의 크기를 제공하도록 조정되어야 합니다.

한도를 설정하기 전에 [Adobe 크기 조정 지침](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers)을 검토하십시오. Asset Compute 작업자는 에셋을 처리할 때 메모리가 부족하여 Adobe I/O Runtime 실행이 종료될 수 있으므로, 모든 후보 에셋을 처리할 수 있는 작업자 크기를 적절히 조절하십시오.

1. 새 `inputs` 작업 항목에 `wknd-asset-compute` 섹션을 추가합니다. 이를 통해 Asset Compute 작업자의 전체 성능 및 리소스 할당을 조정할 수 있습니다.

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

## Github의 manifest.yml

최종 `.manifest.yml`은(는) Github의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.yml 유효성 검사

생성된 Asset Compute `manifest.yml`이(가) 업데이트되면 로컬 개발 도구를 실행하고 업데이트된 `manifest.yml` 설정으로 성공적으로 시작하는지 확인하십시오.

Asset Compute 프로젝트용 Asset Compute 개발 도구를 시작하려면 다음을 수행하십시오.

1. Asset Compute 프로젝트 루트에서 명령줄을 열고(VS 코드에서 터미널 > 새 터미널을 통해 IDE에서 직접 열 수 있음) 다음 명령을 실행합니다.

   ```
   $ aio app run
   ```

1. 로컬 Asset Compute 개발 도구는 기본 웹 브라우저인 __http://localhost :9000__에서 열립니다.

   ![aio 앱 실행](assets/environment-variables/aio-app-run.png)

1. 개발 도구가 초기화될 때 명령줄 출력 및 웹 브라우저에서 오류 메시지를 확인합니다.
1. Asset Compute 개발 도구를 중지하려면 `Ctrl-C`을(를) 실행한 창에서 `aio app run`을(를) 탭하여 프로세스를 종료하십시오.

## 문제 해결

+ [잘못된 YAML 들여쓰기](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize 제한이 너무 낮게 설정되어 있습니다.](../troubleshooting.md#memorysize-limit-is-set-too-low)
