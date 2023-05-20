---
title: AEM 콘텐츠 조각 콘솔 확장 테스트
description: 프로덕션에 배포하기 전에 AEM 콘텐츠 조각 콘솔 확장을 테스트하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 확장 테스트

AEM AEM 콘텐츠 조각 콘솔 확장은 확장이 속한 Adobe 조직의 as a Cloud Service 환경에 대해 테스트할 수 있습니다.

확장 테스트는 AEM 콘텐츠 조각 콘솔에 확장을 로드하도록 지시하는 특별히 제작된 URL을 통해 수행됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## AEM 콘텐츠 조각 콘솔 URL

![AEM 콘텐츠 조각 콘솔 URL](./assets/test/content-fragment-console-url.png){align="center"}

비프로덕션 확장을 AEM 콘텐츠 조각 콘솔에 마운트하는 URL을 만들려면 원하는 AEM 콘텐츠 조각 콘솔의 URL을 수집해야 합니다. AEM as a Cloud Service 환경으로 이동하여 확장을 테스트하고 AEM 콘텐츠 조각 콘솔의 URL을 복사합니다.

1. AEM 원하는 as a Cloud Service 환경에 로그인합니다.

   + AEM 개발 환경 사용 [개발 빌드 테스트](#testing-development-builds)
   + AEM 스테이지 또는 QA 개발 환경 사용 [스테이지 빌드 테스트](#testing-stage-builds)

1. 다음 항목 선택 __컨텐츠 조각__ 아이콘.
1. AEM 콘텐츠 조각 콘솔이 브라우저에서 로드될 때까지 기다립니다.
1. 브라우저의 주소 표시줄에서 AEM 콘텐츠 조각 콘솔의 URL을 복사합니다. URL은 다음과 유사해야 합니다.

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

이 URL은 개발 및 스테이지 테스트용 URL을 만들 때 아래에 사용됩니다.

## 로컬 개발 빌드 테스트

1. 확장 프로젝트의 루트에 대한 명령줄을 엽니다.
1. AEM 콘텐츠 조각 콘솔 확장을 로컬 App Builder 앱으로 실행

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

위에 표시된 로컬 애플리케이션 URL을 확인합니다. `-> https://localhost:9080`

1. 다음 두 개의 쿼리 매개 변수를 [AEM 콘텐츠 조각 콘솔 URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, 일반적으로 `&ext=https://localhost:9080`.

   위의 두 쿼리 매개 변수(`devMode` 및 `ext`)을 로 사용하는 경우 __첫 번째__ 콘텐츠 조각 콘솔에서 해시 경로(`#/@wknd/aem/...`), 이렇게 하면 이후에 매개 변수를 잘못 게시합니다. `#` 작동하지 않습니다.

   테스트 URL은 다음과 같아야 합니다.

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 테스트 URL을 복사하여 브라우저에 붙여넣습니다.

   + 처음에는 그랬다가 주기적으로 그렇게 해야 할 수도 있습니다 [https 인증서 수락](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 로컬 애플리케이션 호스트(`https://localhost:9080`).

1. AEM 콘텐츠 조각 콘솔은 테스트를 위해 로컬 버전의 확장이 주입된 상태로 로드되며 로컬 App Builder 앱이 실행되는 동안 변경 사항을 핫리로드합니다.

>[!IMPORTANT]
>
>이 접근 방식을 사용할 때 개발 중인 확장은 경험에만 영향을 주며 AEM 콘텐츠 조각 콘솔의 다른 모든 사용자는 삽입된 확장 없이 콘텐츠에 액세스합니다.


## 스테이지 빌드 테스트

1. 확장 프로젝트의 루트에 대한 명령줄을 엽니다.
1. 스테이지 작업 공간이 활성화되어 있는지(또는 테스트에 사용되는 모든 작업 영역) 확인합니다.

   ```shell
   $ aio app use -w Stage
   ```

   모든 변경 내용 병합 `.env` 및 `.aio`.

1. 업데이트된 확장 App Builder 앱을 배포합니다. 로그인하지 않은 경우 를 실행합니다. `aio login` 첫 번째.

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

1. 다음 두 개의 쿼리 매개 변수를 [AEM 콘텐츠 조각 콘솔 URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   위의 두 쿼리 매개 변수(`devMode` 및 `ext`)을 로 사용하는 경우 __첫 번째__ 콘텐츠 조각 콘솔에서 해시 경로(`#/@wknd/aem/...`), 이렇게 하면 이후에 매개 변수를 잘못 게시합니다. `#` 작동하지 않습니다.

   테스트 URL은 다음과 같아야 합니다.

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 테스트 URL을 복사하여 브라우저에 붙여넣습니다.
1. AEM 콘텐츠 조각 콘솔에서 의 스테이지 작업 영역에 배포된 확장 버전을 전달합니다. 이 단계 URL은 테스트를 위해 QA 또는 비즈니스 사용자에게 공유할 수 있습니다.

이 접근 방식을 사용할 때 스테이징된 확장은 크래프트 스테이지 URL로 액세스할 때 AEM 콘텐츠 조각 콘솔에만 삽입됩니다.

1. 배포된 확장은 를 실행하여 업데이트할 수 있습니다. `aio app deploy` 다시 말하지만, 이러한 변경 사항은 테스트 URL을 사용할 때 자동으로 반영됩니다.
1. 테스트를 위해 확장을 제거하려면 `aio app undeploy`.
