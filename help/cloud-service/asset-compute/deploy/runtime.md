---
title: Cloud Service으로 AEM과 함께 사용할 에셋 컴퓨팅 작업자 배포
description: '자산 계산 프로젝트와 여기에 포함된 작업자는 AEM에서 Cloud Service으로 사용하려면 Adobe I/O Runtime에 배포되어야 합니다. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 0%

---


# Adobe I/O Runtime에 배포

자산 계산 프로젝트와 포함된 작업자는 AEM에서 Cloud Service으로 사용할 Adobe I/O CLI를 통해 Adobe I/O Runtime에 배포해야 합니다.

AEM에서 Cloud Service 작성자 서비스로 사용하기 위해 Adobe I/O Runtime에 배포하는 경우 두 개의 환경 변수만 필요합니다.

+ `AIO_runtime_namespace` 배포할 Adobe 프로젝트 Firefly 작업 공간을 가리킵니다.
+ `AIO_runtime_auth` adobe 프로젝트 Firefly 작업 공간의 인증 자격 증명입니다.

파일에 정의된 다른 표준 변수는 자산 계산 작업자를 호출할 때 AEM에서 Cloud Service으로 암시적으로 제공됩니다. `.env`

## 개발 작업 공간

이 프로젝트는 작업 공간 `aio app init` 을 사용하여 생성되었으므로 로컬 `Development``AIO_runtime_namespace` 파일 `81368-wkndaemassetcompute-development` 에서 일치하는 것으로 자동으로 `AIO_runtime_auth` `.env` 설정됩니다.  배포 명령을 실행하는 데 사용되는 디렉토리에 파일이 `.env` 있는 경우, [스테이지 및 프로덕션](#stage-and-production) 작업 공간을 대상으로 하는 OS 수준 변수 내보내기를 통해 파일을 대체하지 않는 한 해당 값이 사용됩니다.

![.env 변수를 사용하여 앱 배포](./assets/runtime/development__aio.png)

작업 공간에 배포하려면 프로젝트 `.env` 파일에서 다음을 정의합니다.

1. 자산 계산 응용 프로그램 프로젝트의 루트에서 명령줄을 엽니다.
1. 명령 실행 `aio app deploy`
1. 이 사용자 지정 자산 계산 작업자 `aio app get-url` 를 참조하는 Cloud Service 처리 프로필로 AEM에서 사용할 작업자 URL을 얻으려면 명령을 실행합니다. 프로젝트에 여러 작업자가 포함된 경우 각 작업자에 대한 개별 URL이 나열됩니다.

Cloud Service 개발 환경으로 로컬 개발 및 AEM에서 별도의 에셋 컴퓨팅 배포를 사용하는 경우, Cloud Service Dev로 AEM에 배포하는 것은 [스테이지 및 프로덕션 배포와 동일한 방식으로 관리할 수 있습니다](#stage-and-production).

## 스테이지 및 프로덕션 작업 영역{#stage-and-production}

스테이지 및 프로덕션 작업 영역에 배포하는 작업은 일반적으로 원하는 CI/CD 시스템에서 수행합니다. 자산 계산 프로젝트는 각 작업 공간(스테이지 및 프로덕션)에 개별적으로 배포해야 합니다.

true 환경 변수를 설정하면 에서 동일한 이름의 변수에 대한 값이 재정의됩니다 `.env`.

![내보내기 변수를 사용하여 앱 배포](./assets/runtime/stage__export-and-aio.png)

스테이지 및 프로덕션 환경에 배포하기 위해 일반적으로 CI/CD 시스템에 의해 자동화되는 일반적인 방법은 다음과 같습니다.

1. Adobe [I/O CLI npm 모듈 및 Asset Compute 플러그인이](../set-up/development-environment.md#aio) 설치되어 있는지 확인합니다.
1. Git에서 배포할 에셋 컴퓨팅 응용 프로그램 확인
1. 대상 작업 공간(스테이지 또는 프로덕션)에 해당하는 값으로 환경 변수를 설정합니다.
   + 두 가지 필수 변수 `AIO_runtime_namespace` 는 작업 공간의 모든 `AIO_runtime_auth` 다운로드 기능을 통해 Adobe I/O 개발자 콘솔의 작업 공간별로 획득되며 __이러한 변수들은 모두__ 사용됩니다.

![Adobe 개발자 콘솔 - AIO 런타임 네임스페이스 및 인증](./assets/runtime/stage-auth-namespace.png)

이러한 키의 값은 명령줄에서 내보내기 명령을 실행하여 설정할 수 있습니다.

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

자산 계산 작업자에게 클라우드 스토리지와 같은 다른 변수가 필요한 경우 환경 변수로도 내보내야 합니다.

1. 대상 작업 공간이 배포되도록 모든 환경 변수가 설정되면 배포 명령을 실행합니다.
   + `aio app deploy`
1. AEM에서 Cloud Service 처리 프로필로 참조한 작업자 URL은 다음을 통해서도 사용할 수 있습니다.
   + `aio app get-url`.

자산 계산 응용 프로그램 버전이 작업자 URL도 새 버전을 반영하도록 변경되면 처리 프로필에서 URL을 업데이트해야 합니다.

## 작업 공간 API 프로비저닝{#workspace-api-provisioning}

Adobe I/O [에서 로컬 개발을 지원하기 위해 Adobe 프로젝트 Firefly 프로젝트를 설정할 때 새로운 개발 작업 공간이 만들어지고](../set-up/firefly.md) 자산 계산, I/O 이벤트 __및__ I/O 이벤트 관리 API가 __여기에__ 추가되었습니다.

자산 __계산, I/O 이벤트__ 및 __I/O 이벤트 관리 APIs__ API는 로컬 개발에 사용되는 작업 영역에만 명시적으로 추가됩니다. Cloud Service 환경으로 AEM과 통합(전용)되는 작업 영역은 API가 AEM에서 Cloud Service으로 자연스럽게 사용할 수 있게 됨에 따라 명시적으로 추가된 API가 __필요하지 않습니다__ .
