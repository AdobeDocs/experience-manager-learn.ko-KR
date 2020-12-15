---
title: AEM과 함께 Cloud Service으로 사용할 수 있도록 Adobe I/O Runtime에 Asset compute 직원 배포
description: 'asset compute 프로젝트 및 포함된 작업자는 AEM에서 Cloud Service으로 사용하려면 Adobe I/O Runtime에 배포해야 합니다. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# Adobe I/O Runtime에 배포

asset compute 프로젝트와 포함된 작업자는 AEM에서 Cloud Service으로 사용할 Adobe I/O CLI를 통해 Adobe I/O Runtime에 배포해야 합니다.

AEM에서 Cloud Service 작성 서비스로 사용하기 위해 Adobe I/O Runtime에 배포하는 경우 2개의 환경 변수만 필요합니다.

+ `AIO_runtime_namespace` 배포할 Adobe 프로젝트 Firefly 작업 공간을 가리킵니다.
+ `AIO_runtime_auth` adobe 프로젝트 Firefly 작업 공간의 인증 자격 증명입니다.

`.env` 파일에 정의된 다른 표준 변수는 Asset compute 워커를 호출할 때 AEM에서 Cloud Service으로 암시적으로 제공됩니다.

## 개발 작업 영역

이 프로젝트는 `Development` 작업 영역을 사용하여 `aio app init`을(를) 사용하여 생성되었으므로 `AIO_runtime_namespace`은(는) 로컬 `.env` 파일에서 일치하는 `AIO_runtime_auth`와 함께 `81368-wkndaemassetcompute-development`으로 자동 설정됩니다.  `.env` 파일이 배포 명령을 실행하는 데 사용된 디렉토리에 있으면 [스테이지 및 프로덕션](#stage-and-production) 작업 영역을 대상으로 하는 OS 수준 변수 내보내기를 통해 대체되지 않는 한 해당 값이 사용됩니다.

![.env 변수를 사용하여 앱 배포](./assets/runtime/development__aio.png)

작업 공간에 배포하려면 `.env` 프로젝트 파일에서 정의합니다.

1. asset compute 프로젝트의 루트에서 명령줄을 엽니다.
1. `aio app deploy` 명령 실행
1. `aio app get-url` 명령을 실행하여 AEM에서 Cloud Service 처리 프로필로 사용할 작업자 URL을 가져와 이 사용자 지정 Asset compute 작업자를 참조합니다. 프로젝트에 여러 작업자가 포함된 경우 각 작업자에 대한 단속 URL이 나열됩니다.

Cloud Service 개발 환경으로 로컬 개발 및 AEM이 별도의 Asset compute 배포를 사용하는 경우 Cloud Service Dev로 AEM에 대한 배포는 [스테이지 및 프로덕션 배포](#stage-and-production)와 동일한 방식으로 관리할 수 있습니다.

## 스테이지 및 프로덕션 작업 영역{#stage-and-production}

스테이지 및 프로덕션 작업 영역에 배포하는 작업은 일반적으로 원하는 CI/CD 시스템에 의해 수행됩니다. asset compute 프로젝트를 각 작업 공간(스테이지 및 프로덕션)에 개별적으로 배포해야 합니다.

true 환경 변수를 설정하면 `.env`에 있는 동일한 이름의 변수에 대한 값이 재정의됩니다.

![내보내기 변수를 사용하여 앱 배포](./assets/runtime/stage__export-and-aio.png)

일반적으로 CI/CD 시스템에서 스테이지 및 프로덕션 환경에 배포하는 일반적인 방법은 다음과 같습니다.

1. [Adobe I/O CLI npm 모듈 및 Asset compute 플러그인](../set-up/development-environment.md#aio)이(가) 설치되어 있는지 확인합니다.
1. Git에서 배포할 Asset compute 프로젝트 확인
1. 대상 작업 공간(스테이지 또는 프로덕션)에 해당하는 값으로 환경 변수를 설정합니다.
   + 두 개의 필수 변수는 `AIO_runtime_namespace` 및 `AIO_runtime_auth`이며, 작업 공간의 __모두 다운로드__ 기능을 통해 Adobe I/O 개발자 콘솔에서 작업 영역별로 가져옵니다.

![Adobe 개발자 콘솔 - AIO 런타임 네임스페이스 및 인증](./assets/runtime/stage-auth-namespace.png)

다음 키의 값은 명령줄에서 내보내기 명령을 실행하여 설정할 수 있습니다.

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset compute 근로자가 클라우드 스토리지와 같은 다른 변수가 필요한 경우 이러한 변수를 환경 변수로도 내보내야 합니다.

1. 배포할 대상 작업 영역에 대해 모든 환경 변수가 설정되면 배포 명령을 실행합니다.
   + `aio app deploy`
1. AEM에서 Cloud Service 처리 프로필로 참조하는 작업자 URL도 다음 방법을 통해 사용할 수 있습니다.
   + `aio app get-url`.

asset compute 프로젝트 버전이 작업자 URL도 새 버전을 반영하도록 변경되면 처리 프로필에서 URL을 업데이트해야 합니다.

## 작업 공간 API 프로비저닝{#workspace-api-provisioning}

로컬 개발을 지원하기 위해 Adobe I/O[에서 Adobe 프로젝트 Firefly 프로젝트를 설정하는 경우 새 개발 작업 영역이 만들어지고 ](../set-up/firefly.md)Asset compute, I/O 이벤트&#x200B;__및__ I/O 이벤트 관리 API __가 이 프로젝트에 추가되었습니다.__

__Asset compute, I/O 이벤트__ 및 __I/O 이벤트 관리 API__ API는 로컬 개발에 사용되는 작업 영역에만 명시적으로 추가됩니다. Cloud Service 환경으로 AEM과 통합(전용)하는 작업 영역은 API가 Cloud Service으로 자연스럽게 AEM에서 사용할 수 있도록 하기 위해 이러한 API를 명시적으로 추가해야 하는 __가 아닙니다.__
