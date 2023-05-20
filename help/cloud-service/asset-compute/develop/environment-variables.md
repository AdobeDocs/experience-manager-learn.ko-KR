---
title: asset compute 확장성을 위한 환경 변수 구성
description: 환경 변수는 로컬 개발을 위해 .env 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명 및 클라우드 스토리지 자격 증명을 제공하는 데 사용됩니다.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# 환경 변수 구성

![점 환경 파일](assets/environment-variables/dot-env-file.png)

asset compute 작업자의 개발을 시작하기 전에 프로젝트가 Adobe I/O 및 클라우드 스토리지 정보로 구성되었는지 확인하십시오. 이 정보는 프로젝트의 `.env`  로컬 개발에만 사용되고 Git에는 저장되지 않습니다. 다음 `.env` 파일은 키/값 쌍을 로컬 Asset compute 로컬 개발 환경에 노출하는 편리한 방법을 제공합니다. 날짜 [배포](../deploy/runtime.md) Adobe I/O Runtime에 작업자 asset compute, `.env` 파일이 사용되지 않고 값의 하위 집합이 환경 변수를 통해 전달됩니다. 다른 사용자 지정 매개 변수와 비밀은 `.env` 타사 웹 서비스에 대한 개발 자격 증명과 같은 파일도 제공합니다.

## 참조 `private.key`

![개인 키](assets/environment-variables/private-key.png)

를 엽니다. `.env` 파일, 주석 제거 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 키 및 파일 시스템의 절대 경로를 `private.key` Adobe I/O 앱 빌더 프로젝트에 추가된 공개 인증서와 쌍을 이룹니다.

+ 키 쌍이 Adobe I/O에 의해 생성된 경우 의 일부로 자동 다운로드되었습니다  `config.zip`.
+ 공개 키를 Adobe I/O에 제공한 경우 일치하는 개인 키도 보유해야 합니다.
+ 이러한 키 쌍이 없는 경우 새 키 쌍을 생성하거나 아래쪽의 새 공개 키를 업로드할 수 있습니다.
   [https://console.adobe.com](https://console.adobe.io) > Asset compute 앱 빌더 프로젝트 > 작업 공간 @ 개발 > 서비스 계정(JWT).

다음을 기억하십시오. `private.key` 파일에 비밀이 포함되어 있으므로 Git으로 체크인해서는 안 되며, 프로젝트 외부의 안전한 장소에 저장해야 합니다.

예를 들어 macOS에서 다음과 같을 수 있습니다.

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 클라우드 스토리지 자격 증명 구성

asset compute 작업자의 로컬 개발을 위해서는 다음에 대한 액세스 권한이 필요합니다. [클라우드 스토리지](../set-up/accounts-and-services.md#cloud-storage). 로컬 개발에 사용되는 클라우드 스토리지 자격 증명은에서 제공됩니다. `.env` 파일.

이 자습서에서는 Azure Blob 저장소를 사용하지만 Amazon S3 및 `.env` 파일을 대신 사용할 수 있습니다.

### Azure Blob 저장소 사용

주석 처리를 제거하고 `.env` 파일을 만든 다음 Azure 포털에 있는 프로비저닝된 클라우드 스토리지의 값으로 채웁니다.

![Azure Blob 저장소](./assets/environment-variables/azure-portal-credentials.png)

1. 값: `AZURE_STORAGE_CONTAINER_NAME` key
1. 값: `AZURE_STORAGE_ACCOUNT` key
1. 값: `AZURE_STORAGE_KEY` key

예를 들어, 다음과 같이 표시될 수 있습니다(그림 전용 값).

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

결과 `.env` 파일은 다음과 같습니다.

![Azure Blob 저장소 자격 증명](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blob 저장소를 사용하지 않는 경우 접두사로 을 추가하여 주석 처리된 파일을 제거하거나 그대로 둡니다. `#`).

### Amazon S3 클라우드 스토리지 사용{#amazon-s3}

Amazon S3 클라우드 저장소를 사용하는 경우 주석 처리를 제거하고 `.env` 파일.

예를 들어, 다음과 같이 표시될 수 있습니다(그림 전용 값).

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 프로젝트 구성 확인

생성된 Asset compute 프로젝트가 구성되면에서 지원 서비스가 프로비저닝되었는지 확인하기 위해 코드를 변경하기 전에 구성을 확인합니다. `.env` 파일.

asset compute 프로젝트에 대한 Asset compute 개발 도구를 시작하려면 다음을 수행하십시오.

1. asset compute 프로젝트 루트의 명령줄을 열고(VS 코드에서 터미널 > 새 터미널을 통해 IDE에서 직접 열 수 있음) 다음 명령을 실행합니다.

   ```
   $ aio app run
   ```

1. 로컬 Asset compute 개발 도구 가 기본 웹 브라우저인 __http://localhost:9000__.

   ![aio 앱 실행](assets/environment-variables/aio-app-run.png)

1. 개발 도구가 초기화될 때 명령줄 출력 및 웹 브라우저에서 오류 메시지를 확인합니다.
1. asset compute 개발 도구를 중지하려면 `Ctrl-C` 실행한 창에서 `aio app run` 프로세스를 종료합니다.

## 문제 해결

+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
