---
title: AEM 컨텐츠 조각 콘솔 확장 테스트
description: 프로덕션에 배포하기 전에 AEM 컨텐츠 조각 콘솔 확장을 테스트하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---


# 확장 테스트

AEM 컨텐츠 조각 콘솔 확장은 확장이 속한 Adobe 조직의 AEM as a Cloud Service 환경에 대해 테스트할 수 있습니다.

확장 테스트는 AEM 컨텐츠 조각 콘솔에서 확장을 로드하도록 지시하는 특별히 제작된 URL을 통해 수행됩니다.

## AEM 컨텐츠 조각 콘솔 URL

![AEM 컨텐츠 조각 콘솔 URL](./assets/test/content-fragment-console-url.png){align="center"}

비프로덕션 확장을 AEM 컨텐츠 조각 콘솔에 마운트하는 URL을 만들려면 원하는 AEM 컨텐츠 조각 콘솔의 URL을 수집해야 합니다. AEM as a Cloud Service 환경으로 이동하여 확장을 테스트하고 AEM 컨텐츠 조각 콘솔의 URL을 복사합니다.

1. 원하는 AEM as a Cloud Service env에 로그인합니다.

   + 에 AEM 개발 환경 사용 [개발 빌드 테스트](#testing-development-builds)
   + 에 AEM Stage 또는 QA 개발 환경 사용 [단계 빌드 테스트](#testing-stage-builds)

1. 을(를) 선택합니다 __컨텐츠 조각__ 아이콘.
1. AEM 컨텐츠 조각 콘솔이 브라우저에서 로드될 때까지 기다립니다.
1. 브라우저의 주소 표시줄에서 AEM 컨텐츠 조각 콘솔의 URL을 복사합니다. 이 URL은 다음과 같아야 합니다.

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

이 URL은 개발 및 스테이지 테스트를 위한 URL을 작성할 때 아래에 사용됩니다.

## 로컬 개발 빌드 테스트

1. 확장 프로젝트의 루트에 대한 명령줄을 엽니다.
1. AEM 컨텐츠 조각 콘솔 확장을 로컬 App Builder 앱으로 실행

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

위에 표시된 로컬 애플리케이션 URL을 주목하십시오. `-> https://localhost:9080`

1. 다음 두 쿼리 매개 변수를 [AEM 컨텐츠 조각 콘솔의 URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`일반적으로 `&ext=https://localhost:9080`.

   테스트 URL은 다음과 같아야 합니다.

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://localhost:9080
   ```

1. 테스트 URL을 복사하여 브라우저에 붙여넣습니다.

   + 처음에, 그리고 주기적으로 [https 인증서 수락](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 로컬 애플리케이션의 호스트(`https://localhost:9080`).

1. AEM 컨텐츠 조각 콘솔은 로컬 App Builder 앱이 실행 중인 한 테스트를 위해 확장에 삽입된 로컬 버전과 함께 로드되고, 핫 재로드 변경 사항이 제공됩니다.

이 방법을 사용하는 경우 개발 중인 확장은 경험에만 영향을 주며 AEM 컨텐츠 조각 콘솔의 다른 모든 사용자는 삽입된 확장 없이 경험에 액세스할 수 있습니다.

## 테스트 단계 빌드

1. 확장 프로젝트의 루트에 대한 명령줄을 엽니다.
1. 스테이지 작업 영역이 활성 상태인지(또는 테스트에 사용되는 모든 Workspace) 확인합니다.

   ```shell
   $ aio app use -w Stage
   ```
   변경 내용 병합 `.env` 및 `.aio`.
1. 업데이트된 확장 앱 빌더 앱을 배포합니다. 로그인하지 않은 경우 를 실행합니다. `aio login` 먼저

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. 다음 두 쿼리 매개 변수를 [AEM 컨텐츠 조각 콘솔의 URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   테스트 URL은 다음과 같아야 합니다.

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html
   ```

1. 테스트 URL을 복사하여 브라우저에 붙여넣습니다.
1. AEM 컨텐츠 조각 콘솔은 의 스테이지 작업 공간에 배포된 확장 버전을 삽입합니다. 이 단계 URL은 테스트를 위해 QA 또는 비즈니스 사용자에게 공유할 수 있습니다.

이 방법을 사용하는 경우 준비 단계 URL로 액세스할 때 스테이징된 확장은 AEM 컨텐츠 조각 콘솔에서만 삽입됩니다.

1. 배포된 확장은 실행을 통해 업데이트할 수 있습니다 `aio app deploy` 이러한 변경 사항은 테스트 URL을 사용할 때 자동으로 반영됩니다.
1. 테스트할 확장을 제거하려면 다음을 실행합니다 `aio app undeploy`.



