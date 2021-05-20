---
title: AEM as a Cloud Service과 함께 사용할 Adobe I/O Runtime에 Asset compute 작업자 배포
description: 'asset compute 프로젝트 및 여기에 포함된 작업자는 AEM에서 Cloud Service으로 사용하려면 Adobe I/O Runtime에 배포해야 합니다. '
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Adobe I/O Runtime에 배포

asset compute 프로젝트와 여기에 포함된 작업자는 AEM에서 Cloud Service으로 사용하려면 Adobe I/O CLI를 통해 Adobe I/O Runtime에 배포해야 합니다.

AEM as a Cloud Service 작성자 서비스에서 사용하기 위해 Adobe I/O Runtime에 배포할 때에는 두 개의 환경 변수만 필요합니다.

+ `AIO_runtime_namespace` 에서는 배포할 Project Firefly Adobe을 가리킵니다
+ `AIO_runtime_auth` Adobe Project Firefly 작업 공간의 인증 자격 증명은 다음과 같습니다

`.env` 파일에 정의된 다른 표준 변수는 Asset compute 작업자를 호출할 때 AEM에서 Cloud Service으로 암시적으로 제공됩니다.

## 개발 작업 공간

이 프로젝트는 `Development` 작업 공간을 사용하여 `aio app init`을 사용하여 생성되었으므로 `AIO_runtime_namespace`는 로컬 `.env` 파일에서 일치하는 `AIO_runtime_auth`와 함께 자동으로 `81368-wkndaemassetcompute-development`로 설정됩니다.  배포 명령을 실행하는 데 사용되는 디렉터리에 `.env` 파일이 있으면 [스테이지 및 프로덕션](#stage-and-production) 작업 공간이 타깃팅되는 OS 수준 변수 내보내기를 통해 대체되지 않는 한 해당 값이 사용됩니다.

![.env 변수를 사용하여 앱 배포](./assets/runtime/development__aio.png)

작업 공간에 배포하려면 프로젝트 `.env` 파일에 를 정의합니다.

1. asset compute 프로젝트의 루트에서 명령줄을 엽니다.
1. `aio app deploy` 명령을 실행합니다.
1. `aio app get-url` 명령을 실행하여 AEM에서 Cloud Service 처리 프로필로 사용할 작업자 URL을 가져와 이 사용자 정의 Asset compute 작업자를 참조합니다. 프로젝트에 여러 작업자가 포함되어 있는 경우 각 작업자에 대한 단속 URL이 나열됩니다.

로컬 개발 및 AEM as a Cloud Service 개발 환경에서 별도의 Asset compute 배포를 사용하는 경우 AEM as a Cloud Service 개발 배포는 [스테이지 및 프로덕션 배포](#stage-and-production)와 동일한 방식으로 관리할 수 있습니다.

## 스테이지 및 프로덕션 작업 공간{#stage-and-production}

스테이지 및 프로덕션 작업 공간에 배포는 일반적으로 원하는 CI/CD 시스템에서 수행합니다. asset compute 프로젝트를 각 작업 공간(스테이지 및 프로덕션)에 정확하게 배포해야 합니다.

true 환경 변수를 설정하면 `.env`에서 동일한 이름의 변수에 대한 값이 재정의됩니다.

![내보내기 변수를 사용하여 앱 배포](./assets/runtime/stage__export-and-aio.png)

일반적으로 CI/CD 시스템에서 스테이지 및 프로덕션 환경에 배포하는 일반적인 방법은 다음과 같습니다.

1. [Adobe I/O CLI npm 모듈과 Asset compute 플러그인](../set-up/development-environment.md#aio)이 설치되어 있는지 확인합니다.
1. Git에서 배포할 Asset compute 프로젝트 확인
1. 대상 작업 공간(스테이지 또는 프로덕션)에 해당하는 값으로 환경 변수를 설정합니다
   + 두 개의 필수 변수는 `AIO_runtime_namespace` 및 `AIO_runtime_auth` 이며, Workspace의 __모두 다운로드__ 기능을 통해 Adobe I/O 개발자 콘솔에서 작업 공간별로 가져옵니다.

![Adobe 개발자 콘솔 - AIO 런타임 네임스페이스 및 인증](./assets/runtime/stage-auth-namespace.png)

이러한 키의 값은 명령줄에서 내보내기 명령을 실행하여 설정할 수 있습니다.

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset compute 작업자에게 클라우드 스토리지 등의 다른 변수가 필요한 경우 환경 변수로도 내보내야 합니다.

1. 배포할 대상 작업 영역에 대해 모든 환경 변수가 설정되면 deploy 명령을 실행합니다.
   + `aio app deploy`
1. AEM에서 Cloud Service 처리 프로필로 참조하는 작업자 URL도 다음을 통해 사용할 수 있습니다.
   + `aio app get-url`.

asset compute 프로젝트 버전이 작업자 URL도 변경하여 새 버전을 반영하므로 처리 프로필에서 URL을 업데이트해야 합니다.

## 작업 공간 API 프로비저닝{#workspace-api-provisioning}

Adobe I/O](../set-up/firefly.md)에서 로컬 개발을 지원하기 위해 [Adobe Project Firefly 프로젝트를 설정할 때 새 개발 작업 영역이 생성되어 __Asset compute, I/O 이벤트__ 및 __I/O 이벤트 관리 API__&#x200B;가 추가되었습니다.

__Asset compute, I/O 이벤트__ 및 __I/O 이벤트 관리 API__ API는 로컬 개발에 사용되는 작업 영역에만 명시적으로 추가됩니다. AEM as a Cloud Service 환경으로 통합(전용)하는 작업 공간은 API가 자연스럽게 AEM에서 Cloud Service으로 사용할 수 있게 되므로 __명시적으로 추가되지 않아도 됩니다.__
