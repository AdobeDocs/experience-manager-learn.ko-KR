---
title: Adobe I/O Runtime 작업 및 AEM 이벤트
description: Adobe I/O Runtime 작업을 사용하여 AEM 이벤트를 수신하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# Adobe I/O Runtime 작업 및 AEM 이벤트

를 사용하여 AEM 이벤트를 수신하는 방법 알아보기 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) 작업을 수행하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime은 Adobe I/O 이벤트에 응답하여 코드를 실행할 수 있는 서버를 사용하지 않는 플랫폼입니다. 따라서 인프라 걱정 없이 이벤트 기반 애플리케이션을 구축할 수 있습니다.

이 예에서는 Adobe I/O Runtime을 만듭니다 [작업](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) AEM 이벤트를 수신하고 이벤트 세부 사항을 기록합니다.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

높은 수준의 단계는 다음과 같습니다.

- Adobe Developer 콘솔에서 프로젝트 만들기
- 로컬 개발을 위한 프로젝트 초기화
- Adobe Developer 콘솔에서 프로젝트 구성
- AEM 이벤트 트리거 및 작업 실행 확인

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- AEM as a Cloud Service 환경 [AEM 이벤트 활성화됨](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- 액세스 대상: [Adobe Developer 콘솔](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 가 로컬 컴퓨터에 설치되었습니다.

>[!IMPORTANT]
>
>AEM as a Cloud Service 이벤트는 프리릴리스 모드의 등록된 사용자만 사용할 수 있습니다. AEM as a Cloud Service 환경에서 AEM 이벤트를 활성화하려면 [AEM 이벤트 팀](mailto:grp-aem-events@adobe.com).

## Adobe Developer 콘솔에서 프로젝트 만들기

Adobe Developer 콘솔에서 프로젝트를 만들려면 다음 단계를 수행하십시오.

- 다음으로 이동 [Adobe Developer 콘솔](https://developer.adobe.com/) 및 클릭 **콘솔** 단추를 클릭합니다.

- 다음에서 **빠른 시작** 섹션, 클릭 **템플릿에서 프로젝트 만들기**. 그런 다음 **템플릿 찾아보기** 대화 상자, 선택 **App Builder** 템플릿.

- 프로젝트 제목, 앱 이름을 업데이트하고 필요한 경우 작업 영역을 추가합니다. 그런 다음 을 클릭합니다. **저장**.

  ![Adobe Developer 콘솔에서 프로젝트 만들기](../assets/examples/runtime-action/create-project.png)


## 로컬 개발을 위한 프로젝트 초기화

프로젝트에 Adobe I/O Runtime 작업을 추가하려면 로컬 개발을 위해 프로젝트를 초기화해야 합니다. 로컬 컴퓨터 열기 터미널에서 프로젝트를 초기화할 위치로 이동하여 다음 단계를 수행합니다.

- 실행하여 프로젝트 초기화

  ```bash
  aio app init
  ```

- 다음 항목 선택 `Organization`, `Project` 이전 단계에서 및 작업 영역을 만들었습니다. 위치 `What templates do you want to search for?` 단계, 선택 `All Templates` 옵션을 선택합니다.

  ![조직-프로젝트-선택 - 프로젝트 초기화](../assets/examples/runtime-action/all-templates.png)

- 템플릿 목록에서 를 선택합니다. `@adobe/generator-app-excshell` 옵션을 선택합니다.

  ![확장성 템플릿 - 프로젝트 초기화](../assets/examples/runtime-action/extensibility-template.png)

- 즐겨찾는 IDE에서 프로젝트(예: VSCode)를 엽니다.

- 선택한 항목 _확장성 템플릿_ (`@adobe/generator-app-excshell`)는 일반 런타임 작업을 제공하며 코드는 다음과 같습니다 `src/dx-excshell-1/actions/generic/index.js` 파일. 간단하게 업데이트하고, 이벤트 세부 사항을 기록하고, 성공 응답을 반환하겠습니다. 그러나 다음 예에서는 수신된 AEM 이벤트를 처리하도록 향상되었습니다.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- 마지막으로 를 실행하여 Adobe I/O Runtime에 업데이트된 작업을 배포합니다.

  ```bash
  aio app deploy
  ```

## Adobe Developer 콘솔에서 프로젝트 구성

AEM 이벤트를 수신하고 이전 단계에서 만든 Adobe I/O Runtime 작업을 실행하려면 Adobe Developer 콘솔에서 프로젝트를 구성합니다.

- Adobe Developer 콘솔에서 다음 위치로 이동합니다. [프로젝트](https://developer.adobe.com/console/projects) 이전 단계에서 을(를) 생성한 다음 을(를) 클릭하여 엽니다. 다음 항목 선택 `Stage` workspace, 작업이 배포된 위치입니다.

- 클릭 **서비스 추가** 단추 및 선택 **API** 옵션을 선택합니다. 다음에서 **API 추가** 모달, 선택 **Adobe 서비스** > **I/O 관리 API** 및 클릭 **다음**&#x200B;추가 구성 단계를 수행하고 을 클릭합니다. **구성된 API 저장**.

  ![서비스 추가 - 프로젝트 구성](../assets/examples/runtime-action/add-io-management-api.png)

- 마찬가지로 을 클릭합니다. **서비스 추가** 단추 및 선택 **이벤트** 옵션을 선택합니다. 다음에서 **이벤트 추가** 대화 상자, 선택 **Experience Cloud** > **AEM Sites**, 및 클릭 **다음**. 추가 구성 단계를 수행하고 AEMCS 인스턴스, 이벤트 유형 및 기타 세부 정보를 선택합니다.

- 마지막으로 **이벤트 수신 방법** 단계, 확장 **런타임 작업** 옵션을 선택하고 _일반_ 이전 단계에서 만든 작업입니다. 클릭 **구성된 이벤트 저장**.

  ![런타임 작업 - 프로젝트 구성 ](../assets/examples/runtime-action/select-runtime-action.png)

- 이벤트 등록 세부 사항을 검토하고 **추적 디버그** 을(를) 탭하고 확인합니다. **챌린지 프로브** 요청 및 응답.

  ![이벤트 등록 세부 정보](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## AEM 이벤트 트리거

위의 Adobe Developer 콘솔 프로젝트에 등록된 AEM as a Cloud Service 환경에서 AEM 이벤트를 트리거하려면 다음 단계를 따르십시오.

- AEM 를 통해 as a Cloud Service 작성자 환경에 액세스하고 로그인합니다. [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 다음에 따라 **구독한 이벤트**, 콘텐츠 조각 만들기, 업데이트, 삭제, 게시 또는 게시 취소.

## 이벤트 세부 사항 검토

위의 단계를 완료하면 AEM 이벤트가 일반 작업에 전달되는 것을 볼 수 있습니다.

다음에서 이벤트 세부 사항을 검토할 수 있습니다. **추적 디버그** 이벤트 등록 세부 정보 탭.

![AEM 이벤트 세부 정보](../assets/examples/runtime-action/aem-event-details.png)


## 다음 단계

다음 예제에서는 AEM 이벤트를 처리하고, AEM 작성자 서비스를 다시 호출하여 컨텐츠 세부 정보를 가져오고, 세부 정보를 Adobe I/O Runtime 저장소에 저장하고, SPA(단일 페이지 애플리케이션)을 통해 표시하도록 이 작업을 개선해 보겠습니다.
