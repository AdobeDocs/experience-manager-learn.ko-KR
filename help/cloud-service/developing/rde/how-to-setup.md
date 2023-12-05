---
title: 신속한 개발 환경을 설정하는 방법
description: AEM as a Cloud Service으로 신속한 개발 환경을 설정하는 방법에 대해 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 528
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 신속한 개발 환경을 설정하는 방법

학습 **설정 방법** AEMas a Cloud Service 의 RDE(신속한 개발 환경)

이 비디오는 다음을 보여 줍니다.

- Cloud Manager를 사용하여 프로그램에 RDE 추가
- Adobe IMS를 사용한 RDE 로그인 흐름, 다른 AEM as a Cloud Service 환경과 유사 방법
- 설정 [Adobe I/O Runtime 확장 가능 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 또는 `aio CLI`
- AEM RDE 및 Cloud Manager 설정 및 구성 `aio CLI` 플러그인

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 전제 조건

다음은 로컬에 설치해야 합니다.

- [Node.js](https://nodejs.org/en/) (LTS - 장기 지원)
- [npm 8+](https://docs.npmjs.com/)

## 로컬 설정

를 배포하려면 [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 로컬 컴퓨터에서 RDE로 코드 및 콘텐츠를 업로드하려면 다음 단계를 완료하십시오.

### Adobe I/O Runtime 확장 가능 CLI

Adobe I/O Runtime Extensible CLI(CLI)를 설치합니다. `aio CLI` 명령줄에서 다음 명령을 실행합니다.

```shell
$ npm install -g @adobe/aio-cli
```

### AEM 플러그인

다음을 사용하여 Cloud Manager 및 AEM RDE 플러그인 설치 `aio cli`의 `plugins:install` 명령입니다.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager 플러그인을 사용하면 개발자가 명령줄에서 Cloud Manager와 상호 작용할 수 있습니다.

AEM RDE 플러그인을 사용하면 개발자가 로컬 컴퓨터에서 코드와 콘텐츠를 배포할 수 있습니다.

또한 플러그인을 업데이트하려면 `aio plugins:update` 명령입니다.

## AEM 플러그인 구성

AEM 플러그인은 RDE와 상호 작용하도록 구성해야 합니다. 먼저 Cloud Manager UI를 사용하여 조직, 프로그램 및 환경 ID의 값을 복사합니다.

1. 조직 ID: 값을 복사할 원본 위치 **프로필 사진 > 계정 정보(내부) > 모달 창 > 현재 조직 ID**

   ![조직 ID](./assets/Org-ID.png)

1. 프로그램 ID:에서 값 복사 **프로그램 개요 > 환경 > {ProgramName}-rde > 브라우저 URI > 다음 범위의 숫자 `program/` 및`/environment`**

1. 환경 ID:에서 값 복사 **프로그램 개요 > 환경 > {ProgramName}-rde > 브라우저 URI > 다음 숫자`environment/`**

   ![프로그램 및 환경 ID](./assets/Program-Environment-Id.png)

1. 그런 다음 를 사용하여 `aio cli`의 `config:set` 다음 명령을 실행하여 이러한 값을 설정합니다.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

다음 명령을 실행하여 현재 구성 값을 확인할 수 있습니다.

```shell
$ aio config:list
```

또한 아래 명령을 사용하여 현재 로그인한 조직을 전환하거나 알 수 있습니다.

```shell
$ aio where
```

## RDE 액세스 확인

다음 명령을 실행하여 AEM RDE 플러그인 설치 및 구성을 확인합니다.

```shell
$ aio aem:rde:status
```

RDE 상태 정보는 환경 상태, 목록 과 같이 표시됩니다. _내 AEM 프로젝트_ author 및 publish 서비스의 번들 및 구성.

## 다음 단계

학습 [사용 방법](./how-to-use.md) 개발 주기를 단축하기 위해 즐겨 사용하는 IDE(통합 개발 환경)에서 코드와 콘텐츠를 배포하는 RDE입니다.


## 추가 리소스

[프로그램 설명서에서 RDE 활성화](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

설정 [Adobe I/O Runtime 확장 가능 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 또는 `aio CLI`

[AIO CLI 사용 및 명령](https://github.com/adobe/aio-cli#usage)

[AEM Rapid Development 환경과의 상호 작용을 위한 Adobe I/O Runtime CLI 플러그인](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI 플러그인](https://github.com/adobe/aio-cli-plugin-cloudmanager)
