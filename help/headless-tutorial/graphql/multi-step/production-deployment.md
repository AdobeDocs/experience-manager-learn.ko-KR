---
title: AEM 게시 서비스를 사용한 프로덕션 배포 - AEM 헤드리스 시작하기 - GraphQL
description: AEM 작성자 및 게시 서비스 및 헤드리스 애플리케이션에 대한 권장 배포 패턴에 대해 알아봅니다. 이 자습서에서는 환경 변수를 사용하여 대상 환경을 기반으로 GraphQL 엔드포인트를 동적으로 변경하는 방법을 알아봅니다. CORS(원본 간 리소스 공유)를 위해 AEM을 올바르게 구성하는 방법을 알아봅니다.
sub-product: assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 1%

---


# AEM 게시 서비스를 사용한 프로덕션 배포

이 자습서에서는 작성자 인스턴스에서 게시 인스턴스로 배포되는 컨텐츠를 시뮬레이션하도록 로컬 환경을 설정합니다. GraphQL API를 사용하여 AEM 게시 환경의 컨텐츠를 소비하도록 구성된 React 앱의 프로덕션 빌드를 생성합니다. 이 과정에서 환경 변수를 효과적으로 사용하는 방법과 AEM CORS 구성을 업데이트하는 방법을 알아봅니다.

## 전제 조건

이 자습서는 여러 부분으로 구성된 자습서의 일부입니다. 이전 부품에 요약된 단계가 완료된 것으로 가정합니다.

## 목표

방법 알아보기:

* AEM 작성자 및 게시 아키텍처를 이해합니다.
* 환경 변수 관리에 대한 우수 사례를 알아봅니다.
* CORS(원본 간 리소스 공유)를 위해 AEM을 올바르게 구성하는 방법을 알아봅니다.

## 작성자 게시 배포 패턴 {#deployment-pattern}

전체 AEM 환경은 작성자, 게시 및 Dispatcher로 구성됩니다. 작성자 서비스는 내부 사용자가 컨텐츠를 만들고, 관리하고, 미리 보는 곳입니다. 게시 서비스는 &quot;라이브&quot; 환경으로 간주되며 일반적으로 최종 사용자가 상호 작용하는 것입니다. 작성 서비스에서 편집하고 승인되면 컨텐츠가 게시 서비스에 배포됩니다.

AEM 헤드리스 애플리케이션을 사용하는 가장 일반적인 배포 패턴은 애플리케이션의 프로덕션 버전이 AEM Publish 서비스에 연결되어 있는 것입니다.

![높은 수준 배포 패턴](assets/publish-deployment/high-level-deployment.png)

위의 다이어그램은 이러한 일반적인 배포 패턴을 나타냅니다.

1. **컨텐츠 작성자**&#x200B;는 AEM 작성자 서비스를 사용하여 컨텐츠를 작성, 편집 및 관리합니다.
2. **컨텐츠 작성자** 및 다른 내부 사용자는 작성자 서비스에서 바로 컨텐츠를 미리 볼 수 있습니다. 애플리케이션의 미리 보기 버전은 작성자 서비스에 연결하는 데 설정할 수 있습니다.
3. 컨텐츠가 승인되면 AEM 게시 서비스에 **게시된**&#x200B;일 수 있습니다.
4. **최종** 사용자는 애플리케이션의 프로덕션 버전과 상호 작용합니다. 프로덕션 애플리케이션은 게시 서비스에 연결하고 GraphQL API를 사용하여 컨텐츠를 요청하고 사용합니다.

이 자습서에서는 현재 설정에 AEM 게시 인스턴스를 추가하여 위의 배포를 시뮬레이션합니다. 이전 장에서 React App은 작성자 인스턴스에 직접 연결하여 미리 보기로 작동했습니다. React 앱의 프로덕션 빌드가 새 게시 인스턴스에 연결하는 정적 Node.js 서버에 배포됩니다.

마지막으로 세 개의 로컬 서버가 실행됩니다.

* http://localhost:4502 - 작성자 인스턴스
* http://localhost:4503 - 게시 인스턴스
* http://localhost:5000 - 프로덕션 모드에서 React App, 게시 인스턴스에 연결합니다.

