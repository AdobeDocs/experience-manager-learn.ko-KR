---
title: 자산 계산 확장성을 위한 자산 계산 프로젝트 만들기
description: 자산 계산 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합할 수 있습니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---


# 자산 계산 프로젝트 만들기

자산 계산 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합할 수 있습니다. 단일 자산 계산 프로젝트에는 하나 이상의 자산 계산 작업자가 포함될 수 있으며, 여기에는 AEM에서 Cloud Service 처리 프로필로 참조할 수 있는 개별 HTTP 끝점이 있습니다.

## 프로젝트 생성

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)
_에셋 계산 프로젝트(오디오 없음)를 생성하는 클릭스루_


Adobe [I/O CLI Asset Compute 플러그인을](../set-up/development-environment.md#aio-cli) 사용하여 비어 있는 Asset Compute 프로젝트를 새로 생성합니다.

1. 명령줄에서 프로젝트를 포함할 폴더로 이동합니다.
1. 명령줄에서 실행하여 대화형 프로젝트 생성 CLI `aio app init` 를 시작합니다.
   + Adobe I/O에 대한 인증을 요청하는 웹 브라우저가 생길 수 있습니다.그런 경우 [필요한 Adobe 서비스 및 제품과 관련된 Adobe 자격 증명을 제공합니다](../set-up/accounts-and-services.md). 로그인할 수 없는 경우 프로젝트를 생성하는 방법에 대한 지침을 따르십시오.
1. __조직 선택__
   + AEM이 있는 Adobe 조직, Project Firefly가
1. __프로젝트 선택__
   + 프로젝트를 찾아 선택합니다. Firefly [프로젝트 템플릿에서 만든 프로젝트 제목입니다](../set-up/firefly.md) . 이 경우 `WKND AEM Asset Compute`
1. __작업 공간 선택__
   + 작업 공간 `Development` 선택
1. __이 프로젝트에 어떤 Adobe I/O 앱 기능을 활성화하시겠습니까? 포함할 구성 요소 선택__
   + 선택 `Actions: Deploy runtime actions`
   + 화살표 키를 사용하여 선택 및 선택/선택을 취소할 공간, Enter 키를 사용하여 선택 확인
1. __생성할 작업 유형 선택__
   + 선택 `Adobe Asset Compute Worker`
   + 화살표 키를 사용하여 선택, 선택/선택을 취소할 공간, Enter 키를 사용하여 선택 확인
1. __이 작업의 이름을 어떻게 지정하시겠습니까?__
   + 기본 이름을 사용합니다 `worker`.
   + 프로젝트에 서로 다른 에셋 계산을 수행하는 여러 명의 작업자가 포함되어 있는 경우 중간 이름을 지정합니다

## 그 프로젝트의 구조 검토

생성된 자산 계산 프로젝트는 전문화된 Adobe 프로젝트 Firefly 프로젝트에 대한 Node.js 프로젝트이며, 다음은 자산 계산 프로젝트와 별개입니다.

+ `/actions` 하위 폴더가 들어 있고 각 하위 폴더는 자산 계산 작업자를 정의합니다.
   + `/actions/<worker-name>/index.js` 이 작업자의 작업을 수행하기 위해 실행되는 JavaScript를 정의합니다.
      + 폴더 이름 `worker` 은 기본값이며, 폴더 이름에 등록된 한 모든 것이 될 수 있습니다 `manifest.yml`.
      + 필요에 따라 두 개 이상의 작업자 폴더를 정의할 수 `/actions` 있지만, 폴더에 등록해야 합니다 `manifest.yml`.
+ `/test/asset-compute` 은 각 작업자에 대한 테스트 세트를 포함합니다. 폴더와 유사하게 여러 하위 폴더가 포함될 `/actions` `/test/asset-compute` 수 있으며 각 하위 폴더는 테스트 중인 작업자에 해당합니다.
   + `/test/asset-compute/worker`특정 작업자에 대한 테스트 세트를 나타내는 에는 테스트 입력, 매개 변수 및 예상 출력과 함께 특정 테스트 케이스를 나타내는 하위 폴더가 포함됩니다.
+ `/build` 에는 자산 계산 테스트 케이스 실행의 출력, 로그 및 가공물이 포함됩니다.
+ `/manifest.yml` 프로젝트에서 제공하는 자산 계산 작업자를 정의합니다. AEM에서 Cloud Service으로 사용할 수 있도록 하려면 각 작업자 구현이 이 이 파일에 열거되어야 합니다.
+ `/.aio` 에는 aio CLI 도구에서 사용하는 구성이 들어 있습니다. 이 파일은 `aio config` 명령을 통해 구성할 수 있습니다.
+ `/.env` 구문에 환경 변수를 정의하고 공유할 수 없는 비밀을 `key=value` 포함합니다. 이러한 비밀을 보호하려면 이 파일을 Git로 체크 인하지 않아야 하며 프로젝트의 기본 `.gitignore` 파일을 통해 무시됩니다.
   + 이 파일에 정의된 변수는 명령줄에서 변수를 [내보내어](../deploy/runtime.md) 재정의할 수 있습니다.

프로젝트 구조 검토에 대한 자세한 내용은 Firefly Adobe 프로젝트 [구조 프로젝트를 검토하십시오](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

대부분의 개발 작업은 작업자 구현을 개발하는 `/actions` 폴더에서 발생하고 사용자 지정 자산 계산 작업자에 대한 테스트를 `/test/asset-compute` 쓰는 방식으로 이루어집니다.

## Github에서 자산 계산 프로젝트

최종 자산 계산 프로젝트는 다음 위치의 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains is the final state of the project, fully built with the worker and test cases, but does not contain any credentials, ie.`.env`,`.config.json`or`.aio`._