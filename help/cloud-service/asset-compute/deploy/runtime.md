---
title: as a Cloud Service으로 사용할 Adobe I/O RuntimeAEM 에 Asset compute 작업자 배포
description: Asset compute 프로젝트와 여기에 포함된 작업자를 AEM as a Cloud Service으로 사용하려면 Adobe I/O Runtime에 배포해야 합니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Adobe I/O Runtime에 배포

Asset compute 프로젝트와 여기에 포함된 작업자는 Adobe I/O CLI를 통해 AEM에서 as a Cloud Service으로 사용할 수 있도록 Adobe I/O Runtime에 배포되어야 합니다.

AEM as a Cloud Service 작성자 서비스에서 사용하기 위해 Adobe I/O Runtime에 배포할 때 두 개의 환경 변수만 필요합니다.

+ `AIO_runtime_namespace` 은 배포할 App Builder 작업 영역을 지정합니다.
+ `AIO_runtime_auth` 은 App Builder 작업 영역의 인증 자격 증명입니다

에 정의된 기타 표준 변수 `.env` 파일은 Asset compute 작업자를 호출할 때 AEMas a Cloud Service 에 의해 암시적으로 제공됩니다.

## 개발 작업 영역

이 프로젝트는 다음을 사용하여 생성되었으므로 `aio app init` 사용 `Development` 작업 영역, `AIO_runtime_namespace` 이(가) (으)로 자동 설정됩니다. `81368-wkndaemassetcompute-development` 와 일치 `AIO_runtime_auth` 로컬 `.env` 파일.  다음과 같은 경우 `.env` deploy 명령을 실행하는 데 사용되는 디렉터리에 파일이 있고, 해당 값이 OS 수준 변수 내보내기를 통해 대체되지 않는 한 사용됩니다. [스테이징 및 프로덕션](#stage-and-production) 작업 영역이 타깃팅됩니다.

![.env 변수를 사용하여 aio 앱 배포](./assets/runtime/development__aio.png)

프로젝트에 정의된 작업 공간으로 배포하려면 `.env` 파일:

1. asset compute 프로젝트의 루트에서 명령줄을 엽니다
1. 명령 실행 `aio app deploy`
1. 명령 실행 `aio app get-url` AEM as a Cloud Service 처리 프로필에서 사용할 작업자 URL을 가져와서 이 사용자 지정 Asset compute 작업자를 참조합니다. 프로젝트에 여러 작업자가 포함된 경우 각 작업자에 대한 개별 URL이 나열됩니다.

로컬 개발 및 AEM as a Cloud Service 개발 환경에서 별도의 Asset compute 배포를 사용하는 경우 AEM as a Cloud Service Dev에 대한 배포를 다음과 동일한 방식으로 관리할 수 있습니다. [스테이지 및 프로덕션 배포](#stage-and-production).

## 스테이지 및 프로덕션 작업 공간{#stage-and-production}

스테이지 및 프로덕션 작업 공간에 배포하는 작업은 일반적으로 선택한 CI/CD 시스템에서 수행합니다. asset compute 프로젝트는 각 작업 영역(단계 및 프로덕션)에 개별적으로 배포되어야 합니다.

true 환경 변수를 설정하면 의 동일한 명명된 변수에 대한 값이 무시됩니다. `.env`.

![내보내기 변수를 사용하여 aio 앱 배포](./assets/runtime/stage__export-and-aio.png)

스테이지 및 프로덕션 환경에 배포하기 위해 일반적으로 CI/CD 시스템에 의해 자동화되는 일반적인 접근 방식은 다음과 같습니다.

1. 다음을 확인합니다. [Adobe I/O CLI npm 모듈 및 Asset compute 플러그인](../set-up/development-environment.md#aio) 설치됨
1. Git에서 배포할 Asset compute 프로젝트 확인
1. 대상 작업 공간(스테이지 또는 프로덕션)에 해당하는 값으로 환경 변수를 설정합니다
   + 두 가지 필수 변수는 다음과 같습니다 `AIO_runtime_namespace` 및 `AIO_runtime_auth` 및 는 Workspace를 통해 Adobe I/O 개발자 콘솔의 Workspace별로 가져옵니다. __모두 다운로드__ 기능.

![Adobe Developer 콘솔 - AIO 런타임 네임스페이스 및 인증](./assets/runtime/stage-auth-namespace.png)

이러한 키의 값은 명령줄에서 내보내기 명령을 실행하여 설정할 수 있습니다.

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset compute 작업자가 클라우드 스토리지와 같은 다른 변수를 필요로 하는 경우 환경 변수로도 내보내야 합니다.

1. 배포할 대상 작업 영역에 대해 모든 환경 변수가 설정되면 deploy 명령을 실행합니다.
   + `aio app deploy`
1. AEM as a Cloud Service 처리 프로필에서 참조하는 작업자 URL은 다음을 통해서도 사용할 수 있습니다.
   + `aio app get-url`

asset compute 프로젝트 버전이 변경되면 새 버전을 반영하도록 작업자 URL도 변경되며, URL은 처리 프로필에서 업데이트해야 합니다.

## 작업 공간 API 프로비저닝{#workspace-api-provisioning}

날짜 [Adobe I/O에서 App Builder 프로젝트 설정](../set-up/app-builder.md) 로컬 개발을 지원하기 위해 새 개발 작업 영역이 생성되었으며 __Asset compute, I/O 이벤트__ 및 __I/O 이벤트 관리 API__ 이(가) 추가되었습니다.

다음 __Asset compute, I/O 이벤트__ 및 __I/O 이벤트 관리 API__ API는 로컬 개발에 사용되는 작업 영역에만 명시적으로 추가됩니다. AEM as a Cloud Service 환경과 (독점적으로) 통합되는 작업 영역은 다음과 같습니다 __아님__ API가 자연적으로 AEM에서 as a Cloud Service으로 사용할 수 있게 되므로 이러한 API가 명시적으로 추가되어야 합니다.
