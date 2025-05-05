---
title: 신속한 개발 환경을 설정하는 방법
description: AEM as a Cloud Service을 위한 신속한 개발 환경을 설정하는 방법에 대해 알아봅니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# 신속한 개발 환경을 설정하는 방법

AEM as a Cloud Service에서 **RDE(빠른 개발 환경)를 설정하는 방법**&#x200B;을 알아봅니다.

이 비디오는 다음을 보여 줍니다.

- Cloud Manager을 사용하여 프로그램에 RDE 추가
- Adobe IMS를 사용하는 RDE 로그인 흐름, 다른 AEM as a Cloud Service 환경과 유사한 방식
- `aio CLI`이라고도 하는 [Adobe I/O Runtime 확장 가능 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 설정
- 비대화형 모드를 사용하여 AEM RDE 및 Cloud Manager `aio CLI` 플러그인을 설정하고 구성합니다. 대화형 모드의 경우 [설치 지침](#setup-the-aem-rde-plugin)을 참조하세요.

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 전제 조건

다음은 로컬에 설치해야 합니다.

- [Node.js](https://nodejs.org/en/)&#x200B;(LTS - 장기 지원)
- [npm 8+](https://docs.npmjs.com/)

## 로컬 설정

[WKND Sites 프로젝트의](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 코드와 콘텐츠를 로컬 컴퓨터에서 RDE에 배포하려면 다음 단계를 완료하십시오.

### Adobe I/O Runtime 확장 가능 CLI

명령줄에서 다음 명령을 실행하여 Adobe I/O Runtime Extensible CLI(`aio CLI`)를 설치합니다.

```shell
$ npm install -g @adobe/aio-cli
```

### aio CLI 플러그인 설치 및 설정

RDE와 상호 작용하려면 aio CLI에 플러그인이 설치되어 있고 조직, 프로그램 및 RDE 환경 ID로 설정되어 있어야 합니다. 보다 간단한 대화형 모드 또는 비대화형 모드를 사용하는 aio CLI를 통해 설정을 수행할 수 있습니다.

>[!BEGINTABS]

>[!TAB 대화형 모드]

`aio cli`의 `plugins:install` 명령을 사용하여 AEM RDE 플러그인을 설치하고 설정합니다.

1. `aio cli`의 `plugins:install` 명령을 사용하여 aio CLI의 AEM RDE 플러그인을 설치합니다.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   AEM RDE 플러그인을 사용하면 개발자가 로컬 컴퓨터에서 코드와 콘텐츠를 배포할 수 있습니다.

2. 다음 명령을 실행하여 Adobe I/O Runtime Extensible CLI에 로그인하여 액세스 토큰을 가져옵니다. Cloud Manager과 동일한 Adobe 조직에 로그인해야 합니다.

   ```shell
   $ aio login
   ```

3. 다음 명령을 실행하여 대화형 모드를 사용하여 RDE를 설정합니다.

   ```shell
   $ aio aem:rde:setup
   ```

4. CLI에서 조직 ID, 프로그램 ID 및 환경 ID를 입력하라는 메시지가 표시됩니다.

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - 단일 RDE로만 작업하고 로컬 컴퓨터에 RDE 구성을 전체적으로 저장하려면 __아니요__&#x200B;를 선택하세요.

   - 여러 RDE로 작업하거나 각 프로젝트에 대해 현재 폴더의 `.aio` 파일에 RDE 구성을 로컬로 저장하려면 __예__&#x200B;를 선택합니다.

5. 사용 가능한 옵션 목록에서 조직 ID, 프로그램 ID 및 RDE 환경 ID를 선택합니다.

6. 다음 명령을 실행하여 올바른 조직, 프로그램 및 환경이 설정되었는지 확인합니다.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB 비대화형 모드]

`aio cli`의 `plugins:install` 명령을 사용하여 Cloud Manager 및 AEM RDE 플러그인을 설치하고 설정합니다.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Cloud Manager 플러그인을 사용하면 개발자가 명령줄에서 Cloud Manager과 상호 작용할 수 있습니다.

AEM RDE 플러그인을 사용하면 개발자가 로컬 컴퓨터에서 코드와 콘텐츠를 배포할 수 있습니다.

RDE와 상호 작용하도록 aio CLI 플러그인을 구성해야 합니다.

1. 먼저 Cloud Manager을 사용하여 조직, 프로그램 및 환경 ID의 값을 복사합니다.

   - 조직 ID: **프로필 사진 > 계정 정보(내부) > 모달 창 > 현재 조직 ID**&#x200B;에서 값을 복사합니다.

   ![조직 ID](./assets/Org-ID.png)

   - 프로그램 ID: **프로그램 개요 > 환경 > {ProgramName}- > 브라우저 URI > `program/`에서`/environment`** 사이의 숫자에서 값을 복사합니다.

   ![프로그램 및 환경 ID](./assets/Program-Environment-Id.png)

   - 환경 ID: **프로그램 개요 > 환경 > {ProgramName}- > 브라우저 URI >`environment/`** 이후 숫자에서 값을 복사합니다.

   ![프로그램 및 환경 ID](./assets/Program-Environment-Id.png)

1. `aio cli`의 `config:set` 명령을 사용하여 다음 명령을 실행하여 이러한 값을 설정하십시오.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. 다음 명령을 실행하여 현재 구성 값을 확인합니다.

   ```shell
   $ aio config:list
   ```

1. 현재 로그인한 조직 전환 또는 검토:

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## RDE 액세스 확인

다음 명령을 실행하여 AEM RDE 플러그인 설치 및 구성을 확인합니다.

```shell
$ aio aem:rde:status
```

RDE 상태 정보는 환경 상태, 작성자 및 게시 서비스의 _AEM 프로젝트_ 번들 및 구성 목록과 같이 표시됩니다.

## 다음 단계

개발 주기를 단축하기 위해 즐겨 사용하는 IDE(통합 개발 환경)에서 코드와 콘텐츠를 배포하기 위해 RDE를 [사용하는 방법](./how-to-use.md)에 대해 알아봅니다.


## 추가 리소스

[프로그램 설명서에서 RDE 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=ko#enabling-rde-in-a-program)

`aio CLI`이라고도 하는 [Adobe I/O Runtime 확장 가능 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 설정

[aio CLI 사용 및 명령](https://github.com/adobe/aio-cli#usage)

[AEM 빠른 개발 환경과의 상호 작용을 위한 Adobe I/O Runtime CLI 플러그인](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager aio CLI 플러그인](https://github.com/adobe/aio-cli-plugin-cloudmanager)
