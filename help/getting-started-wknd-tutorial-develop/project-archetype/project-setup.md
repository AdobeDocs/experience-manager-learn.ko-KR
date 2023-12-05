---
title: AEM Sites 시작 - 프로젝트 설정
description: Maven 다중 모듈 프로젝트를 만들어 Experience Manager 사이트에 대한 코드 및 구성을 관리합니다.
version: 6.5, Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 659
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1684'
ht-degree: 1%

---

# 프로젝트 설정 {#project-setup}

이 튜토리얼에서는 Adobe Experience Manager 사이트에 대한 코드 및 구성을 관리하기 위한 Maven 다중 모듈 프로젝트 만들기에 대해 설명합니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](./overview.md#local-dev-environment). 로컬에서 사용할 수 있는 Adobe Experience Manager의 새 인스턴스가 있고 추가 샘플/데모 패키지가 설치되어 있지 않은지 확인합니다(필수 서비스 팩 제외).

## 목표 {#objective}

1. Maven Archetype을 사용하여 새 AEM 프로젝트를 생성하는 방법을 알아봅니다.
1. AEM Project Archetype에서 생성한 다양한 모듈과 이러한 모듈을 함께 사용하는 방법에 대해 알아봅니다.
1. AEM 프로젝트에 AEM 핵심 구성 요소를 포함하는 방법을 이해합니다.

## 빌드할 항목 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

이 장에서는 를 사용하여 새 Adobe Experience Manager 프로젝트를 생성합니다 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). AEM 프로젝트에는 Sites 구현에 사용되는 전체 코드, 콘텐츠 및 구성이 포함됩니다. 이 장에서 생성된 프로젝트는 WKND Site 구현의 기반 역할을 하며 향후 장에서 빌드됩니다.