## AEM SDK 설치 - 게시 모드 {#aem-sdk-publish}

현재 **Author** 모드에 SDK의 실행 인스턴스가 있습니다. SDK를 **게시** 모드에서 시작하여 AEM 게시 환경을 시뮬레이션할 수도 있습니다.

로컬 개발 환경 설정에 대한 자세한 안내서는 [여기에서 찾을 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. 로컬 파일 시스템에서 게시 인스턴스를 설치할 전용 폴더(예: `~/aem-sdk/publish`)를 만듭니다.
1. 이전 장의 작성자 인스턴스에 사용된 Quickstart jar 파일을 복사하여 `publish` 디렉토리에 붙여넣습니다. 또는 [소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)으로 이동하여 최신 SDK를 다운로드하고 Quickstart jar 파일을 추출합니다.
1. jar 파일의 이름을 `aem-publish-p4503.jar`로 변경합니다.

   `publish` 문자열은 Quickstart jar가 게시 모드에서 시작되도록 지정합니다. `p4503`은 Quickstart 서버가 포트 4503에서 실행되도록 지정합니다.

1. 새 터미널 창을 열고 jar 파일이 포함된 폴더로 이동합니다. AEM 인스턴스를 설치하고 시작합니다.

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 관리자 암호를 `admin`으로 입력합니다. 모든 관리자 암호를 사용할 수 있지만 추가 구성을 방지하려면 로컬 개발에 기본값을 사용하는 것이 좋습니다.
1. AEM 인스턴스 설치가 완료되면 새 브라우저 창이 [http://localhost:4503/content.html](http://localhost:4503/content.html)에 열립니다

   404 찾을 수 없음 페이지를 반환합니다. 완전히 새로운 AEM 인스턴스이며, 설치된 컨텐츠가 없습니다.

## 샘플 컨텐츠 및 GraphQL 엔드포인트 {#wknd-site-content-endpoints} 설치

작성자 인스턴스와 마찬가지로 게시 인스턴스에도 GraphQL 엔드포인트가 활성화되어 있어야 하며 샘플 컨텐츠가 필요합니다. 그런 다음 게시 인스턴스에 WKND 참조 사이트를 설치합니다.

1. WKND Site용 최신 컴파일된 AEM 패키지를 다운로드합니다.[aem-guides-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > AEM as a Cloud Service과 호환되는 표준 버전을 다운로드하십시오. **버전이 아닌**&#x200B;을 다운로드하십시오. `classic`

1. 다음 위치로 직접 이동하여 게시 인스턴스에 로그인합니다.[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)(사용자 이름 `admin`)과 암호 `admin`가 있는 경우).
1. 다음으로 [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)의 패키지 관리자로 이동합니다.
1. **패키지 업로드**&#x200B;를 클릭하고 이전 단계에서 다운로드한 WKND 패키지를 선택합니다. **설치**&#x200B;를 클릭하여 패키지를 설치합니다.
1. 패키지를 설치한 후 WKND 참조 사이트를 이제 [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)에서 사용할 수 있습니다.
1. 메뉴 모음에서 &quot;로그아웃&quot; 단추를 클릭하여 `admin` 사용자로 로그아웃하십시오.

   ![WKND 로그아웃 참조 사이트](assets/publish-deployment/sign-out-wknd-reference-site.png)

   AEM 작성자 인스턴스와 달리 AEM 게시 인스턴스는 기본적으로 익명 읽기 전용 액세스로 설정됩니다. React 응용 프로그램을 실행할 때 익명의 사용자의 경험을 시뮬레이션하려고 합니다.

## 환경 변수를 업데이트하여 게시 인스턴스 {#react-app-publish}

그런 다음 React 애플리케이션에서 사용하는 환경 변수를 업데이트하여 게시 인스턴스를 가리킵니다. React 앱은 **만 프로덕션 모드에서 게시 인스턴스에 연결해야 합니다.**

그런 다음 새 파일 `.env.production.local`을 추가하여 프로덕션 경험을 시뮬레이션합니다.

1. IDE에서 WKND GraphQL React 앱을 엽니다.

1. `aem-guides-wknd-graphql/react-app` 아래에 `.env.production.local` 파일을 추가합니다.
1. `.env.production.local` 을 다음과 같이 채웁니다.

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![새 환경 변수 파일 추가](assets/publish-deployment/env-production-local-file.png)

   환경 변수를 사용하면 애플리케이션 코드 내에 추가 논리를 추가하지 않고 작성자 또는 게시 환경 간에 GraphQL 엔드포인트를 쉽게 전환할 수 있습니다. React에 대한 [사용자 지정 환경 변수에 대한 자세한 내용은 ](https://create-react-app.dev/docs/adding-custom-environment-variables)에서 확인할 수 있습니다.

   >[!NOTE]
   >
   > 게시 환경은 기본적으로 컨텐츠에 익명 액세스를 제공하므로 인증 정보가 포함되지 않습니다.

## 정적 노드 서버 {#static-server} 배포

React 앱은 웹 팩 서버를 사용하여 시작할 수 있지만 개발용입니다. 다음으로, [serve](https://github.com/vercel/serve)을 사용하여 Node.js를 사용하여 React 앱의 프로덕션 빌드를 호스팅하여 프로덕션 배포를 시뮬레이션합니다.

1. 새 터미널 창을 열고 `aem-guides-wknd-graphql/react-app` 디렉토리로 이동합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 다음 명령을 사용하여 [serve](https://github.com/vercel/serve)을 설치합니다.

   ```shell
   $ npm install serve --save-dev
   ```

1. `react-app/package.json`에서 `package.json` 파일을 엽니다. `serve` 스크립트를 추가합니다.

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve` 스크립트는 두 가지 작업을 수행합니다. 먼저 React 앱의 프로덕션 빌드가 생성됩니다. 둘째, Node.js 서버가 시작되고 프로덕션 빌드를 사용합니다.

1. 터미널로 돌아가서 정적 서버를 시작하는 명령을 입력합니다.

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. 새 브라우저를 열고 [http://localhost:5000/](http://localhost:5000/)로 이동합니다. 반응 앱이 제공되어야 합니다.

   ![React 앱 제공](assets/publish-deployment/react-app-served-port5000.png)

   GraphQL 쿼리가 홈 페이지에서 작동하는지 확인합니다. 개발자 도구를 사용하여 **XHR** 요청을 Inspect에 추가합니다. GraphQL POST이 `http://localhost:4503/content/graphql/global/endpoint.json`에 있는 게시 인스턴스에 있는지 확인합니다.

   하지만, 모든 이미지는 홈페이지에서 끊어집니다!

1. Adventure Detail 페이지 중 하나를 클릭합니다.

   ![모험 세부 정보 오류](assets/publish-deployment/adventure-detail-error.png)

   `adventureContributor`에 대해 GraphQL 오류가 발생하는지 확인합니다. 다음 연습에서는 손상된 이미지와 `adventureContributor` 문제가 해결됩니다.

## 절대 이미지 참조 {#absolute-image-references}

`<img src` 속성이 상대 경로로 설정되고 `http://localhost:5000/`에 있는 Node 정적 서버를 가리키도록 종료되기 때문에 이미지가 손상된 것으로 나타납니다. 대신 이러한 이미지는 AEM 게시 인스턴스를 가리킵니다. 이에 대한 몇 가지 잠재적인 해결책이 있습니다. 웹 팩 개발 서버를 사용하는 경우 파일 `react-app/src/setupProxy.js` 은(는) `/content`에 대한 요청에 대해 웹 팩 서버와 AEM 작성자 인스턴스 간에 프록시를 설정합니다. 프록시 구성은 프로덕션 환경에서 사용할 수 있지만 웹 서버 수준에서 구성해야 합니다. 예: [Apache의 프록시 모듈](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

`REACT_APP_HOST_URI` 환경 변수를 사용하여 절대 URL을 포함하도록 앱을 업데이트할 수 있습니다. 대신 AEM GraphQL API 기능을 사용하여 이미지에 대한 절대 URL을 요청하겠습니다.

1. Node.js 서버를 중지합니다.
1. IDE로 돌아가서 `react-app/src/components/Adventures.js`에서 `Adventures.js` 파일을 엽니다.
1. `allAdventuresQuery` 내의 `ImageRef`에 `_publishUrl` 속성을 추가합니다.

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` 및  `_authorUrl` 는 절대 url을  `ImageRef` 보다 쉽게 포함할 수 있도록 개체에 빌드된 값입니다.

1. 위의 단계를 반복하여 `_publishUrl` 속성을 포함하도록 `filterQuery(activity)` 함수에 사용되는 쿼리를 수정합니다.
1. `<img src=''>` 태그를 구성할 때 `_path` 속성 대신 `_publishUrl`를 참조하도록 `function AdventureItem(props)`에서 `AdventureItem` 구성 요소를 수정합니다.

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. `react-app/src/components/AdventureDetail.js`에서 `AdventureDetail.js` 파일을 엽니다.
1. 같은 단계를 반복하여 GraphQL 쿼리를 수정하고 `_publishUrl` 속성을 Adventure에 추가합니다

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. `AdventureDetail.js`에서 Adventure Primary Image와 Contributor Picture 참조의 두 `<img>` 태그를 수정합니다.

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. 터미널로 돌아가서 정적 서버를 시작합니다.

   ```shell
   $ npm run serve
   ```

1. [http://localhost:5000/](http://localhost:5000/)로 이동하여 이미지가 표시되고 `<img src''>` 특성이 `http://localhost:4503`를 가리키는지 확인합니다.

   ![손상된 이미지 수정](assets/publish-deployment/broken-images-fixed.png)

## 컨텐츠 게시 시뮬레이트 {#content-publish}

Adventure Details 페이지가 요청될 때 `adventureContributor`에 대해 GraphQL 오류가 발생합니다. **기여자** 컨텐츠 조각 모델이 아직 게시 인스턴스에 없습니다. **Adventure** 컨텐츠 조각 모델에 대한 업데이트도 게시 인스턴스에서도 사용할 수 없습니다. 이러한 변경 사항은 작성자 인스턴스에 직접 적용되었으며 게시 인스턴스에 배포해야 합니다.

컨텐츠 조각 또는 컨텐츠 조각 모델 업데이트를 사용하는 애플리케이션에 대한 새 업데이트를 롤아웃할 때 고려해야 할 사항입니다.

다음으로, 로컬 작성자 및 게시 인스턴스 간에 컨텐츠 게시를 시뮬레이션할 수 있습니다.

1. 작성자 인스턴스를 시작하고(아직 시작되지 않은 경우) [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)에서 패키지 관리자로 이동합니다.
1. 패키지 [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) 패키지를 다운로드하고 패키지 관리자를 사용하여 설치합니다.

   이 패키지는 작성자 인스턴스가 컨텐츠를 게시 인스턴스에 게시할 수 있도록 하는 구성을 설치합니다. [이 구성에 대한 수동 단계는 여기](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)에 있습니다.

   >[!NOTE]
   >
   > AEM as a Cloud Service 환경에서는 작성 계층이 게시 계층에 컨텐츠를 배포하도록 자동으로 설정됩니다.

1. **AEM 시작** 메뉴에서 **도구** > **자산** > **컨텐츠 조각 모델**&#x200B;으로 이동합니다.

1. **WKND 사이트** 폴더를 클릭합니다.

1. 세 가지 모델을 모두 선택하고 **게시**&#x200B;를 클릭합니다.

   ![컨텐츠 조각 모델 게시](assets/publish-deployment/publish-contentfragment-models.png)

   확인 대화 상자가 나타나면 **게시**&#x200B;를 클릭합니다.

1. [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)의 Bali Surf Camp 컨텐츠 조각으로 이동합니다.

1. 상단 메뉴 막대에서 **게시** 단추를 클릭합니다.

   ![컨텐츠 조각 편집기에서 게시 단추를 클릭합니다](assets/publish-deployment/publish-bali-content-fragment.png)

1. 게시 마법사는 게시해야 하는 종속 자산을 표시합니다. 이 경우 참조된 조각 **stacey-roswell**&#x200B;이 나열되고 여러 이미지도 참조됩니다. 참조된 자산은 조각과 함께 게시됩니다.

   ![게시할 참조된 자산](assets/publish-deployment/referenced-assets.png)

   **게시** 단추를 다시 클릭하여 컨텐츠 조각 및 종속 자산을 게시합니다.

1. [http://localhost:5000/](http://localhost:5000/)에서 실행되는 React 앱으로 돌아갑니다. 이제 발리 서프 캠프를 클릭하여 모험 세부 사항을 볼 수 있습니다.

1. [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)에서 AEM 작성자 인스턴스로 돌아가서 조각의 **제목**&#x200B;을 업데이트하십시오. **조각을 저장** 및 닫습니다. 그런 다음 **조각을 게시** 합니다.
1. [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)로 돌아가서 게시된 변경 사항을 확인합니다.

   ![발리 서프 캠프 게시 업데이트](assets/publish-deployment/bali-surf-camp-update.png)

## COR 구성 업데이트

AEM은 기본적으로 안전하며 비 AEM 웹 속성이 클라이언트측 호출을 수행하도록 허용하지 않습니다. AEM CORS(원본 간 리소스 공유) 구성을 사용하면 특정 도메인에서 AEM을 호출할 수 있습니다.

다음으로, AEM 게시 인스턴스의 CORS 구성을 실험합니다.

1. `npm run serve` 명령을 사용하여 React 앱이 실행 중인 터미널 창으로 돌아갑니다.

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   두 URL이 제공되는지 확인합니다. 하나는 `localhost` 을 사용하고 다른 하나는 로컬 네트워크 IP 주소를 사용합니다.

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)로 시작하는 주소로 이동합니다. 로컬 컴퓨터마다 주소가 약간 다릅니다. 데이터를 가져올 때 CORS 오류가 있는지 확인합니다. 이것은 현재 CORS 구성이 `localhost`의 요청만 허용하기 때문입니다.

   ![CORS 오류](assets/publish-deployment/cors-error-not-fetched.png)

   다음으로, 네트워크 IP 주소에서 요청을 허용하도록 AEM Publish CORS 구성을 업데이트합니다.

1. [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)로 이동하여 사용자 이름 `admin` 및 암호 `admin`로 로그인합니다.

1. [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)로 이동하여 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`에서 WKND GraphQL 구성을 찾습니다.

1. 네트워크 IP 주소를 포함하도록 **허용된 원본** 필드를 업데이트합니다.

   ![CORS 구성 업데이트](assets/publish-deployment/cors-update.png)

   특정 하위 도메인의 모든 요청을 허용하는 정규 표현식을 포함할 수도 있습니다. 변경 사항을 저장합니다.

1. **Apache Sling Referrer Filter**&#x200B;를 검색하고 구성을 검토합니다. 외부 도메인에서 GraphQL 요청을 활성화하려면 **Allow Empty** 구성도 필요합니다.

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   이는 WKND 참조 사이트의 일부로 구성되었습니다. [GitHub 저장소](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)를 통해 전체 OSGi 구성 세트를 볼 수 있습니다.

   >[!NOTE]
   >
   > OSGi 구성은 소스 제어에 커밋되는 AEM 프로젝트에서 관리됩니다. AEM 프로젝트는 Cloud Manager를 사용하여 AEM에 Cloud Service 환경으로 배포할 수 있습니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)은 특정 구현에 대한 프로젝트를 생성하는 데 도움이 될 수 있습니다.

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)로 시작하는 React 앱으로 돌아가서 응용 프로그램에서 더 이상 CORS 오류가 발생하지 않도록 확인합니다.

   ![CORS 오류가 수정되었습니다.](assets/publish-deployment/cors-error-corrected.png)

## 축하합니다! {#congratulations}

축하합니다! 이제 AEM 게시 환경을 사용하여 전체 프로덕션 배포를 시뮬레이션했습니다. AEM에서 CORS 구성을 사용하는 방법도 배웠습니다.

## 기타 리소스

컨텐츠 조각 및 GraphQL에 대한 자세한 내용은 다음 리소스를 참조하십시오.

* [GraphQL에서 컨텐츠 조각을 사용하여 헤드리스 컨텐츠 전달](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [컨텐츠 조각에 사용할 AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Cloud Service으로 AEM에 코드 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
