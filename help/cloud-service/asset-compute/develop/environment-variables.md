---
title: asset compute 확장성을 위한 환경 변수 구성
description: 환경 변수는 로컬 개발을 위해 .env 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명과 클라우드 저장소 자격 증명을 제공하는 데 사용됩니다.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# 환경 변수 구성

![도트 env 파일](assets/environment-variables/dot-env-file.png)

asset compute 근로자의 개발을 시작하기 전에 프로젝트가 Adobe I/O 및 클라우드 스토리지 정보로 구성되어 있는지 확인합니다. 이 정보는 프로젝트의 `.env`에 저장되며 로컬 개발에만 사용되며 Git에는 저장되지 않습니다. `.env` 파일은 키/값 쌍을 로컬 Asset compute 로컬 개발 환경에 노출할 수 있는 편리한 방법을 제공합니다. [Asset compute 작업자를 Adobe I/O Runtime에 배포하는 경우 `.env` 파일이 사용되지 않지만, 값의 하위 집합은 환경 변수를 통해 전달됩니다. ](../deploy/runtime.md) 타사 웹 서비스에 대한 개발 자격 증명과 같이 다른 사용자 지정 매개 변수와 기밀은 `.env` 파일에도 저장할 수 있습니다.

## `private.key` 참조

![개인 키](assets/environment-variables/private-key.png)

`.env` 파일을 열고 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 키의 주석을 해제하고 파일 시스템의 절대 경로를 `private.key`에 제공하여 Adobe I/O FireFly 프로젝트에 추가된 공용 인증서와 함께 추가합니다.

+ 키 쌍이 Adobe I/O에 의해 생성된 경우 `config.zip`의 일부로 자동 다운로드되었습니다.
+ 공개 키를 Adobe I/O에 제공한 경우 일치하는 개인 키도 보유해야 합니다.
+ 이러한 키 쌍이 없는 경우 다음 맨 아래에 새 키 쌍을 생성하거나 새 공개 키를 업로드할 수 있습니다.
   [https://console.adobe.com](https://console.adobe.io) > Asset compute Firefly 프로젝트 > 작업 영역 @ 개발 > 서비스 계정(JWT).

`private.key` 파일은 비밀이 포함되어 있으므로 Git에 체크 인되지 않아야 합니다. 그렇지 않으면 프로젝트 외부의 안전한 위치에 저장해야 합니다.

예를 들어, macOS에서는 다음과 같이 표시됩니다.

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 클라우드 저장소 자격 증명 구성

asset compute 근로자의 로컬 개발을 위해서는 [클라우드 스토리지](../set-up/accounts-and-services.md#cloud-storage)에 액세스해야 합니다. 로컬 개발에 사용되는 클라우드 저장소 자격 증명이 `.env` 파일에 제공됩니다.

이 자습서는 Azure Blob 저장소를 사용하는 것을 선호하지만 Amazon S3에서는 대신 `.env` 파일의 해당 키를 사용할 수 있습니다.

### Azure Blob 저장소 사용

`.env` 파일의 다음 키를 주석을 해제하고 채우고 Azure Portal에 있는 제공된 클라우드 저장소 값으로 채웁니다.

![Azure Blob 저장소](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME` 키의 값
1. `AZURE_STORAGE_ACCOUNT` 키의 값
1. `AZURE_STORAGE_KEY` 키의 값

예를 들어, 다음과 같이 보일 수 있습니다(그림에만 대한 값).

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

결과 `.env` 파일은 다음과 같습니다.

![Azure Blob 저장소 자격 증명](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blob 저장소를 사용하지 않는 경우 이러한 주석을 제거하거나 종료하십시오(`#` 접두사로 고정).

### Amazon S3 클라우드 스토리지 사용{#amazon-s3}

Amazon S3 클라우드 스토리지를 사용 중인 경우 주석을 해제하고 `.env` 파일에서 다음 키를 채웁니다.

예를 들어, 다음과 같이 보일 수 있습니다(그림에만 대한 값).

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 프로젝트 구성 유효성 확인

생성된 Asset compute 프로젝트가 구성되면 코드를 변경하기 전에 구성을 검증하여 `.env` 파일에서 지원 서비스가 제공되는지 확인합니다.

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

+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
