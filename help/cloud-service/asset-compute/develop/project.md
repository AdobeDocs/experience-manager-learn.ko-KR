---
title: asset compute 확장성을 위한 Asset compute 프로젝트 만들기
description: asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합할 수 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# asset compute 프로젝트 만들기

asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합됩니다. 단일 Asset compute 프로젝트에는 하나 이상의 Asset compute 작업자가 포함될 수 있으며 각는 AEM에서 Cloud Service 처리 프로필로 분리된 HTTP 종단점을 참조할 수 있습니다.

## 프로젝트 생성

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_asset compute 프로젝트 생성 클릭스루(오디오 없음)_


[Adobe I/O CLI Asset compute 플러그인](../set-up/development-environment.md#aio-cli)을 사용하여 비어 있는 새 Asset compute 프로젝트를 생성합니다.

1. 명령줄에서 프로젝트를 포함할 폴더로 이동합니다.
1. 명령줄에서 `aio app init`을 실행하여 대화형 프로젝트 생성 CLI를 시작합니다.
   + Adobe I/O에 대한 인증을 요청하는 웹 브라우저가 생길 수 있습니다.이 경우 [필수 Adobe 서비스 및 제품](../set-up/accounts-and-services.md)과 연결된 Adobe 자격 증명을 제공합니다. 로그인할 수 없는 경우 [프로젝트](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)를 생성하는 방법에 대한 지침을 따르십시오.
1. __조직 선택__
   + AEM이 있는 Adobe 조직, Project Firefly가
1. __프로젝트 선택__
   + 프로젝트를 찾아 선택합니다. Firefly 프로젝트 템플릿에서 만든 [프로젝트 제목](../set-up/firefly.md)입니다. 이 경우 `WKND AEM Asset Compute`
1. __작업 공간 선택__
   + `Development` 작업 영역을 선택합니다.
1. __이 프로젝트에 어떤 Adobe I/O 앱 기능을 활성화하시겠습니까? 포함할 구성 요소 선택__
   + 선택 `Actions: Deploy runtime actions`
   + 화살표 키를 사용하여 선택 및 선택/선택을 취소할 공간, Enter 키를 사용하여 선택 확인
1. __생성할 작업 유형 선택__
   + 선택 `Adobe Asset Compute Worker`
   + 화살표 키를 사용하여 선택, 선택/선택을 취소할 공간, Enter 키를 사용하여 선택 확인
1. __이 작업의 이름을 어떻게 지정하시겠습니까?__
   + 기본 이름 `worker`을 사용하십시오.
   + 프로젝트에 서로 다른 에셋 계산을 수행하는 여러 명의 작업자가 포함되어 있는 경우 중간 이름을 지정합니다

## console.json 생성

새로 만든 Asset compute 프로젝트의 루트에서 다음 명령을 실행하여 `console.json`을(를) 생성합니다.

```
$ aio app use
```

현재 작업 공간 세부 정보가 올바른지 확인하고 `Y`을(를) 수정하거나 `console.json`을(를) 생성합니다. `.env` 및 `.aio`이(가) 이미 존재하는 것으로 감지되면 `x`를 눌러 만들기를 건너뜁니다.

## 그 프로젝트의 구조 검토

생성된 Asset compute 프로젝트는 특수 Adobe 프로젝트 Firefly 프로젝트에 대한 Node.js 프로젝트이며, 다음은 Asset compute 프로젝트와 별개입니다.

+ `/actions` 하위 폴더가 들어 있고 각 하위 폴더는 Asset compute 작업자를 정의합니다.
   + `/actions/<worker-name>/index.js` 이 작업자의 작업을 수행하기 위해 실행되는 JavaScript를 정의합니다.
      + 폴더 이름 `worker`은 기본값이며 `manifest.yml`에 등록되어 있는 한 모든 것이 될 수 있습니다.
      + 필요에 따라 `/actions` 아래에 둘 이상의 작업자 폴더를 정의할 수 있지만 `manifest.yml`에 등록해야 합니다.
+ `/test/asset-compute` 은 각 작업자에 대한 테스트 세트를 포함합니다. `/actions` 폴더와 유사하게 `/test/asset-compute`에는 여러 하위 폴더가 포함될 수 있으며 각 하위 폴더가 테스트 중인 작업자에 해당합니다.
   + `/test/asset-compute/worker`특정 작업자에 대한 테스트 세트를 나타내는 에는 테스트 입력, 매개 변수 및 예상 출력과 함께 특정 테스트 케이스를 나타내는 하위 폴더가 포함됩니다.
+ `/build` 은 Asset compute 테스트 케이스 실행 결과, 로그 및 객체를 포함합니다.
+ `/manifest.yml` 프로젝트에서 제공하는 Asset compute 근로자를 정의합니다. AEM에서 Cloud Service으로 사용할 수 있도록 하려면 각 작업자 구현이 이 이 파일에 열거되어야 합니다.
+ `/console.json` adobe i/o 구성 정의
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
+ `/.aio` 에는 aio CLI 도구에서 사용하는 구성이 들어 있습니다.
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
+ `/.env` 구문에 환경 변수를 정의하고  `key=value` 공유할 수 없는 비밀을 포함합니다. 이 파일을 생성하거나 이러한 비밀을 보호하려면 이 파일을 Git에 체크 인하지 않아야 하며 프로젝트의 기본 `.gitignore` 파일을 통해 무시됩니다.
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
   + 이 파일에 정의된 변수는 명령줄에서 [내보내기 변수](../deploy/runtime.md)로 재정의할 수 있습니다.

프로젝트 구조 검토에 대한 자세한 내용은 Adobe 프로젝트 Firefly 프로젝트의 구조[를 검토하십시오.](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)

대부분의 개발 작업은 작업자 구현을 개발하는 `/actions` 폴더에서, 사용자 지정 Asset compute 작업자에 대한 테스트를 `/test/asset-compute` 쓰기 폴더에서 발생합니다.

## Github의 asset compute 프로젝트

최종 Asset compute 프로젝트는 다음 위치의 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains is the final state of the project, fully built with the worker and test cases, but does not contain any credentials, ie. `.env`,  `console.json` or  `.aio`._

