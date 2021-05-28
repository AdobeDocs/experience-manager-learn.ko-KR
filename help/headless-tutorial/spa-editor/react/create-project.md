---
title: 프로젝트 만들기 | AEM SPA 편집기 및 반응 시작하기
description: AEM SPA 편집기와 통합된 반응형 응용 프로그램의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 생성하는 방법을 알아봅니다.
sub-product: 사이트
feature: SPA 편집기, AEM 프로젝트 원형
version: cloud-service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 1%

---


# 프로젝트 만들기 {#spa-editor-project}

AEM SPA 편집기와 통합된 반응형 응용 프로그램의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 생성하는 방법을 알아봅니다.

## 목표

1. AEM 프로젝트 원형 을 사용하여 SPA 편집기 지원 프로젝트를 생성합니다.
2. 시작 프로젝트를 AEM의 로컬 인스턴스에 배포합니다.

## {#what-build} 빌드할 내용

이 장에서는 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을 기반으로 새 AEM 프로젝트가 생성됩니다. AEM 프로젝트는 React SPA에 대한 매우 간단한 시작점으로 부트스트릿이 됩니다.

**Maven 프로젝트란?** -  [Apache ](https://maven.apache.org/) Mabanis 는 프로젝트를 제작하기 위한 소프트웨어 관리 도구입니다. *모든 Adobe Experience* Manager 구현은 Maven 프로젝트를 사용하여 AEM 맨 위에 사용자 지정 코드를 작성, 관리 및 배포합니다.

**Maven 원형은 무엇입니까?** - Maven  [원형은 ](https://maven.apache.org/archetype/index.html) 새 프로젝트를 생성하기 위한 템플릿 또는 패턴입니다. AEM 프로젝트 원형을 사용하면 사용자 지정 네임스페이스로 새 프로젝트를 생성하고 우수 사례를 따르는 프로젝트 구조를 포함하여 프로젝트를 크게 가속화할 수 있습니다.

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. **author** 모드에서 시작된 Adobe Experience Manager의 새 인스턴스가 로컬로 실행되는지 확인합니다.

## 프로젝트를 만듭니다 {#create}

>[!NOTE]
>
>이 자습서에서는 원형 버전 **27**&#x200B;을 사용합니다. 항상 원형 중 **최신** 버전을 사용하여 새 프로젝트를 생성하는 것이 좋습니다.

1. 명령줄 터미널을 열고 다음 Maven 명령을 입력합니다.

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.5+를 타깃팅하는 경우 `aemVersion="cloud"`을 `aemVersion="6.5.5"`(으)로 바꿉니다. 6.4.8+를 타깃팅하는 경우 `aemVersion="6.4.8"` 을 사용하십시오.

   `frontendModule=react` 속성을 확인합니다. 이렇게 하면 AEM Project Archetype에 AEM SPA 편집기에 사용할 시작 [React 코드 기본](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)을 사용하여 프로젝트를 부트스트랩하도록 지시합니다. `appTitle`, `appId`, `artifactId` 및 `groupId`와 같은 속성은 프로젝트와 목적을 식별하는 데 사용됩니다.

   프로젝트 [를 구성하는 데 사용할 수 있는 속성의 전체 목록은 여기에서 찾을 수 있습니다](https://github.com/adobe/aem-project-archetype#available-properties).

1. 다음 폴더 및 파일 구조는 로컬 파일 시스템의 Maven 원형에 의해 생성됩니다.

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
       |--- core/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- it.tests/
       |--- dispatcher/
       |--- analyse/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
   ```

   각 폴더는 개별 Maven 모듈을 나타냅니다. 이 자습서에서는 주로 React 앱인 `ui.frontend` 모듈로 작업하게 됩니다. 개별 모듈에 대한 자세한 내용은 [AEM Project Archetype 설명서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)에서 확인할 수 있습니다.

## 프로젝트 배포 및 빌드

그런 다음 Maven을 사용하여 프로젝트 코드를 AEM의 로컬 인스턴스에 컴파일하고 빌드하고 배포합니다.

1. AEM 인스턴스가 포트 **4502**&#x200B;에서 로컬로 실행 중인지 확인합니다.
1. 명령줄에서 `aem-guides-wknd-spa.react` 프로젝트 디렉토리로 이동합니다.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 다음 명령을 실행하여 전체 프로젝트를 AEM에 빌드 및 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   빌드는 약 1분 정도 걸리며 다음 메시지로 끝나야 합니다.

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

   Maven 프로필 `autoInstallSinglePackage`은 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 **4502** 포트에서 로컬로 실행되고 `admin:admin` 자격 증명과 함께 AEM 인스턴스에 배포됩니다.

1. 로컬 AEM 인스턴스에서 **패키지 관리자**&#x200B;로 이동합니다.[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)

1. `aem-guides-wknd-spa.react` 접두사가 있는 여러 패키지가 표시됩니다.

   ![WKND SPA 패키지](assets/create-project/package-manager.png)

   *AEM 패키지 관리자*

   프로젝트에 필요한 모든 사용자 지정 코드는 이러한 패키지에 번들로 제공되며 AEM 환경에 설치됩니다.

## 컨텐츠 작성

그런 다음 원형에서 생성된 시작 SPA을 열고 일부 컨텐츠를 업데이트합니다.

1. **사이트** 콘솔로 이동합니다.[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)

   WKND SPA에는 국가, 언어 및 홈 페이지가 포함된 기본 사이트 구조가 포함되어 있습니다. 이 계층 구조는 `language_country` 및 `isSingleCountryWebsite`에 대한 원형 유형의 기본값을 기반으로 합니다. 이러한 값은 프로젝트를 생성할 때 [사용 가능한 속성](https://github.com/adobe/aem-project-archetype#available-properties)을 업데이트하여 덮어쓸 수 있습니다.

2. 페이지를 선택하고 메뉴 모음에서 **편집** 단추를 클릭하여 **us** > **en** > **WKND SPA React 홈 페이지** 페이지를 엽니다.

   ![사이트 콘솔](./assets/create-project/open-home-page.png)

3. **텍스트** 구성 요소가 페이지에 이미 추가되었습니다. 이 구성 요소는 AEM의 다른 구성 요소와 같이 편집할 수 있습니다.

   ![텍스트 구성 요소 업데이트](./assets/create-project/update-text-component.gif)

4. 페이지에 추가 **Text** 구성 요소를 추가합니다.

   작성 경험은 기존 AEM Sites 페이지의 경험과 유사합니다. 현재 사용할 수 있는 구성 요소 수는 제한되어 있습니다. 튜토리얼의 과정에 더 많은 항목이 추가됩니다.

## 단일 페이지 애플리케이션 Inspect

그런 다음 브라우저의 개발자 도구를 사용하여 단일 페이지 애플리케이션인지 확인합니다.

1. **페이지 편집기**&#x200B;에서 **페이지 정보** 단추 > **게시됨으로 보기**&#x200B;를 클릭합니다.

   ![게시됨으로 보기 단추](./assets/create-project/view-as-published.png)

   이렇게 하면 쿼리 매개 변수 `?wcmmode=disabled`을 사용하여 새 탭이 열리고 AEM 편집기가 효과적으로 꺼집니다.[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 페이지의 소스를 보고 텍스트 콘텐츠 **[!DNL Hello World]** 또는 다른 콘텐츠를 찾을 수 없습니다. 대신 다음과 같은 HTML이 표시됩니다.

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 는 페이지에 로드되고 컨텐츠 렌더링을 담당하는 React SPA입니다.

   그러나 *콘텐츠는 어디에서 오는 것입니까?*

3. 탭으로 돌아갑니다.[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 새로 고침 중에 브라우저의 개발자 도구를 열고 페이지의 네트워크 트래픽을 검사합니다. **XHR** 요청을 봅니다.

   ![XHR 요청](./assets/create-project/xhr-requests.png)

   [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)에 대한 요청이 있어야 합니다. 여기에는 SPA을 구동하는 JSON으로 포맷된 모든 컨텐츠가 포함됩니다.

5. 새 탭에서 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)을 엽니다.

   요청 `en.model.json`은 응용 프로그램을 구동할 컨텐츠 모델을 나타냅니다. Inspect은 JSON 출력을 사용하므로 **[!UICONTROL Text]** 구성 요소를 나타내는 코드 조각을 찾을 수 있어야 합니다.

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

   다음 장에서는 이 JSON 컨텐츠가 AEM 구성 요소에서 SPA 구성 요소에 매핑되어 AEM SPA Editor 경험을 기반으로 형성되는 방식을 검사합니다.

   >[!NOTE]
   >
   > JSON 출력을 자동으로 포맷하기 위해 브라우저 확장을 설치하는 것이 도움이 될 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM SPA 편집기 프로젝트를 만들었습니다!

SPA은 아주 간단합니다. 다음 몇 장에 더 많은 기능이 추가됩니다.

### 다음 단계 {#next-steps}

[SPA 통합](integrate-spa.md)  - SPA 소스 코드가 AEM Project와 통합되는 방법을 배우고 SPA을 신속하게 개발할 수 있는 도구를 이해합니다.
