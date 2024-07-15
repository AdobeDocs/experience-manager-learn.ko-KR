---
title: asset compute 확장성을 위한 Asset compute 프로젝트 만들기
description: Asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, 특정 구조를 준수하여 Adobe I/O Runtime에 배포하고 AEM as a Cloud Service과 통합할 수 있습니다.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# asset compute 프로젝트 만들기

Asset compute 프로젝트는 Adobe I/O CLI를 사용하여 생성된 Node.js 프로젝트로서, Adobe I/O Runtime에 배포하고 AEM as a Cloud Service과 통합할 수 있도록 하는 특정 구조를 준수합니다. 단일 Asset compute 프로젝트에는 하나 이상의 Asset compute 작업자가 포함될 수 있으며 각 작업에는 AEM as a Cloud Service 처리 프로필에서 참조할 수 있는 개별 HTTP 끝점이 있습니다.

## 프로젝트 생성

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Asset compute 프로젝트 생성 클릭스루(오디오 없음)_

[Adobe I/O CLI Asset compute 플러그인](../set-up/development-environment.md#aio-cli)을 사용하여 비어 있는 새 Asset compute 프로젝트를 생성합니다.

1. 명령줄에서 프로젝트를 포함할 폴더로 이동합니다.
1. 명령줄에서 `aio app init`을(를) 실행하여 대화형 프로젝트 생성 CLI를 시작합니다.
   + 이 명령은 Adobe I/O 인증을 묻는 웹 브라우저를 생성할 수 있습니다. 그럴 경우 [필요한 Adobe 서비스 및 제품](../set-up/accounts-and-services.md)과(와) 연결된 Adobe 자격 증명을 제공하십시오. 로그인할 수 없는 경우 [프로젝트를 생성하는 방법에 대한 지침](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)을(를) 따르십시오.
1. __조직 선택__
   + AEM as a Cloud Service, App Builder이 등록된 Adobe 조직을 선택합니다.
1. __프로젝트 선택__
   + 프로젝트를 찾아 선택합니다. App Builder 프로젝트 템플릿에서 만든 [프로젝트 제목](../set-up/app-builder.md)(이 경우 `WKND AEM Asset Compute`)입니다.
1. __Workspace 선택__
   + `Development` 작업 영역 선택
1. __이 프로젝트에 사용할 Adobe I/O 앱 기능을 활성화하시겠습니까? 포함할 구성 요소 선택__
   + `Actions: Deploy runtime actions` 선택
   + 화살표 키를 사용하여 선택/선택 취소하려면 선택하고, 선택/선택 취소하려면 스페이스를 두고, 선택 내용을 확인하려면 Enter 키를 누릅니다.
1. __생성할 액션 유형 선택__
   + `DX Asset Compute Worker v1` 선택
   + 화살표 키를 사용하여 선택하고, 선택 취소/선택할 공간을 확보하고, Enter 키를 사용하여 선택 확인
1. __이 작업의 이름을 어떻게 지정하시겠습니까?__
   + 기본 이름 `worker`을(를) 사용합니다.
   + 프로젝트에 서로 다른 자산 계산을 수행하는 작업자가 여러 개 포함된 경우 이름을 의미적으로 지정합니다

## console.json 생성

개발자 도구에는 Adobe I/O에 연결하는 데 필요한 자격 증명이 포함된 `console.json` 파일이 필요합니다. 이 파일은 Adobe I/O 콘솔에서 다운로드됩니다.

1. asset compute 작업자의 [Adobe I/O](https://console.adobe.io) 프로젝트를 엽니다.
1. `console.json` 자격 증명을 다운로드할 프로젝트 작업 영역을 선택하십시오. 이 경우 `Development`을(를) 선택하십시오
1. Adobe I/O 프로젝트의 루트로 이동한 다음 오른쪽 상단에서 __모두 다운로드__&#x200B;를 탭합니다.
1. 파일이 프로젝트 및 작업 영역 접두사가 있는 `.json` 파일로 다운로드됩니다(예: `wkndAemAssetCompute-81368-Development.json`).
1. 다음을 수행할 수 있습니다.
   + 파일 이름을 `console.json`(으)로 변경하고 Asset compute 작업자 프로젝트의 루트로 이동합니다. 다음은 이 자습서의 접근 방식입니다.
   + 임의의 폴더로 이동하고 구성 항목 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`이(가) 있는 `.env` 파일에서 해당 폴더를 참조합니다. 파일 경로는 절대 경로이거나 프로젝트 루트에 상대적일 수 있습니다. 예:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     또는
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> 메모
> 파일에 자격 증명이 포함되어 있습니다. 프로젝트 내에 파일을 저장하는 경우 파일이 공유되지 않도록 `.gitignore` 파일에 추가해야 합니다. `.env` 파일에도 동일하게 적용됩니다. 이러한 자격 증명 파일은 공유하거나 Git에 저장해서는 안 됩니다.

## GitHub에서 프로젝트 asset compute

최종 Asset compute 프로젝트는 GitHub의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub에는 작업자 및 테스트 사례로 완전히 채워진 프로젝트의 최종 상태가 포함되어 있지만 `.env`, `console.json` 또는 `.aio`과(와) 같은 자격 증명은 포함되어 있지 않습니다._
