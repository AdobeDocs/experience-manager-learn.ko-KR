---
title: 로컬 개발 환경 설정
description: Edge Delivery Services으로 제공되고 유니버설 편집기로 편집할 수 있는 사이트에 대한 로컬 개발 환경을 설정합니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 1%

---

# 로컬 개발 환경 설정

Edge Delivery Services에서 제공하는 웹 사이트를 빠르게 개발하려면 로컬 개발 환경이 필수적입니다. 이 환경에서는 Edge Delivery Services에서 콘텐츠를 소싱하면서 로컬로 개발된 코드를 사용하므로 개발자가 코드 변경 사항을 즉시 확인할 수 있습니다. 이러한 설정은 빠르고 반복적인 개발 및 테스트를 지원합니다.

Edge Delivery Services 웹 사이트 프로젝트의 개발 도구 및 프로세스는 웹 개발자에게 친숙하고 빠르고 효율적인 개발 경험을 제공하도록 설계되었습니다.

## 개발 토폴로지

이 비디오는 범용 편집기로 편집할 수 있는 Edge Delivery Services 웹 사이트 프로젝트의 개발 토폴로지에 대한 개요를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3443978/?learn=on&enablevpops)

+++추가 개발 토폴로지 세부 정보 참조

- **GitHub 저장소**:
   - **목적**: 웹 사이트의 코드(CSS 및 JavaScript)를 호스팅합니다.
   - **구조**: **주 분기**&#x200B;에 프로덕션 준비 코드가 있고 다른 분기에는 작업 코드가 있습니다.
   - **기능**: 모든 분기의 코드는 라이브 웹 사이트에 영향을 주지 않고 **프로덕션** 또는 **미리 보기** 환경에서 실행할 수 있습니다.

- **AEM 작성자 서비스**:
   - **목적**: 웹 사이트 콘텐츠를 편집하고 관리하는 표준 콘텐츠 리포지토리 역할을 합니다.
   - **기능**: **유니버설 편집기**&#x200B;에서 콘텐츠를 읽고 씁니다. 편집된 콘텐츠가 **프로덕션** 또는 **미리보기** 환경의 **Edge Delivery Services**&#x200B;에 게시됩니다.

- **유니버설 편집기**:
   - **목적**: 웹 사이트 콘텐츠를 편집하기 위한 WYSIWYG 인터페이스를 제공합니다.
   - **기능**: **AEM 작성자 서비스**&#x200B;에서 읽고 씁니다. **GitHub 저장소**&#x200B;의 모든 분기의 코드를 사용하여 변경 내용을 테스트하고 유효성을 검사하도록 구성할 수 있습니다.

- **Edge Delivery Services**:
   - **프로덕션 환경**:
      - **목적**: 라이브 웹 사이트 콘텐츠 및 코드를 최종 사용자에게 전달합니다.
      - **기능**: **GitHub 저장소**&#x200B;의 **주 분기**&#x200B;에서 가져온 코드를 사용하여 **AEM 작성자 서비스**&#x200B;에서 게시한 콘텐츠를 제공합니다.
   - **환경 미리 보기**:
      - **목적**: 배포 전에 콘텐츠 및 코드를 테스트하고 미리 볼 수 있는 스테이징 환경을 제공합니다.
      - **기능**: **AEM 작성자 서비스**&#x200B;에서 게시된 콘텐츠를 **GitHub 저장소**&#x200B;의 모든 분기에서 코드를 사용하여 제공하므로 실시간 웹 사이트에 영향을 주지 않고 철저한 테스트를 수행할 수 있습니다.

- **로컬 개발자 환경**:
   - **목적**: 개발자가 로컬에서 코드(CSS 및 JavaScript)를 작성하고 테스트할 수 있도록 허용합니다.
   - **구조**:
      - 분기 기반 개발을 위한 **GitHub 저장소**&#x200B;의 로컬 복제입니다.
      - 개발 서버 역할을 하는 **AEM CLI**&#x200B;는 핫리로드 환경을 위해 로컬 코드 변경 사항을 **미리 보기 환경**&#x200B;에 적용합니다.
   - **워크플로**: 개발자는 로컬에서 코드를 작성하고, 작업 분기에 변경 내용을 커밋하고, 분기를 GitHub로 푸시하고, **유니버설 편집기**&#x200B;에서 유효성을 검사하고(지정된 분기 사용), 프로덕션 배포가 준비되면 **주 분기**&#x200B;에 병합합니다.

+++

## 사전 요구 사항

개발을 시작하기 전에 시스템에 다음을 설치하십시오.

