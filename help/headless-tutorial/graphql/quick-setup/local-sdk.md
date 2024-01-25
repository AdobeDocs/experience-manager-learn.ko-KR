---
title: 로컬 AEM SDK를 사용한 AEM Headless 빠른 설정
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. AEM SDK를 설치하고 샘플 콘텐츠를 추가한 다음 GraphQL API를 사용하여 AEM의 콘텐츠를 사용하는 애플리케이션을 배포합니다. AEM이 옴니채널 경험을 어떻게 강화하는지 알아보십시오.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
duration: 327
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 1%

---

# 로컬 AEM SDK를 사용한 AEM Headless 빠른 설정 {#setup}

AEM Headless 빠른 설정은 WKND Site 샘플 프로젝트 및 AEM Headless GraphQL API에 대한 콘텐츠를 사용하는 샘플 React 앱(SPA)의 콘텐츠를 사용하여 AEM Headless를 실습해 볼 수 있도록 합니다. 이 안내서에서는 [AEM AS A CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1. AEM SDK 설치 {#aem-sdk}

이 설정에서는 [AEM AS A CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) AEM GraphQL API를 살펴보십시오. 이 섹션에서는 AEM SDK를 설치하고 작성자 모드에서 실행하는 방법에 대한 빠른 안내서를 제공합니다. 로컬 개발 환경 설정에 대한 자세한 안내서 [은(는) 여기에서 찾을 수 있음](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> 또한 다음을 사용하여 자습서를 따를 수 있습니다. [AEM as a Cloud Service 환경](./cloud-service.md). Cloud 환경 사용에 대한 추가 참고가 자습서 전체에 포함되어 있습니다.

1. 다음 위치로 이동 **[소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** 최신 버전의 를 다운로드합니다. **AEM SDK**.

   ![소프트웨어 배포 포털](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. 다운로드의 압축을 풀고 Quickstart jar 를 복사합니다(`aem-sdk-quickstart-XXX.jar`) 전용 폴더로, 즉 `~/aem-sdk/author`.
1. jar 파일 이름을 로 변경합니다. `aem-author-p4502.jar`.

   다음 `author` name 은 작성자 모드에서 Quickstart jar가 시작되도록 지정합니다. 다음 `p4502` 포트 4502에서 빠른 시작 실행을 지정합니다.

1. AEM 인스턴스를 설치하고 시작하려면 jar 파일이 포함된 폴더에서 명령 프롬프트를 열고 다음 명령 를 실행합니다.

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 관리자 암호 입력 `admin`. 모든 관리자 암호는 허용되지만 을 사용하는 것이 좋습니다. `admin` 로컬 개발을 통해 재구성의 필요성을 줄일 수 있습니다.
1. AEM 서비스 설치가 완료되면 새 브라우저 창이에서 열립니다 [http://localhost:4502](http://localhost:4502).
1. 사용자 이름으로 로그인 `admin` 및 AEM 초기 시작 중에 선택한 암호(일반적으로 `admin`).

## 2. 샘플 콘텐츠 설치 {#install-sample-content}

샘플 콘텐츠 **WKND 참조 사이트** 은 자습서를 가속화하는 데 사용됩니다. WKND는 AEM 트레이닝과 함께 자주 사용되는 가상의 라이프스타일 브랜드입니다.

WKND 사이트에는 [GraphQL 엔드포인트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). 실제 구현에서는 다음과 같은 문서화된 단계를 수행합니다. [GraphQL 엔드포인트 포함](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) 고객 프로젝트에서. A [CORS](#cors-config) 는 또한 WKND Site의 일부로 패키지화되어 있습니다. 외부 애플리케이션에 대한 액세스 권한을 부여하려면 CORS 구성이 필요합니다. 자세한 내용 [CORS](#cors-config) 은 아래에 있습니다.

1. WKND 사이트용 컴파일된 최신 AEM 패키지 다운로드: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > AEM as a Cloud Service 및 와 호환되는 표준 버전을 다운로드해야 합니다. **아님** 다음 `classic` 버전.

1. 다음에서 **AEM 시작** 메뉴, 다음으로 이동 **도구** > **배포** > **패키지**.

   ![패키지로 이동](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. 클릭 **패키지 업로드** 이전 단계에서 다운로드한 WKND 패키지를 선택합니다. **설치**&#x200B;를 클릭하여 패키지를 설치합니다.

1. 다음에서 **AEM 시작** 메뉴, 다음으로 이동 **에셋** > **파일** > **WKND 공유** > **영어** > **모험**.

   ![모험 폴더 보기](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   WKND 브랜드에서 추진하는 다양한 모험을 구성하는 모든 에셋의 폴더입니다. 여기에는 이미지 및 비디오와 같은 기존 미디어 유형 및 AEM과 유사한 특정 미디어가 포함됩니다 **컨텐츠 조각**.

1. 을(를) 클릭하여 **다운힐 스키 와이오밍** 폴더를 클릭하고 **활강 스키 와이오밍 콘텐츠 조각** 카드:

   ![컨텐츠 조각 카드](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. 활강 스키 와이오밍 모험을 위한 콘텐츠 조각 편집기가 열립니다.

   ![콘텐츠 조각 편집기](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   다양한 필드가 다음과 같음을 관찰하십시오. **제목**, **설명**, 및 **활동** 조각을 정의합니다.

   **컨텐츠 조각** 는 AEM에서 컨텐츠를 관리할 수 있는 방법 중 하나입니다. 콘텐츠 조각은 텍스트, 리치 텍스트, 날짜 또는 다른 콘텐츠 조각에 대한 참조와 같은 구조화된 데이터 요소로 구성된 재사용 가능한 프레젠테이션 독립적인 콘텐츠입니다. 콘텐츠 조각은 나중에 빠른 설정에서 더 자세히 살펴봅니다.

1. 클릭 **취소** 을 눌러 조각을 닫습니다. 자유롭게 다른 폴더 중 일부로 이동하여 다른 어드벤처 콘텐츠를 살펴보십시오.

>[!NOTE]
>
> Cloud Service 환경을 사용하는 경우 방법에 대한 설명서를 참조하십시오. [Cloud Service 환경에 WKND 참조 사이트 와 같은 코드 베이스 배포](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. WKND React 앱 다운로드 및 실행 {#sample-app}

이 자습서의 목표 중 하나는 GraphQL API를 사용하여 외부 애플리케이션에서 AEM 콘텐츠를 사용하는 방법을 보여 주는 것입니다. 이 자습서에서는 React 앱 예제를 사용합니다. React 앱은 AEM GraphQL API와의 통합에 초점을 맞추기 위해 의도적으로 간단합니다.

1. 새 명령 프롬프트를 열고 GitHub에서 샘플 React 앱을 복제합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 에서 React 앱 열기 `aem-guides-wknd-graphql/react-app` 원하는 IDE에서.
1. IDE에서 파일을 엽니다. `.env.development` 위치: `/.env.development`. 확인 `REACT_APP_AUTHORIZATION` 행이 주석 처리 제거되며 파일이 다음 변수를 선언합니다.

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   확인 `REACT_APP_HOST_URI` 는 로컬 AEM SDK를 가리킵니다. 편의를 위해 이 빠른 시작은 React 앱을 와 연결합니다.  **AEM 작성자**. **작성자** 서비스에는 인증이 필요하므로 앱은 `admin` 연결을 설정할 사용자입니다. AEM Author에 앱을 연결하는 것은 개발 중에 일반적인 관례입니다. 변경 사항을 게시하지 않고도 콘텐츠를 빠르게 반복할 수 있기 때문입니다.

   >[!NOTE]
   >
   > 프로덕션 시나리오에서는 앱이 AEM에 연결됩니다. **게시** 환경. 이에 대해서는 다음에서 자세히 설명합니다. _프로덕션 배포_ 섹션.


1. React 앱을 설치하고 시작합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창에서 앱이 자동으로 열립니다. [http://localhost:3000](http://localhost:3000).

   ![React 스타터 앱](assets/quick-setup/aem-sdk/react-app__home-view.png)

   AEM의 모험 콘텐츠 목록이 표시됩니다.

1. 어드벤처 세부 사항을 보려면 어드벤처 이미지 중 하나를 클릭하십시오. 모험에 대한 세부 정보를 반환하도록 AEM에 요청합니다.

   ![어드벤처 세부 사항 보기](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. 브라우저의 개발자 도구를 사용하여 **네트워크** 요청. 보기 **XHR** 에 대한 여러 GET 요청을 요청하고 관찰합니다. `/graphql/execute.json/...`. 이 경로 접두사는 AEM 지속 쿼리 끝점을 호출하여 이름 및 접두사 다음의 인코딩된 매개 변수를 사용하여 실행할 지속 쿼리를 선택합니다.

   ![GraphQL Endpoint XHR 요청](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. AEM에서 컨텐츠 편집

React 앱이 실행 중인 경우 AEM에서 콘텐츠를 업데이트하고 변경 사항이 앱에 반영되었는지 확인합니다.

1. AEM으로 이동 [http://localhost:4502](http://localhost:4502).
1. 다음으로 이동 **에셋** > **파일** > **WKND 공유** > **영어** > **모험** > **[발리 서프 캠프](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![발리 서프 캠프 폴더](assets/setup/bali-surf-camp-folder.png)

1. 을(를) 클릭하여 **발리 서프 캠프** 콘텐츠 조각 편집기를 여는 콘텐츠 조각.
1. 수정 **제목** 및 **설명** 모험.

   ![콘텐츠 조각 수정](assets/setup/modify-content-fragment-bali.png)

1. 클릭 **저장** 변경 내용을 저장합니다.
1. 다음 위치에서 React 앱 새로 고침: [http://localhost:3000](http://localhost:3000) 변경 사항을 보려면:

   ![업데이트된 발리 서프 캠프 어드벤처](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. GraphiQL 살펴보기 {#graphiql}

1. 열기 [GraphiQL](http://localhost:4502/aem/graphiql.html) 로 이동하여 **도구** > **일반** > **GraphQL 쿼리 편집기**
1. 왼쪽에서 기존 지속 쿼리를 선택하고 실행하여 결과를 확인합니다.

   >[!NOTE]
   >
   > GraphiQL 도구 및 GraphQL API [이 자습서의 뒷부분에서 자세히 살펴보았습니다](../multi-step/explore-graphql-api.md).

## 축하합니다!{#congratulations}

축하합니다. 이제 GraphQL에서 AEM 콘텐츠를 사용하는 외부 애플리케이션이 있습니다. 언제든지 React 앱에서 코드를 검사하고 기존 콘텐츠 조각을 수정해 보십시오.

### 다음 단계

* [AEM Headless 튜토리얼 시작](../multi-step/overview.md)
