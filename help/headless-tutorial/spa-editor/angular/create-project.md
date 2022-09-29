---
title: SPA 편집기 프로젝트 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기와 통합된 Angular 애플리케이션의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 사용하는 방법을 알아봅니다.
sub-product: sites
feature: SPA Editor, AEM Project Archetype
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 3%

---

# SPA 편집기 프로젝트 {#create-project}

AEM SPA 편집기와 통합된 Angular 애플리케이션의 시작점으로 Adobe Experience Manager(AEM) Maven 프로젝트를 사용하는 방법을 알아봅니다.

## 목표

1. Maven 원형에서 빌드된 새 AEM SPA 편집기 프로젝트의 구조를 이해합니다.
2. 시작 프로젝트를 AEM의 로컬 인스턴스에 배포합니다.

## 빌드할 내용

이 장에서는 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype). AEM 프로젝트는 매우 간단한 Angular SPA 시작점으로 부트스트되었습니다. 이 장에서 사용되는 프로젝트는 WKND SPA의 구현 기반 역할을 하며, 이후 장에 구축됩니다.

![WKND SPA Angular 시작 프로젝트](./assets/create-project/what-you-will-build.png)

*클래식 Hello World 메시지.*

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment). Adobe Experience Manager의 새 인스턴스가 **작성자** 모드, 로컬에서 실행 중입니다.

## 프로젝트 가져오기