1. [Git](https://git-scm.com/)
1. [Node.js 및 npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/)&#x200B;(또는 유사한 코드 편집기)

## GitHub 리포지토리 복제

AEM Edge Delivery Services 코드 프로젝트를 포함하는 새 코드 프로젝트 챕터[&#128279;](./1-new-code-project.md)에서 만든 GitHub 리포지토리를 로컬 개발 환경에 복제합니다.

![GitHub 리포지토리 복제](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

프로젝트의 루트 역할을 하는 `Code` 디렉터리에 새 `aem-wknd-eds-ue` 폴더가 만들어집니다. 컴퓨터의 모든 위치에 프로젝트를 복제할 수 있지만 이 자습서에서는 `~/Code`을(를) 루트 디렉터리로 사용합니다.

## 프로젝트 종속성 설치

프로젝트 폴더로 이동하여 `npm install`과(와) 함께 필요한 종속성을 설치하십시오. Edge Delivery Services 프로젝트에서는 Webpack 또는 Vite와 같은 기존 Node.js 빌드 시스템을 사용하지 않지만 로컬 개발을 위해서는 여전히 몇 가지 종속성이 필요합니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM CLI 설치

AEM CLI는 Edge Delivery Services 기반 AEM 웹 사이트의 개발을 간소화하도록 설계된 Node.js 명령줄 도구로서, 웹 사이트의 신속한 개발 및 테스트를 위한 로컬 개발 서버를 제공합니다.

AEM CLI를 설치하려면 다음을 실행합니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI는 `npm install --global @adobe/aem-cli`을(를) 사용하여 전체적으로 설치할 수도 있습니다.

## 로컬 AEM 개발 서버 시작

`aem up` 명령은 로컬 개발 서버를 시작하고 서버의 URL에 대한 브라우저 창을 자동으로 엽니다. 이 서버는 Edge Delivery Services 환경에 대한 역방향 프록시 역할을 하며 개발을 위해 로컬 코드 베이스를 사용하는 동안 콘텐츠를 제공합니다.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

AEM CLI가 브라우저에서 웹 사이트(`http://localhost:3000/`)를 엽니다. 프로젝트의 변경 내용이 웹 브라우저에서 자동으로 핫 로드되는 반면, 콘텐츠 변경 내용 [을(를) 미리 보기 환경에 게시하고 웹 브라우저를 새로 고쳐야 합니다](./6-author-block.md).

웹 사이트가 404 페이지로 열리는 경우 [새 코드 프로젝트](./1-new-code-project.md)에서 업데이트된 [fstab.yaml 또는 paths.json](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)이 잘못 구성되었거나 변경 내용이 `main` 분기에 커밋되지 않았을 수 있습니다.

## JSON 조각 작성

[AEM Boilerplate XWalk 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)을 사용하여 만든 Edge Delivery Services 프로젝트는 범용 편집기에서 블록 작성을 활성화하는 JSON 구성을 사용합니다.

- **JSON 조각**: 연결된 블록과 함께 저장되고 블록 모델, 정의 및 필터를 정의합니다.
   - **모델 조각**: `/blocks/example/_example.json`에 저장됨.
   - **정의 조각**: `/blocks/example/_example.json`에 저장됨.
   - **필터 조각**: `/blocks/example/_example.json`에 저장됨.


[AEM Boilerplate XWalk 프로젝트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)에는 JSON 조각의 변경 내용을 감지하고 `git commit`에 해당 `component-*.json` 파일로 컴파일하는 [Husky](https://typicode.github.io/husky/) 사전 커밋 후크가 포함되어 있습니다.

다음 NPM 스크립트는 `npm run`을(를) 통해 수동으로 실행하여 JSON 파일을 작성할 수 있지만, 허스키 사전 커밋 후크가 자동으로 처리하므로 일반적으로 필요하지 않습니다.

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM 스크립트 | 설명 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 조각에서 모든 JSON 파일을 빌드하고 해당 `component-*.json` 파일에 추가합니다. |
| `build:json:models` | 빌드는 JSON 조각을 차단하여 `/component-models.json`(으)로 컴파일합니다. |
| `build:json:definitions` | 페이지 JSON 조각을 빌드하고 `/component-definitions.json`(으)로 컴파일합니다. |
| `build:json:filters` | 페이지 JSON 조각을 빌드하고 `/component-filters.json`(으)로 컴파일합니다. |

>[!TIP]
>
> 조각 파일을 변경한 후 `npm run build:json`을(를) 실행하여 기본 JSON 파일을 다시 생성합니다.

## 린팅

링크를 사용하면 변경 내용을 `main` 분기에 병합하기 전에 Edge Delivery Services 프로젝트에 필요한 코드 품질과 일관성을 유지할 수 있습니다.

NPM 스크립트는 `npm run`을(를) 통해 실행할 수 있습니다. 예:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM 스크립트 | 설명 |
|------------------|--------------------------------------------------|
| `lint:js` | JavaScript 린팅 검사를 실행합니다. |
| `lint:css` | CSS 린팅 검사를 실행합니다. |
| `lint` | JavaScript 및 CSS 린팅 검사를 모두 실행합니다. |

### 자동으로 린팅 문제 해결

프로젝트의 `package.json`에 다음 `scripts`을(를) 추가하여 린팅 문제를 자동으로 해결할 수 있으며 `npm run`을(를) 통해 실행할 수 있습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

이러한 스크립트는 AEM Boilerplate XWalk 템플릿으로 미리 구성된 것이 아니라 `package.json` 파일에 추가할 수 있습니다.

>[!BEGINTABS]

>[!TAB 추가 스크립트]

| NPM 스크립트 | 명령 | 설명 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | JavaScript 린팅 문제를 자동으로 수정합니다. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | CSS 연결 문제를 자동으로 수정합니다. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 빠른 정리를 위해 JS 및 CSS 수정 스크립트를 모두 실행합니다. |

>[!TAB package.json 예]

다음 스크립트 항목을 `package.json` `scripts` 배열에 추가할 수 있습니다.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