**Maven 프로젝트란 무엇입니까?** - [Apache Maven](https://maven.apache.org/) 는 프로젝트를 빌드하는 소프트웨어 관리 도구입니다. *모든 Adobe Experience Manager* 구현은 Maven 프로젝트를 사용하여 AEM 위에 사용자 지정 코드를 작성, 관리 및 배포합니다.

**Maven Archetype이란?** - A [Maven Archetype](https://maven.apache.org/archetype/index.html) 는 새 프로젝트를 생성하기 위한 템플릿 또는 패턴입니다. AEM Project Archetype은 사용자 정의 네임스페이스로 새 프로젝트를 생성하고 모범 사례를 따르는 프로젝트 구조를 포함하여 프로젝트 개발을 크게 가속화합니다.

## 프로젝트 만들기 {#create}

AEM용 Maven 다중 모듈 프로젝트를 제작하기 위한 두 가지 옵션이 있습니다. 이 튜토리얼에서는 [Maven AEM Project Archetype **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager도 [ui 마법사 제공](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html) AEM 응용 프로그램 프로젝트 만들기를 시작합니다. Cloud Manager UI에서 생성된 기본 프로젝트는 Archetype을 직접 사용하는 것과 동일한 구조를 생성합니다.

>[!NOTE]
>
>이 자습서에서는 버전을 사용합니다. **35** 원형 중 하나를 선택할 수 있습니다. 를 사용하는 것은 항상 모범 사례입니다. **최신** 새 프로젝트를 생성할 archetype 버전입니다.

다음 일련의 단계는 UNIX® 기반 명령줄 터미널을 사용하지만 Windows 터미널을 사용하는 경우에는 유사해야 합니다.

1. 명령줄 터미널을 엽니다. Maven이 설치되었는지 확인합니다.

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. AEM 프로젝트를 생성할 디렉토리로 이동합니다. 프로젝트의 소스 코드를 유지하려는 모든 디렉토리가 될 수 있습니다. (예: `code` 사용자의 홈 디렉터리 아래:

   ```shell
   $ cd ~/code
   ```

1. 다음 내용을 명령줄에 붙여 넣습니다. [배치 모드로 프로젝트 생성](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.14+를 타깃팅하려면 바꾸기 `aemVersion="cloud"` 포함 `aemVersion="6.5.14"`.
   >
   > 또한 항상 최신 버전을 사용하십시오 `archetypeVersion` 를 참조하여 [AEM Project Archetype > 사용](https://github.com/adobe/aem-project-archetype#usage)

   프로젝트 구성을 위해 사용 가능한 속성의 전체 목록 [은(는) 여기에서 찾을 수 있음](https://github.com/adobe/aem-project-archetype#available-properties).

1. 로컬 파일 시스템에서 Maven Archetype에 의해 다음 폴더 및 파일 구조가 생성됩니다.

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 프로젝트 배포 및 빌드 {#build}

프로젝트 코드를 빌드하고 AEM의 로컬 인스턴스에 배포합니다.

1. 포트에서 로컬로 실행 중인 AEM의 작성자 인스턴스가 있는지 확인합니다. **4502**.
1. 명령줄에서 `aem-guides-wknd` 프로젝트 디렉토리.

   ```shell
   $ cd aem-guides-wknd
   ```

1. 다음 명령을 실행하여 전체 프로젝트를 빌드하고 AEM에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   빌드는 약 1분 정도 소요되며 다음 메시지로 끝납니다.

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Maven 프로필 `autoInstallSinglePackage` 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 포트에서 로컬로 실행되는 AEM 인스턴스에 배포됩니다. **4502** 의 자격 증명으로 `admin:admin`.

1. 로컬 AEM 인스턴스의 패키지 관리자로 이동합니다. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 다음에 대한 패키지를 확인해야 합니다. `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`, 및 `aem-guides-wknd.all`.

1. 사이트 콘솔로 이동합니다. [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND 사이트 는 사이트 중 하나입니다. 여기에는 미국 및 언어 마스터 계층 구조가 있는 사이트 구조가 포함됩니다. 이 사이트 계층 구조는 의 값을 기반으로 합니다. `language_country` 및 `isSingleCountryWebsite` archetype을 사용하여 프로젝트를 생성할 때

1. 를 엽니다. **US** `>` **영어** 페이지를 선택하고 **편집** 메뉴 모음의 단추:

   ![사이트 콘솔](assets/project-setup/aem-sites-console.png)

1. 스타터 콘텐츠는 이미 만들어졌으며 여러 구성 요소를 페이지에 추가할 수 있습니다. 이러한 구성 요소를 실험하여 기능을 파악하십시오. 다음 장에서 구성 요소의 기본 사항에 대해 알아봅니다.

   ![Home Starter 콘텐츠](assets/project-setup/start-home-page.png)

   *Archetype에서 생성된 샘플 콘텐츠*

## 프로젝트 Inspect {#project-structure}

생성된 AEM 프로젝트는 각각 다른 역할을 가진 개별 Maven 모듈로 구성됩니다. 이 자습서와 대부분의 개발은 다음 모듈에 중점을 둡니다.

* [코어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java 코드, 주로 백엔드 개발자입니다.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) - 주로 프론트엔드 개발자를 위한 CSS, JavaScript, Sass, TypeScript용 소스 코드를 포함합니다.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) - 구성 요소 및 대화 상자 정의를 포함하며, 컴파일된 CSS 및 JavaScript를 클라이언트 라이브러리로 임베드합니다.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) - 편집 가능한 템플릿, 메타데이터 스키마(/content, /conf)와 같은 구조적 콘텐츠 및 구성을 포함합니다.

* **모두** - 위의 모듈을 AEM 환경에 배포할 수 있는 단일 패키지에 결합하는 빈 Maven 모듈입니다.

![Maven 프로젝트 다이어그램](assets/project-setup/project-pom-structure.png)

다음을 참조하십시오. [AEM Project Archetype 설명서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 에 대한 자세한 내용을 알아보려면 **모두** Maven 모듈.

### 핵심 구성 요소 포함 {#core-components}

[AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 는 AEM용 표준화된 웹 컨텐츠 관리(WCM) 구성 요소 세트입니다. 이러한 구성 요소는 기본 기능 세트를 제공하며 개별 프로젝트에 대해 스타일링, 사용자 정의 및 확장됩니다.

AEM as a Cloud Service 환경에는 최신 버전의 [AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 따라서 AEM에 대해 생성된 프로젝트는 as a Cloud Service으로 수행됩니다. **아님** AEM 핵심 구성 요소 임베드를 포함합니다.

AEM 6.5/6.4 생성 프로젝트의 경우 Archetype이 자동으로 임베드됩니다 [AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 을 클릭합니다. AEM 6.5/6.4에서는 최신 버전이 프로젝트와 함께 배포되도록 AEM 핵심 구성 요소를 포함하는 것이 좋습니다. 핵심 구성 요소 방법에 대한 자세한 정보 [프로젝트에 포함된 내용은 여기에서 확인하십시오.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 소스 제어 관리 {#source-control}

항상 일부 형식의 소스 제어를 사용하여 애플리케이션에서 코드를 관리하는 것이 좋습니다. 이 자습서에서는 git 및 GitHub를 사용합니다. SCM에서 무시해야 하는 Maven 및/또는 선택한 IDE에서 생성되는 여러 파일이 있습니다.

Maven은 코드 패키지를 빌드하고 설치할 때마다 대상 폴더를 생성합니다. 대상 폴더 및 콘텐츠는 SCM에서 제외되어야 합니다.

아래, `ui.apps` 모듈은 많은 `.content.xml` 파일이 만들어집니다. 이러한 XML 파일은 JCR에 설치된 컨텐츠의 노드 유형 및 속성을 매핑합니다. 이들 파일은 매우 중요하며 **할 수 없음** 무시하십시오.

AEM Project Archetype 은 샘플을 생성합니다 `.gitignore` 파일을 안전하게 무시할 수 있는 시작점으로 사용할 수 있는 파일입니다. 다음 위치에 파일이 생성됩니다. `<src>/aem-guides-wknd/.gitignore`.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM 프로젝트를 만들었습니다!

### 다음 단계 {#next-steps}

간단한 방법을 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술 이해 `HelloWorld` 예제 사용 [구성 요소 기본 사항](component-basics.md) 튜토리얼.

## 고급 Maven 명령(보너스) {#advanced-maven-commands}

개발 중에 모듈 중 하나만 사용하여 작업할 수 있으므로 시간을 절약하기 위해 전체 프로젝트를 빌드하지 않으려는 경우 AEM Publish 인스턴스 또는 포트 4502에서 실행되지 않는 AEM 인스턴스에 직접 배포할 수도 있습니다.

다음으로 개발 중에 유연성을 높이기 위해 사용할 수 있는 몇 가지 추가 Maven 프로필 및 명령을 검토해 보겠습니다.

### 핵심 모듈 {#core-module}

다음 **[코어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 모듈에는 프로젝트와 연결된 모든 Java™ 코드가 포함되어 있습니다. 빌드 **코어** 모듈이 AEM에 OSGi 번들을 배포합니다. 이 모듈만 만들려면:

1. 다음으로 이동 `core` 폴더(아래) `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. 다음 명령을 실행합니다.

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. 다음으로 이동 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). OSGi 웹 콘솔이며 AEM 인스턴스에 설치된 모든 번들에 대한 정보가 포함되어 있습니다.

1. 전환 **Id** 열을 정렬하면 WKND 번들이 설치되어 있고 활성 상태로 표시됩니다.

   ![핵심 번들](assets/project-setup/wknd-osgi-console.png)

1. 에서 Jar의 &#39;물리적&#39; 위치를 확인할 수 있습니다. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Jar의 CRXDE 위치](assets/project-setup/jcr-bundle-location.png)

### Ui.apps 및 Ui.content 모듈 {#apps-content-module}

다음 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven 모듈에는 아래 사이트에 필요한 모든 렌더링 코드가 포함됩니다 `/apps`. 다음과 같은 AEM 형식으로 저장된 CSS/JS가 여기에 포함됩니다. [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 여기에는 다음도 포함됩니다 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 동적 HTML 렌더링 스크립트. 다음을 생각할 수 있습니다. **ui.apps** 모듈은 JCR의 구조에 매핑되지만 파일 시스템에 저장되고 소스 제어에 커밋될 수 있는 형식으로 사용됩니다. 다음 **ui.apps** 모듈에는 코드만 포함됩니다.

이 모듈만 만들려면:

1. 명령줄에서 다음으로 이동 `ui.apps` 폴더(아래) `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. 다음 명령을 실행합니다.

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 다음으로 이동 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 다음이 표시됩니다. `ui.apps` 패키지를 첫 번째로 설치된 패키지로, 다른 패키지보다 최신 타임스탬프가 있어야 합니다.

   ![Ui.apps 패키지 설치됨](assets/project-setup/ui-apps-package.png)

1. 명령줄로 돌아가서 다음 명령 실행( `ui.apps` 폴더):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   프로필 `autoInstallPackagePublish` 은 포트에서 실행 중인 게시 환경에 패키지를 배포하기 위한 것입니다. **4503**. http://localhost:4503에서 실행 중인 AEM 인스턴스를 찾을 수 없는 경우 위의 오류가 발생합니다.

1. 마지막으로 다음 명령을 실행하여 를 배포합니다. `ui.apps` 포트에 있는 패키지 **4504**:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   포트에서 실행 중인 AEM 인스턴스가 없으면 빌드 오류가 다시 발생합니다 **4504** 을(를) 사용할 수 있습니다. 매개 변수 `aem.port` 는 POM 파일의 다음 위치에 정의됩니다. `aem-guides-wknd/pom.xml`.

다음 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 모듈은 와 동일한 방식으로 구성됩니다. **ui.apps** 모듈. 유일한 차이점은 **ui.content** 모듈에는 라고 하는 것이 포함되어 있습니다. **변하기 쉬워** 콘텐츠. **변경 가능** 콘텐츠는 기본적으로 소스 제어에 저장되는 템플릿, 정책 또는 폴더 구조와 같은 비코드 구성을 나타냅니다 **그러나** AEM 인스턴스에서 직접 수정할 수 있습니다. 이 내용은 페이지 및 템플릿 챕터에서 자세히 알아봅니다.

를 빌드하는 데 사용되는 것과 동일한 Maven 명령 **ui.apps** 모듈을 사용하여 **ui.content** 모듈. 위의 단계를 내에서 자유롭게 반복하십시오. **ui.content** 폴더를 삭제합니다.

## 문제 해결

AEM Project Archetype을 사용하여 프로젝트를 생성하는 데 문제가 있는 경우 다음 목록을 참조하십시오. [알려진 문제](https://github.com/adobe/aem-project-archetype#known-issues) 및 열기 목록 [문제](https://github.com/adobe/aem-project-archetype/issues).

## 다시 한번 축하드립니다! {#congratulations-bonus}

축하합니다, 보너스 재료를 검토해 보았습니다.

### 다음 단계 {#next-steps-bonus}

간단한 방법을 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술 이해 `HelloWorld` 예제 사용 [구성 요소 기본 사항](component-basics.md) 튜토리얼.
