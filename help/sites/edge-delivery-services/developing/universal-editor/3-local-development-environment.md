---
title: 로컬 개발 환경 설정
description: Edge Delivery Services를 통해 제공되고 범용 편집기로 편집 가능한 사이트에 대한 로컬 개발 환경을 설정합니다.
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
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# 로컬 개발 환경 설정

Edge Delivery Services가 제공하는 웹 사이트를 빠르게 개발하려면 로컬 개발 환경이 필요합니다. 이 환경에서는 Edge Delivery Services에서 콘텐츠를 소싱하는 동시에 로컬에서 개발된 코드를 사용하므로 개발자는 코드 변경 사항을 즉시 볼 수 있습니다. 이러한 설정은 빠르고 반복적인 개발과 테스트를 지원합니다.

Edge Delivery Services 웹 사이트 프로젝트를 위한 개발 도구와 프로세스는 웹 개발자가 익숙하게 사용할 수 있도록 설계되었으며 빠르고 효율적인 개발 경험을 제공합니다.

## 개발 토폴로지

이 비디오는 범용 편집기로 편집할 수 있는 Edge Delivery Services 웹 사이트 프로젝트의 개발 토폴로지에 대한 개요를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3443985/?learn=on&enablevpops&captions=kor)

+++추가 개발 토폴로지 세부 정보 보기

- **GitHub 저장소**:
   - **목적**: 웹 사이트 코드(CSS 및 JavaScript)를 호스팅합니다.
   - **구조**: 다른 분기에는 실제 작동하는 코드가 포함되어 있는 반면, **주 분기**&#x200B;에는 제작 준비가 완료된 코드가 포함되어 있습니다.
   - **기능**: 모든 분기의 코드는 라이브 웹 사이트에 영향을 주지 않고 **프로덕션** 또는 **미리보기** 환경에서 실행할 수 있으며, 이때 라이브 웹 사이트에는 영향을 주지 않습니다.

- **AEM 작성자 서비스**:
   - **목적**: 웹 사이트 콘텐츠가 편집되고 관리되는 정식 콘텐츠 저장소 역할을 합니다.
   - **기능**: **범용 편집기**&#x200B;에서 콘텐츠를 읽고 작성하는 기능을 수행합니다. 편집된 콘텐츠는 **프로덕션** 또는 **미리보기** 환경에서 **Edge Delivery Services**&#x200B;에 게시됩니다.

- **범용 편집기**:
   - **목적**: 웹 사이트 콘텐츠를 편집하기 위한 WYSIWYG 인터페이스를 제공합니다.
   - **기능**: **AEM 작성자 서비스**&#x200B;에서 데이터를 읽고 쓰는 기능을 수행합니다. **GitHub 저장소** 내 모든 분기의 코드를 사용하도록 구성하여 변경 사항을 테스트하고 검증할 수 있습니다.

- **Edge Delivery Services**:
   - **프로덕션 환경**:
      - **목적**: 최종 사용자에게 라이브 웹 사이트 콘텐츠와 코드를 제공합니다.
      - **기능**: **GitHub 저장소** **주 분기**&#x200B;의 코드를 사용하여 **AEM 작성자 서비스**&#x200B;에서 게시된 콘텐츠를 제공합니다.
   - **미리보기 환경**:
      - **목적**: 배포 전에 콘텐츠와 코드를 테스트하고 미리 볼 수 있는 스테이징 환경을 제공합니다.
      - **기능**: **GitHub 저장소**&#x200B;의 모든 분기의 코드를 사용하여 **AEM 작성자 서비스**&#x200B;에서 게시된 콘텐츠를 제공합니다. 이렇게 하면 라이브 웹 사이트에 영향을 주지 않고 철저한 테스트를 실행할 수 있습니다.

- **로컬 개발자 환경**:
   - **목적**: 개발자가 로컬에서 코드(CSS 및 JavaScript)를 작성하고 테스트하도록 합니다.
   - **구조**:
      - 분기 기반 개발을 위한 **GitHub 저장소**&#x200B;의 로컬 복제.
      - **AEM CLI**&#x200B;는 개발 서버 역할을 하며, 로컬 코드 변경 사항을 **미리보기 환경**&#x200B;에 적용하여 핫 리로드 경험을 제공합니다.
   - **워크플로**: 개발자는 로컬에서 코드를 작성하고, 작업 분기에 변경 사항을 커밋한 다음 분기를 GitHub에 푸시합니다. 그런 다음 **범용 편집기**&#x200B;에서 지정된 분기를 사용하여 검증하고, 프로덕션 배포 준비가 완료되면 **주 분기**&#x200B;에 병합합니다.

+++

## 사전 요구 사항

개발을 시작하기 전에 다음을 컴퓨터에 설치해야 합니다.

