---
title: asset compute 확장성을 위한 Asset compute 프로젝트 만들기
description: asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합할 수 있습니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 1%

---


# asset compute 프로젝트 만들기

asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 Cloud Service으로 통합할 수 있습니다. 단일 Asset compute 프로젝트에는 하나 이상의 Asset compute 작업자가 포함될 수 있으며 각 작업자는 AEM에서 Cloud Service 처리 프로필로 참조할 수 있는 개별 HTTP 종단점을 갖습니다.

## 프로젝트 생성

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_asset compute 프로젝트를 생성하는 클릭스루(오디오 없음)_

[Adobe I/O CLI Asset compute 플러그인](../set-up/development-environment.md#aio-cli)을 사용하여 비어 있는 새 Asset compute 프로젝트를 생성합니다.

1. 명령줄에서 프로젝트를 포함할 폴더로 이동합니다.
1. 명령줄에서 `aio app init`을(를) 실행하여 대화형 프로젝트 생성 CLI를 시작합니다.
   + 이 명령은 Adobe I/O 인증을 요청하는 웹 브라우저를 만들 수 있습니다.이 경우 [필수 Adobe 서비스 및 제품](../set-up/accounts-and-services.md)과 연관된 Adobe 자격 증명을 제공합니다. 로그인할 수 없는 경우 [프로젝트](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)를 생성하는 방법에 대한 다음 지침을 따르십시오.
1. __조직 선택__
   + AEM as a Cloud Service, Project Firefly 가 등록된 Adobe 조직을 선택합니다
1. __프로젝트 선택__
   + 프로젝트를 찾아 선택합니다. Firefly 프로젝트 템플릿에서 만든 [프로젝트 제목](../set-up/firefly.md)입니다(이 경우 `WKND AEM Asset Compute`)
1. __작업 공간 선택__
   + `Development` 작업 공간을 선택합니다
1. __이 프로젝트에 대해 어떤 Adobe I/O 앱 기능을 활성화하시겠습니까? 포함할 구성 요소 선택__
   + 선택 `Actions: Deploy runtime actions`
   + 화살표 키를 사용하여 선택/선택 취소할 및 공백을 선택하고 Enter 키를 사용하여 선택을 확인합니다
1. __생성할 작업 유형 선택__
   + 선택 `Adobe Asset Compute Worker`
   + 화살표 키를 사용하여 선택하고, 스페이스를 사용하여 선택/선택을 취소하고, Enter 키를 사용하여 선택을 확인합니다
1. __이 작업의 이름을 어떻게 지정하시겠습니까?__
   + 기본 이름 `worker`을 사용하십시오.
   + 프로젝트에 서로 다른 자산 계산을 수행하는 여러 작업자가 포함되어 있는 경우 의미상 이름을 지정합니다

## console.json 생성

개발자 도구에는 Adobe I/O에 연결하는 데 필요한 자격 증명이 포함된 `console.json` 파일이 필요합니다.이 파일은 Adobe I/O 콘솔에서 다운로드됩니다.

1. asset compute 작업자의 [Adobe I/O](https://console.adobe.io) 프로젝트를 엽니다.
1. 프로젝트 작업 영역을 선택하여 `console.json` 자격 증명을 다운로드합니다. 이 경우 `Development` 를 선택합니다
1. Adobe I/O 프로젝트의 루트로 이동하고 오른쪽 위 모서리에서 __모두 다운로드__&#x200B;를 누릅니다.
1. 파일은 프로젝트 및 작업 공간 접두사가 있는 `.json` 파일로 다운로드됩니다. 예:`wkndAemAssetCompute-81368-Development.json`
1. 다음 중 하나를 수행할 수 있습니다.
   + 파일 이름을 `config.json`로 변경하고 Asset compute 작업자 프로젝트의 루트에서 이동합니다. 이 자습서의 접근 방식입니다.
   + 구성 항목이 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`인 `.env` 파일에서 해당 폴더를 임의의 폴더로 이동하고 참조합니다. 파일 경로는 프로젝트의 루트에 대해 절대 또는 상대적일 수 있습니다. 예:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      또는
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 메모
> 파일에 자격 증명이 포함되어 있습니다. 프로젝트 내에 파일을 저장하는 경우 파일을 `.gitignore` 파일에 추가하여 공유되지 않도록 해야 합니다. 동일한 내용이 `.env` 파일에 적용됩니다. 이러한 자격 증명 파일은 공유하거나 Git에 저장해서는 안 됩니다.

## 그 프로젝트의 구조 검토

생성된 Asset compute 프로젝트는 전문 Adobe Project Firefly 프로젝트로 사용하기 위한 Node.js 프로젝트입니다. 다음 구조적 요소는 Asset compute 프로젝트에 특이적으로 사용됩니다.

+ `/actions` 하위 폴더를 포함하고 각 하위 폴더는 Asset compute 작업자를 정의합니다.
   + `/actions/<worker-name>/index.js` 이 작업자의 작업을 수행하는 데 사용되는 JavaScript를 정의합니다.
      + 폴더 이름 `worker`은(는) 기본값이며 `manifest.yml`에 등록된 한 어떤 것이든 될 수 있습니다.
      + 필요에 따라 `/actions` 아래에 둘 이상의 작업자 폴더를 정의할 수 있지만 `manifest.yml`에 등록해야 합니다.
+ `/test/asset-compute` 에는 각 작업자에 대한 테스트 세트가 포함되어 있습니다. `/actions` 폴더와 유사한 `/test/asset-compute` 에는 테스트할 작업자에 해당하는 여러 하위 폴더가 포함될 수 있습니다.
   + `/test/asset-compute/worker`특정 작업자에 대한 테스트 세트를 나타내는 에는 테스트 입력, 매개 변수 및 예상 출력과 함께 특정 테스트 사례를 나타내는 하위 폴더가 포함되어 있습니다.
+ `/build` asset compute 테스트 사례 실행의 출력, 로그 및 객체를 포함합니다.
+ `/manifest.yml` 프로젝트에서 제공하는 Asset compute 작업자를 정의합니다. AEM에서 Cloud Service으로 사용할 수 있도록 하려면 각 작업자 구현을 이 파일에 열거해야 합니다.
+ `/console.json` Adobe I/O 구성 정의
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
+ `/.aio` aio CLI 툴에서 사용하는 구성을 포함합니다.
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
+ `/.env` 구문으로 환경 변수 `key=value` 를 정의하고 공유하지 않아야 하는 암호를 포함합니다. 이러한 비밀을 보호하려면 이 파일을 Git에 체크 인하지 않아야 하며 프로젝트의 기본 `.gitignore` 파일을 통해 무시됩니다.
   + 이 파일은 `aio app use` 명령을 사용하여 생성/업데이트할 수 있습니다.
   + 이 파일에 정의된 변수는 명령줄에서 [변수 내보내기](../deploy/runtime.md)로 대체할 수 있습니다.

프로젝트 구조 검토에 대한 자세한 내용은 [Adobe Project Firefly 프로젝트의 구조](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)를 검토하십시오.

대부분의 개발 작업은 작업자 구현을 개발하는 `/actions` 폴더와 사용자 지정 Asset compute 작업자에 대한 테스트 쓰기에서 수행됩니다.`/test/asset-compute`

## GitHub에서 프로젝트 asset compute

최종 Asset compute 프로젝트는 GitHub에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub에는 프로젝트의 최종 상태가 포함되어 있으며, 작업자 및 테스트 사례로 완전히 채워지지만, 자격 증명( `.env`,  `console.json` 또는  `.aio`)은 포함하지 않습니다._

