---
title: 프로젝트 만들기 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA 편집기와 통합된 React 애플리케이션의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 생성하는 방법에 대해 알아봅니다.
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
duration: 344
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---

# 프로젝트 만들기 {#spa-editor-project}

AEM SPA 편집기와 통합된 React 애플리케이션의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 생성하는 방법에 대해 알아봅니다.

## 목표

1. AEM Project Archetype을 사용하여 SPA 편집기 활성화 프로젝트를 생성합니다.
2. 스타터 프로젝트를 AEM의 로컬 인스턴스에 배포합니다.

## 빌드할 내용 {#what-build}

이 장에서는 를 기반으로 새 AEM 프로젝트를 생성합니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). AEM 프로젝트는 React SPA의 매우 간단한 시작점으로 부트스트랩됩니다.

**Maven 프로젝트란 무엇입니까?** - [Apache Maven](https://maven.apache.org/) 는 프로젝트를 빌드하는 소프트웨어 관리 도구입니다. *모든 Adobe Experience Manager* 구현은 Maven 프로젝트를 사용하여 AEM 위에 사용자 지정 코드를 작성, 관리 및 배포합니다.

**Maven Archetype이란?** - A [Maven Archetype](https://maven.apache.org/archetype/index.html) 는 새 프로젝트를 생성하기 위한 템플릿 또는 패턴입니다. AEM Project Archetype을 통해 사용자 정의 네임스페이스로 새 프로젝트를 생성하고 모범 사례를 따르는 프로젝트 구조를 포함하여 프로젝트를 크게 가속화할 수 있습니다.

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment). Adobe Experience Manager의 새 인스턴스가 **작성자** 모드, 가 로컬에서 실행 중입니다.

## 프로젝트 만들기 {#create}

>[!NOTE]
>
>이 자습서에서는 버전을 사용합니다. **35** 원형 중 하나를 선택할 수 있습니다.

1. 명령줄 터미널을 열고 다음 Maven 명령을 입력합니다.

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.5 이상을 타깃팅하는 경우 바꾸기 `aemVersion="cloud"` 포함 `aemVersion="6.5.5"`. 6.4.8 이상을 타깃팅하는 경우 다음을 사용합니다. `aemVersion="6.4.8"`.

   다음 사항에 주목합니다. `frontendModule=react` 속성. 이는 AEM Project Archetype에 Starter로 프로젝트를 부트스트랩하도록 지시합니다 [React 코드 베이스](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) AEM SPA 편집기와 함께 사용됩니다. 다음과 같은 속성 `appTitle`, `appId`, `artifactId`, 및 `groupId` 프로젝트 및 목적을 식별하는 데 사용됩니다.

   프로젝트 구성을 위해 사용 가능한 속성의 전체 목록 [은(는) 여기에서 찾을 수 있음](https://github.com/adobe/aem-project-archetype#available-properties).

1. 로컬 파일 시스템에서 Maven Archetype에 의해 다음 폴더 및 파일 구조가 생성됩니다.

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   각 폴더는 개별 Maven 모듈을 나타냅니다. 이 자습서에서는 주로 `ui.frontend` 모듈 - React 앱입니다. 개별 모듈에 대한 자세한 내용은 [AEM Project Archetype 설명서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## 프로젝트 배포 및 빌드

그런 다음 Maven을 사용하여 AEM의 로컬 인스턴스에 프로젝트 코드를 컴파일, 빌드 및 배포합니다.

1. AEM 인스턴스가 포트에서 로컬로 실행 중인지 확인합니다. **4502**.
1. 명령줄에서 다음 위치로 이동합니다. `aem-guides-wknd-spa.react` 프로젝트 디렉토리.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 다음 명령을 실행하여 전체 프로젝트를 빌드하고 AEM에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   빌드는 약 1분 정도 소요되며 다음 메시지로 종료됩니다.

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven 프로필 `autoInstallSinglePackage` 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 포트에서 로컬로 실행되는 AEM 인스턴스에 배포됩니다. **4502** 의 자격 증명으로 `admin:admin`.

1. 다음으로 이동 **패키지 관리자** 로컬 AEM 인스턴스에서 다음을 수행합니다. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. 접두사가 있는 여러 패키지가 표시됩니다. `aem-guides-wknd-spa.react`.

   ![WKND SPA 패키지](assets/create-project/package-manager.png)

   *AEM 패키지 관리자*

   프로젝트에 필요한 모든 사용자 지정 코드는 이러한 패키지에 번들로 제공되며 AEM 환경에 설치됩니다.

## 콘텐츠 작성

그런 다음 Archetype으로 생성된 스타터 SPA을 열고 일부 콘텐츠를 업데이트합니다.

1. 다음 위치로 이동 **사이트** 콘솔: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA에는 국가, 언어 및 홈 페이지가 있는 기본 사이트 구조가 포함되어 있습니다. 이 계층은 다음에 대한 Archetype의 기본값을 기반으로 합니다 `language_country` 및 `isSingleCountryWebsite`. 이러한 값은 를 업데이트하여 덮어쓸 수 있습니다. [사용 가능한 속성](https://github.com/adobe/aem-project-archetype#available-properties) 프로젝트를 생성할 때.

2. 를 엽니다. **us** > **en** > **WKND SPA React 홈 페이지** 페이지를 선택하고 **편집** 메뉴 모음의 단추:

   ![사이트 콘솔](./assets/create-project/open-home-page.png)

3. A **텍스트** 구성 요소가 페이지에 이미 추가되었습니다. 이 구성 요소는 AEM의 다른 구성 요소와 마찬가지로 편집할 수 있습니다.

   ![텍스트 구성 요소 업데이트](./assets/create-project/update-text-component.gif)

4. 추가 항목 추가 **텍스트** 구성 요소를 페이지에 추가합니다.

   작성 경험은 기존 AEM Sites 페이지의 작성 경험과 유사합니다. 현재는 제한된 수의 구성 요소를 사용할 수 있습니다. 튜토리얼 과정에 따라 더 많은 내용이 추가됩니다.

## Inspect 단일 페이지 애플리케이션

그런 다음 브라우저의 개발자 도구를 사용하는 단일 페이지 애플리케이션인지 확인합니다.

1. 다음에서 **페이지 편집기**&#x200B;를 클릭하고 **페이지 정보** 단추 > **게시됨으로 보기**:

   ![게시됨으로 보기 버튼](./assets/create-project/view-as-published.png)

   쿼리 매개 변수가 있는 새 탭이 열립니다 `?wcmmode=disabled` AEM 편집기를 효과적으로 해제하는 방법: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 페이지의 소스를 보고 텍스트 콘텐츠가 **[!DNL Hello World]** 또는 다른 콘텐츠를 찾을 수 없습니다. 대신 다음과 같은 HTML이 표시됩니다.

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 는 페이지에 로드되고 콘텐츠 렌더링을 담당하는 React SPA입니다.

   그러나 *콘텐츠는 어디에서 가져옵니까?*

3. 탭으로 돌아갑니다. [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 브라우저의 개발자 도구를 열고 새로 고침 중에 페이지의 네트워크 트래픽을 검사합니다. 보기 **XHR** 요청:

   ![XHR 요청](./assets/create-project/xhr-requests.png)

   다음에 대한 요청이 있어야 합니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 여기에는 SPA을 구동하는 JSON 형식의 모든 콘텐츠가 포함되어 있습니다.

5. 새 탭에서 열기 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   요청 `en.model.json` 응용 프로그램을 구동할 콘텐츠 모델을 나타냅니다. Inspect에 JSON 출력이 제공되면 다음을 나타내는 코드 조각을 찾을 수 있습니다. **[!UICONTROL 텍스트]** 구성 요소.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   다음 장에서는 AEM SPA 편집기 경험의 기반을 형성하기 위해 이 JSON 콘텐츠가 AEM 구성 요소에서 SPA 구성 요소로 매핑되는 방법을 검사합니다.

   >[!NOTE]
   >
   > 브라우저 확장 프로그램을 설치하여 JSON 출력의 형식을 자동으로 지정하는 것이 도움이 될 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM SPA 편집기 프로젝트를 방금 만들었습니다!

SPA은 매우 간단합니다. 다음 몇 개의 챕터에서 더 많은 기능이 추가됩니다.

### 다음 단계 {#next-steps}

[SPA 통합](integrate-spa.md) - SPA 소스 코드를 AEM Project와 통합하는 방법을 알아보고 SPA을 신속하게 개발하는 데 사용할 수 있는 도구를 이해합니다.