AEM용 Maven 다중 모듈 프로젝트를 만드는 옵션은 몇 가지가 있습니다. 이 자습서에서는 최신 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 를 참조하십시오. 여러 버전의 AEM을 지원하기 위해 프로젝트 코드가 수정되었습니다. 검토하세요 [이전 버전과의 호환성에 대한 참고 사항](overview.md#compatibility).

>[!CAUTION]
>
>을 사용하는 것이 가장 좋습니다 **최신** 버전 [원형](https://github.com/adobe/aem-project-archetype) 실제 구현에 대한 새 프로젝트를 생성하기 위해 AEM 프로젝트는 `aemVersion` 원형 속성입니다.

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 다음 폴더 및 파일 구조는 로컬 파일 시스템의 Maven 원형에 의해 생성된 AEM 프로젝트를 나타냅니다.

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. 다음 속성은 Marketing Cloud ID 서비스의 [AEM 프로젝트 원형](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 속성 | 값 |
   |-----------------|---------------------------------------|
   | aemVersion | 구름 |
   | appTitle | WKND SPA Angular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | 패키지 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 다음 사항에 주의하십시오. `frontendModule=angular` 속성을 사용합니다. 이렇게 하면 AEM Project Archetype에서 시작 모드로 프로젝트를 부트스트랩하도록 지시합니다 [Angular 코드 기본](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) AEM SPA 편집기에 사용할 수 있습니다.

## 프로젝트 빌드

그런 다음 Maven을 사용하여 프로젝트 코드를 AEM의 로컬 인스턴스에 컴파일하고 빌드하고 배포합니다.

1. AEM 인스턴스가 포트에서 로컬로 실행 중인지 확인합니다. **4502년**.
2. 명령줄 터미널에서 Maven이 설치되어 있는지 확인합니다.

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 다음 Maven 명령을 `aem-guides-wknd-spa` 프로젝트를 빌드하고 AEM에 배포할 디렉토리:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   프로젝트의 여러 모듈을 컴파일하고 AEM에 배포해야 합니다.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven 프로필 ***autoInstallSinglePackage*** 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 포트에서 로컬로 실행되는 AEM 인스턴스에 배포됩니다 **4502년** 그리고 **admin:admin**.

4. 다음으로 이동 **[!UICONTROL 패키지 관리자]** 로컬 AEM 인스턴스에서 다음을 수행합니다. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 에 대한 패키지가 3개 표시됩니다 `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` 및 `wknd-spa-angular.ui.content`.

   ![WKND SPA 패키지](./assets/create-project/package-manager.png)

   프로젝트에 필요한 모든 사용자 지정 코드는 이러한 패키지에 번들로 제공되며 AEM 런타임에 설치됩니다.

6. 다음에 대한 여러 패키지가 표시되어야 합니다 `spa.project.core` 및 `core.wcm.components`. 이러한 종속성은 원형 유형에 의해 자동으로 포함되는 종속성입니다. 에 대한 추가 정보 [AEM 코어 구성 요소는 여기에서 찾을 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html).

## 컨텐츠 작성

그런 다음 원형에서 생성된 시작 SPA을 열고 일부 컨텐츠를 업데이트합니다.

1. 로 이동합니다 **[!UICONTROL Sites]** 콘솔: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA에는 국가, 언어 및 홈 페이지가 포함된 기본 사이트 구조가 포함되어 있습니다. 이 계층 구조는 기본적으로 `language_country` 및 `isSingleCountryWebsite`. 이러한 값은 [사용 가능한 속성](https://github.com/adobe/aem-project-archetype#available-properties) 프로젝트를 생성할 때

2. 를 엽니다. **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** 페이지를 선택하고 **[!UICONTROL 편집]** 메뉴 모음의 단추:

   ![사이트 콘솔](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL 텍스트]** 구성 요소가 페이지에 이미 추가되었습니다. 이 구성 요소는 AEM의 다른 구성 요소와 같이 편집할 수 있습니다.

   ![텍스트 구성 요소 업데이트](./assets/create-project/update-text-component.gif)

4. 추가 **[!UICONTROL 텍스트]** 구성 요소를 생성하지 않습니다.

   작성 경험은 기존 AEM Sites 페이지의 경험과 유사합니다. 현재 사용할 수 있는 구성 요소 수는 제한되어 있습니다. 자습서의 교육 과정에 더 많은 정보가 추가됩니다.

## 단일 페이지 애플리케이션 Inspect

그런 다음 브라우저의 개발자 도구를 사용하여 단일 페이지 애플리케이션인지 확인합니다.

1. 에서 **[!UICONTROL 페이지 편집기]**&#x200B;를 클릭하고 **[!UICONTROL 페이지 정보]** 메뉴 > **[!UICONTROL 게시됨으로 보기]**:

   ![게시됨으로 보기 단추](./assets/create-project/view-as-published.png)

   이렇게 하면 쿼리 매개 변수가 포함된 새 탭이 열립니다 `?wcmmode=disabled` AEM 편집기를 효과적으로 끕니다. [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 페이지의 소스를 보고 텍스트 컨텐츠가 **[!DNL Hello World]** 또는 다른 콘텐츠를 찾을 수 없습니다. 대신 다음과 같은 HTML이 표시됩니다.

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` 는 페이지에 로드되고 컨텐츠 렌더링을 담당하는 Angular SPA 입니다.

   *컨텐츠는 어디에서 얻습니까?*

3. 탭으로 돌아갑니다. [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 새로 고침 중에 브라우저의 개발자 도구를 열고 페이지의 네트워크 트래픽을 검사합니다. 보기 **XHR** 요청:

   ![XHR 요청](./assets/create-project/xhr-requests.png)

   에 대한 요청이 있어야 합니다. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 여기에는 SPA을 구동하는 JSON으로 포맷된 모든 컨텐츠가 포함됩니다.

5. 새 탭에서 를 엽니다. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   요청 `en.model.json` 응용 프로그램을 구동할 컨텐츠 모델을 나타냅니다. Inspect JSON 출력을 참조하며, 다음을 나타내는 코드 조각을 찾을 수 있어야 합니다 **[!UICONTROL 텍스트]** 구성 요소

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   다음 장에서는 JSON 컨텐츠가 AEM 구성 요소에서 SPA 구성 요소에 매핑되어 AEM SPA Editor 경험을 기반으로 형성되는 방식을 검사합니다.

   >[!NOTE]
   >
   > JSON 출력을 자동으로 포맷하기 위해 브라우저 확장을 설치하는 것이 도움이 될 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM SPA 편집기 프로젝트를 만들었습니다!

현재는 매우 간단하지만 다음 몇 장에서 더 많은 기능이 추가됩니다.

### 다음 단계 {#next-steps}

[SPA 통합](integrate-spa.md) - SPA 소스 코드가 AEM Project와 어떻게 통합되는지 배우고 SPA을 신속하게 개발할 수 있는 도구를 이해합니다.
