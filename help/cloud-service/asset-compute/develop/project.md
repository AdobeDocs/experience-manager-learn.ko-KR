---
title: asset compute 확장성을 위한 Asset compute 프로젝트 만들기
description: asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM as a Cloud Service과 통합할 수 있습니다.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---

# asset compute 프로젝트 만들기

asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM과 통합될 수 있습니다. 단일 Asset compute 프로젝트에는 하나 이상의 Asset compute 작업자가 포함될 수 있으며 각 작업자는 AEM as a Cloud Service 처리 프로필에서 참조할 수 있는 개별 HTTP 종단점을 갖습니다.

## 프로젝트 생성

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_asset compute 프로젝트를 생성하는 클릭스루(오디오 없음)_

를 사용하십시오 [Adobe I/O CLI Asset compute 플러그인](../set-up/development-environment.md#aio-cli) 비어 있는 새 Asset compute 프로젝트를 생성합니다.

1. 명령줄에서 프로젝트를 포함할 폴더로 이동합니다.
1. 명령줄에서 를 실행합니다 `aio app init` 대화식 프로젝트 생성 CLI를 시작합니다.
   + 이 명령은 Adobe I/O 인증을 요청하는 웹 브라우저를 만들 수 있습니다. 이 경우 와 연결된 Adobe 자격 증명을 제공합니다. [필수 Adobe 서비스 및 제품](../set-up/accounts-and-services.md). 로그인할 수 없으면 다음을 수행하십시오 [프로젝트를 생성하는 방법에 대한 다음 지침](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __조직 선택__
   + AEM as a Cloud Service, App Builder가 등록된 Adobe 조직을 선택합니다
1. __프로젝트 선택__
   + 프로젝트를 찾아 선택합니다. 이것은 [프로젝트 제목](../set-up/app-builder.md) 앱 빌더 프로젝트 템플릿에서 만든(이 경우) `WKND AEM Asset Compute`
1. __작업 공간 선택__
   + 을(를) 선택합니다 `Development` 작업 영역
1. __이 프로젝트에 대해 어떤 Adobe I/O 앱 기능을 활성화하시겠습니까? 포함할 구성 요소 선택__
   + 선택 `Actions: Deploy runtime actions`
   + 화살표 키를 사용하여 선택/선택 취소할 및 공백을 선택하고 Enter 키를 사용하여 선택을 확인합니다
1. __생성할 작업 유형 선택__
   + 선택 `DX Asset Compute Worker v1`
   + 화살표 키를 사용하여 선택하고, 스페이스를 사용하여 선택/선택을 취소하고, Enter 키를 사용하여 선택을 확인합니다
1. __이 작업의 이름을 어떻게 지정하시겠습니까?__
   + 기본 이름을 사용합니다 `worker`.
   + 프로젝트에 서로 다른 자산 계산을 수행하는 여러 작업자가 포함되어 있는 경우 의미상 이름을 지정합니다

## console.json 생성

개발자 도구에는 이름이 인 파일이 필요합니다. `console.json` 에는 Adobe I/O에 연결하는 데 필요한 자격 증명이 포함되어 있습니다. 이 파일은 Adobe I/O 콘솔에서 다운로드됩니다.

1. asset compute 작업자의 [Adobe I/O](https://console.adobe.io) 프로젝트
1. 다운로드할 프로젝트 작업 영역을 선택합니다 `console.json` 에 대한 자격 증명. 이 경우 `Development`
1. Adobe I/O 프로젝트의 루트로 이동하여 를 탭합니다 __모두 다운로드__ 오른쪽 상단 모서리에서
1. 파일은 로 다운로드됩니다. `.json` 프로젝트 및 작업 공간 접두사가 있는 파일(예: `wkndAemAssetCompute-81368-Development.json`
1. 다음 중 하나를 수행할 수 있습니다
   + 파일 이름을 다음으로 변경합니다. `console.json` asset compute 작업자 프로젝트의 루트로 이동합니다. 이 자습서의 접근 방식입니다.
   + 임의의 폴더로 이동하고 `.env` 구성 항목이 있는 파일 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. 파일 경로는 프로젝트의 루트에 대해 절대 또는 상대적일 수 있습니다. 예:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      또는
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 메모
> 파일에 자격 증명이 포함되어 있습니다. 프로젝트 내에 파일을 저장하는 경우 파일에 추가해야 합니다 `.gitignore` 파일이 공유되지 않도록 합니다. 에도 마찬가지입니다. `.env` 파일 — 이러한 자격 증명 파일은 공유하거나 Git에 저장해서는 안 됩니다.

## GitHub에서 프로젝트 asset compute

최종 Asset compute 프로젝트는 GitHub에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub에는 프로젝트의 최종 상태가 포함되어 있으며, 작업자 및 테스트 사례로 완전히 채워지지만, 자격 증명(즉, `.env`, `console.json` 또는 `.aio`._