1. [Git](https://git-scm.com/)
1. [Node.js 및 npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) 또는 유사한 코드 편집기

## GitHub 저장소 복제

AEM Edge Delivery Services 코드 프로젝트가 포함된 [새 코드 프로젝트 장에서 만든 GitHub 저장소](./1-new-code-project.md)를 로컬 개발 환경에 복제합니다.

![GitHub 저장소 복제](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

`Code` 디렉터리에 새 `aem-wknd-eds-ue` 폴더가 생성되고, 이 폴더가 프로젝트의 루트가 됩니다. 프로젝트는 컴퓨터의 어느 위치에나 복제할 수 있지만 이 튜토리얼에서는 `~/Code`를 루트 디렉터리로 사용합니다.

## 프로젝트 종속성 설치

프로젝트 폴더로 이동한 다음 `npm install`을 사용하여 필요한 종속성을 설치합니다. Edge Delivery Services 프로젝트는 Webpack이나 Vite와 같은 전통적인 Node.js 빌드 시스템을 사용하지 않지만 로컬 개발을 위해서는 몇 가지 종속성이 필요합니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM CLI 설치

AEM CLI는 Edge Delivery Services 기반 AEM 웹 사이트 개발을 간소화하도록 설계된 Node.js 명령줄 도구로, 웹 사이트를 빠르게 개발하고 테스트할 수 있는 로컬 개발 서버를 제공합니다.

AEM CLI를 설치하려면 다음을 실행하십시오.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI는 `npm install --global @adobe/aem-cli`를 사용하여 전역적으로 설치할 수도 있습니다.

## 로컬 AEM 개발 서버 시작

`aem up` 명령을 사용하면 로컬 개발 서버가 시작되고, 자동으로 브라우저 창이 열리며 서버 URL이 표시됩니다. 이 서버는 Edge Delivery Services 환경에 대한 리버스 프록시 역할을 하며, 해당 환경의 콘텐츠를 제공하면서도 로컬 코드 기반을 사용하여 개발할 수 있도록 합니다.

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

AEM CLI는 웹 사이트를 브라우저에서 `http://localhost:3000/`로 엽니다. 프로젝트의 변경 사항은 웹 브라우저에서 자동으로 핫 리로드되지만 콘텐츠를 변경하면 [미리보기 환경에 게시](./6-author-block.md)하고 웹 브라우저를 새로 고쳐야 합니다.

웹 사이트가 404 페이지로 열린다면 [새 코드 프로젝트](./1-new-code-project.md)에서 업데이트된 [fstab.yaml 또는 paths.json](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)이 잘못 구성되었거나, 변경 사항이 `main` 분기에 커밋되지 않았을 가능성이 높습니다.

## JSON 조각 빌드

[AEM 보일러플레이트 XWalk 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)을 사용하여 생성된 Edge Delivery Services 프로젝트는 범용 편집기에서 블록 작성을 활성화하는 JSON 구성을 사용합니다.

- **JSON 조각**: 연결된 블록과 함께 저장되며 블록 모델, 정의 및 필터를 정의합니다.
   - **모델 조각**: `/blocks/example/_example.json`에 저장됩니다.
   - **정의 조각**: `/blocks/example/_example.json`에 저장됩니다.
   - **필터 조각**: `/blocks/example/_example.json`에 저장됩니다.


[AEM 보일러플레이트 XWalk 프로젝트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)은 [Husky](https://typicode.github.io/husky/) pre-commit hook을 포함하고 있어, `git commit` 시 JSON 조각 변경 사항을 자동으로 감지하고 이를 적절한 `component-*.json` 파일을 자동 컴파일합니다.

다음 NPM 스크립트는 `npm run`으로 수동 실행하여 JSON 파일을 빌드할 수 있지만 일반적으로 Husky pre-commit hook이 자동으로 처리하므로 별도로 실행할 필요는 없습니다.

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM 스크립트 | 설명 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 모든 JSON 파일을 조각에서 빌드하고 적절한 `component-*.json` 파일에 추가합니다. |
| `build:json:models` | 블록 JSON 조각을 빌드하고 이를 `/component-models.json`으로 컴파일합니다. |
| `build:json:definitions` | 페이지 JSON 조각을 빌드하고 이를 `/component-definitions.json`으로 컴파일합니다. |
| `build:json:filters` | 페이지 JSON 조각을 빌드하고 이를 `/component-filters.json`으로 컴파일합니다. |

>[!TIP]
>
> 조각 파일을 변경한 후에 `npm run build:json`을 실행하여 기본 JSON 파일을 다시 생성합니다.

## 린팅

린팅은 `main` 분기에 변경 사항을 병합하기 전에 Edge Delivery Services 프로젝트에 필요한 코드 품질과 일관성을 보장합니다.

NPM 스크립트는 `npm run`을 통해 실행할 수 있으며, 그 예는 다음과 같습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM 스크립트 | 설명 |
|------------------|--------------------------------------------------|
| `lint:js` | JavaScript 린팅 검사를 실행합니다. |
| `lint:css` | CSS 린팅 검사를 실행합니다. |
| `lint` | JavaScript 및 CSS 린팅 검사를 모두 실행합니다. |

### 린팅 문제 자동 수정

다음 `scripts`를 프로젝트의 `package.json`에 추가하면 린팅 문제를 자동으로 해결할 수 있으며, `npm run`을 통해 실행할 수 있습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

이러한 스크립트는 AEM 보일러플레이트 XWalk 템플릿에 미리 구성되어 있지 않지만 `package.json` 파일에 추가할 수 있습니다.

>[!BEGINTABS]

>[!TAB 추가 스크립트]

| NPM 스크립트 | 명령 | 설명 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | JavaScript 린팅 문제를 자동으로 수정합니다. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | CSS 린팅 문제를 자동으로 해결합니다. |
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
