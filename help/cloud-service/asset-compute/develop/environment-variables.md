---
title: 자산 계산 확장성을 위한 환경 변수 구성
description: 환경 변수는 로컬 개발을 위해 .env 파일에서 유지되며 로컬 개발에 필요한 Adobe I/O 자격 증명과 클라우드 저장소 자격 증명을 제공하는 데 사용됩니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# 환경 변수 구성

![점 env 파일](assets/environment-variables/dot-env-file.png)

에셋 컴퓨팅 작업자 개발을 시작하기 전에 프로젝트가 Adobe I/O 및 클라우드 스토리지 정보로 구성되어 있는지 확인합니다. 이 정보는 로컬 개발용으로만 사용되는 프로젝트 `.env` 에 저장되며 Git에는 저장되지 않습니다. 이 `.env` 파일은 키/값 쌍을 로컬 Asset Compute 로컬 개발 환경에 노출할 수 있는 편리한 방법을 제공합니다. 자산 계산 작업자를 Adobe I/O Runtime에 [배포할](../deploy/runtime.md) 때 `.env` 파일은 사용되지 않지만, 대신 값의 하위 세트가 환경 변수를 통해 전달됩니다. 타사 웹 서비스에 대한 개발 자격 증명과 같은 다른 사용자 지정 매개 변수 및 기밀은 `.env` 파일에도 저장할 수 있습니다.

## 참조: `private.key`

![개인 키](assets/environment-variables/private-key.png)

파일 `.env` 을 열고 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 키의 주석을 해제하고 파일 시스템의 절대 경로를 Adobe I/O FireFly 프로젝트에 추가된 공용 인증서와 `private.key` 연결된 파일 시스템에 제공합니다.

+ Adobe I/O에서 키 쌍을 생성한 경우 이 키 쌍은 의 일부로 자동 다운로드되었습니다 `config.zip`.
+ 공개 키를 Adobe I/O에 제공한 경우 일치하는 개인 키도 보유해야 합니다.
+ 이러한 키 쌍을 가지고 있지 않으면 새 키 쌍을 생성하거나 다음 맨 아래에 새 공개 키를 업로드할 수 있습니다.
   [https://console.adobe.com](https://console.adobe.io) > 자산 계산 Firefly 프로젝트 > JWT(개발 > 서비스 계정)의 작업 영역

파일에 비밀이 포함되어 있으므로 Git에 체크 인해서는 안 되며, 프로젝트 외부의 안전한 위치에 저장해야 합니다. `private.key`

예를 들어, macOS에서는 다음과 같이 표시됩니다.

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 클라우드 저장소 자격 증명 구성

에셋 컴퓨팅 작업자의 로컬 개발을 위해서는 [클라우드 스토리지에 액세스해야 합니다](../set-up/accounts-and-services.md#cloud-storage). 로컬 개발에 사용되는 클라우드 저장소 자격 증명이 `.env` 파일에 제공됩니다.

이 자습서는 Azure Blob 저장소 사용을 선호하지만 Amazon S3에서는 대신 해당 키를 사용할 수 `.env` 있습니다.

### Azure Blob 저장소 사용

파일의 주석을 해제하고 다음 키를 `.env` 채우고 Azure Portal에 있는 제공된 클라우드 저장소 값으로 채웁니다.

![Azure Blob 저장소](./assets/environment-variables/azure-portal-credentials.png)

1. 키의 `AZURE_STORAGE_CONTAINER_NAME` 값
1. 키의 `AZURE_STORAGE_ACCOUNT` 값
1. 키의 `AZURE_STORAGE_KEY` 값

예를 들어, 다음과 같이 보일 수 있습니다(그림 전용 값).

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

결과 `.env` 파일은 다음과 같습니다.

![Azure Blob 저장소 자격 증명](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blob 저장소를 사용하고 있지 않은 경우, 이러한 댓글을 제거하거나 게시하지 `#`마십시오.

### Amazon S3 클라우드 스토리지 사용{#amazon-s3}

Amazon S3 클라우드 스토리지 주석을 해제하고 `.env` 파일에 다음 키를 채웁니다.

예를 들어, 다음과 같이 보일 수 있습니다(그림 전용 값).

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 프로젝트 구성 유효성 확인

생성된 자산 계산 프로젝트가 구성되면 코드를 변경하기 전에 구성을 검증하여 지원 서비스가 `.env` 파일에 제공되는지 확인합니다.

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

+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
