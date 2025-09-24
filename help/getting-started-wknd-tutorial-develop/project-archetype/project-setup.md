---
title: AEM Sites 시작하기 - 프로젝트 설정
description: Maven 다중 모듈 프로젝트를 제작하여 Experience Manager Site의 코드 및 구성을 관리할 수 있습니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
duration: 502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1695'
ht-degree: 100%

---

# 프로젝트 설정 {#project-setup}

이 튜토리얼에서는 Adobe Experience Manager 사이트의 코드와 구성을 관리하기 위한 Maven 다중 모듈 프로젝트를 만드는 방법을 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](./overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다. 로컬에서 Adobe Experience Manager의 최신 인스턴스를 사용할 수 있는지 확인하고 필요한 서비스 팩 외에 추가 샘플/데모 패키지가 설치되지 않은 상태인지 확인합니다.

## 목표 {#objective}

1. Maven Archetype을 사용하여 새로운 AEM 프로젝트를 생성하는 방법을 알아봅니다.
1. AEM Project Archetype이 생성한 다양한 모듈과 모듈이 함께 작동하는 방식을 알아봅니다.
1. AEM 핵심 구성 요소가 AEM Project에 어떻게 포함되는지 알아봅니다.

## 빌드 결과물 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

이 챕터에서는 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을 사용하여 새로운 Adobe Experience Manager 프로젝트를 생성합니다. AEM 프로젝트에는 사이트 구현에 사용되는 완전한 코드, 콘텐츠 및 구성이 포함되어 있습니다. 이 챕터에서 생성되는 프로젝트는 WKND 사이트 구현의 기초가 되며 이후 챕터에서 확장됩니다.

**Maven 프로젝트란?** - [Apache Maven](https://maven.apache.org/)은 프로젝트를 빌드하는 소프트웨어 관리 도구입니다. *모든 Adobe Experience Manager* 구현은 Maven 프로젝트를 사용하여 AEM을 기반으로 사용자 정의 코드를 빌드, 관리 및 배포합니다.

**Maven Archetype이란?** - [Maven Archetype](https://maven.apache.org/archetype/index.html)은 새로운 프로젝트를 생성하기 위한 템플릿 또는 패턴입니다. AEM Project Archetype은 사용자 정의 네임스페이스를 갖춘 새로운 프로젝트를 생성하고 모범 사례를 따르는 프로젝트 구조를 포함하는 데 도움이 되므로 프로젝트 개발을 대폭 가속화합니다.

## 프로젝트 만들기 {#create}

AEM을 위한 Maven 다중 모듈 프로젝트를 만드는 경우 몇 가지 옵션이 있습니다. 이 튜토리얼에서는 [Maven AEM Project Archetype **35**](https://github.com/adobe/aem-project-archetype)를 사용합니다. Cloud Manager는 AEM 애플리케이션 프로젝트 생성을 시작하기 위한 [UI 마법사를 제공](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=ko)합니다. Cloud Manager UI에서 생성된 기본 프로젝트는 Archetype을 직접 사용하는 경우와 동일한 구조를 갖습니다.

>[!NOTE]
>
>이 튜토리얼에서는 Archetype **35** 버전을 사용합니다. 새 프로젝트를 생성할 때는 항상 **최신** 버전의 Archetype을 사용하는 것이 가장 좋습니다.

다음 단계는 Unix® 기반 명령줄 터미널을 사용하여 진행되나, Windows 터미널을 사용하는 경우에도 비슷하게 진행됩니다.

1. 명령줄 터미널을 엽니다. Maven이 설치되었는지 확인합니다.

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. AEM 프로젝트를 생성하려는 디렉터리로 이동합니다. 프로젝트의 소스 코드를 유지 관리하려는 디렉터리라면, 어떤 디렉터리라도 상관없습니다. 예를 들어 사용자의 홈 디렉터리 아래에 `code`라는 디렉터리가 있습니다.

   ```shell
   $ cd ~/code
   ```

1. 다음을 명령줄에 붙여넣어 [배치 모드로 프로젝트를 생성](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)합니다.

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
   > AEM 6.5.14+를 타기팅하려면 `aemVersion="cloud"`를 `aemVersion="6.5.14"`로 바꿉니다.
   >
   > 또한 [AEM Project Archetype > 사용](https://github.com/adobe/aem-project-archetype#usage)을 참조하여 항상 최신 `archetypeVersion`을 사용합니다.

   프로젝트 구성을 위해 사용 가능한 속성의 전체 목록은 [여기에서 확인](https://github.com/adobe/aem-project-archetype#available-properties)할 수 있습니다.

1. 다음 폴더와 파일 구조는 로컬 파일 시스템의 Maven Archetype에 의해 생성됩니다.

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

프로젝트 코드를 AEM의 로컬 인스턴스에 빌드하고 배포합니다.

1. **4502** 포트에서 로컬로 실행되는 AEM의 작성자 인스턴스가 있는지 확인합니다.
1. 명령줄에서 `aem-guides-wknd` 프로젝트 디렉터리로 이동합니다.

   ```shell
   $ cd aem-guides-wknd
   ```

1. 다음 명령을 실행하여 전체 프로젝트를 빌드하고 AEM에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   빌드는 약 1분 정도 소요되며 다음 메시지와 함께 종료됩니다.

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

   Maven 프로필인 `autoInstallSinglePackage`는 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 **4502** 포트에서 로컬로 실행되는 AEM 인스턴스에 배포되며 `admin:admin`의 자격 증명을 보유합니다.

1. 로컬 AEM 인스턴스인 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)에서 패키지 관리자로 이동합니다. 그러면 `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`, `aem-guides-wknd.all`에 대한 패키지를 볼 수 있습니다.

1. 사이트 콘솔 [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)로 이동합니다. 이러한 사이트 중 하나가 WKND 사이트입니다. 여기에는 US 및 언어 마스터 계층이 있는 사이트 구조가 포함됩니다. 이 사이트 계층은 Archetype을 사용하여 프로젝트를 생성할 때 `language_country` 및 `isSingleCountryWebsite`에 대한 값을 기반으로 합니다.

1. **US** `>` **영어** 페이지를 선택하여 열고 메뉴 바에서 **편집** 버튼을 클릭합니다.

   ![사이트 콘솔](assets/project-setup/aem-sites-console.png)

1. 스타터 콘텐츠는 이미 생성되었으며, 페이지에 추가할 수 있는 여러 구성 요소가 있습니다. 이러한 구성 요소를 실험해 보면 기능을 파악할 수 있습니다. 다음 챕터에서는 구성 요소의 기본 사항을 알아봅니다.

   ![홈 스타터 콘텐츠](assets/project-setup/start-home-page.png)

   *Archetype에서 생성된 샘플 콘텐츠*

## 프로젝트 검사 {#project-structure}

생성된 AEM 프로젝트는 각각 다른 역할을 가진 개별 Maven 모듈로 구성됩니다. 이 튜토리얼과 대부분의 개발은 다음 모듈에 중점을 둡니다.

* [핵심](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=ko) - Java 코드, 주로 백엔드 개발자입니다.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko) - 주로 프론트엔드 개발자를 위한 CSS, JavaScript, Sass, TypeScript의 소스 코드를 포함합니다.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=ko) - 구성 요소 및 대화 상자 정의를 포함하고, 컴파일된 CSS와 JavaScript를 클라이언트 라이브러리로 임베드합니다.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=ko) - 편집 가능한 템플릿, 메타데이터 스키마(/content, /conf)와 같은 구조적 콘텐츠와 구성을 포함합니다.

* **모두** - 위의 모듈을 AEM 환경에 배포할 수 있는 단일 패키지로 결합한 빈 Maven 모듈입니다.

![Maven 프로젝트 다이어그램](assets/project-setup/project-pom-structure.png)

Maven 모듈의 **모든** 세부 정보를 자세히 알아보려면 [AEM Project Archetype 문서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)를 참조하시기 바랍니다.

### 핵심 구성 요소 포함 {#core-components}

[AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)는 AEM을 위한 표준화된 웹 콘텐츠 관리(WCM) 구성 요소 집합입니다. 이러한 구성 요소는 기능의 기준 세트를 제공하며 개별 프로젝트에 맞게 스타일이 지정, 사용자 정의 및 확장됩니다.

AEM as a Cloud Service 환경은 [AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)의 최신 버전을 포함합니다. 따라서 AEM as a Cloud Service를 위해 생성된 프로젝트는 임베드된 AEM 핵심 구성 요소를 포함하지 **않습니다**.

AEM 6.5/6.4에서 생성된 프로젝트의 경우, Archetype은 프로젝트에 자동으로 [AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)를 임베드합니다. AEM 6.5/6.4의 경우 최신 버전이 프로젝트에 배포되도록 AEM 핵심 구성 요소를 임베드하는 것이 가장 좋습니다. 핵심 구성 요소가 어떻게 [프로젝트에 포함되는지에 관한 자세한 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=ko#core-components)에서 확인할 수 있습니다.

## 소스 제어 관리 {#source-control}

애플리케이션의 코드를 관리하기 위해 일종의 소스 제어를 사용하는 방법은 항상 권장됩니다. 이 튜토리얼에서는 git과 GitHub를 사용합니다. Maven 및/또는 선택한 IDE에서 생성되는 여러 파일 중 SCM에서 무시해야 하는 파일이 몇 개 있습니다.

Maven은 코드 패키지를 빌드하고 설치할 때마다 타깃 폴더를 만듭니다. 타깃 폴더와 내용은 SCM에서 제외되어야 합니다.

`ui.apps` 모듈에서는 `.content.xml` 파일이 다수 생성되는 것을 확인합니다. 이러한 XML 파일은 JCR에 설치된 콘텐츠의 노드 유형과 속성을 매핑합니다. 이 파일들은 중요하므로 무시하면 **안 됩니다**.

AEM Project Archetype은 파일을 안전하게 무시할 수 있는 시작점으로 사용할 수 있는 `.gitignore` 샘플 파일을 생성합니다. 해당 파일은 `<src>/aem-guides-wknd/.gitignore`에서 생성됩니다.

## 축하합니다! {#congratulations}

축하합니다! 첫 번째 AEM Project를 만들었습니다.

### 다음 단계 {#next-steps}

[구성 요소 기본 사항](component-basics.md) 튜토리얼에서 간단한 `HelloWorld` 예제를 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술을 이해합니다.

## 고급 Maven 명령 (보너스) {#advanced-maven-commands}

개발 과정에서 특정 모듈 하나만 작업 중일 때, 시간을 절약하기 위해 전체 프로젝트 빌드를 피하고 싶으실 수 있습니다. 또한 AEM 게시 인스턴스에 직접 배포하거나 포트 4502에서 실행되지 않는 AEM 인스턴스에 배포하고 싶으실 수 있습니다.

다음으로 개발 중에 유연성을 높이기 위해 사용할 수 있는 몇 가지 추가 Maven 프로필과 명령을 살펴보겠습니다.

### 핵심 모듈 {#core-module}

**[핵심](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=ko)** 모듈에는 프로젝트와 관련된 모든 Java™ 코드가 포함되어 있습니다. **핵심** 모듈을 빌드하면 OSGi 번들이 AEM에 배포됩니다. 이 모듈만 빌드하려면 다음 단계를 따르십시오.

1. `aem-guides-wknd` 아래의 `core` 폴더로 이동합니다.

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

1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)로 이동합니다. 이는 OSGi 웹 콘솔로, AEM 인스턴스에 설치된 모든 번들에 대한 정보를 담고 있습니다.

1. **Id** 정렬 열을 토글하면 WKND 번들이 설치되어 활성 상태인 것을 확인할 수 있습니다.

   ![핵심 번들](assets/project-setup/wknd-osgi-console.png)

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)에서 jar의 &#39;물리적&#39; 위치를 확인할 수 있습니다.

   ![Jar의 CRXDE 위치](assets/project-setup/jcr-bundle-location.png)

### Ui.apps 및 Ui.content 모듈 {#apps-content-module}

**[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=ko)** Maven 모듈에는 `/apps` 아래의 사이트에 필요한 모든 렌더링 코드가 포함됩니다. [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko) 라는 AEM 포맷으로 저장되는 CSS/JS가 여기에 포함됩니다. 동적 HTML 렌더링에 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=ko) 스크립트가 포함됩니다. **ui.apps** 모듈은 JCR의 구조에 대한 맵으로 간주될 수 있지만 파일 시스템에 저장되고 소스 제어 전용인 포맷으로 간주될 수도 있습니다. **ui.apps** 모듈에는 코드만 포함되어 있습니다.

이 모듈만 빌드하려면 다음 단계를 따르십시오.

1. 명령줄에서 시작합니다. `aem-guides-wknd` 아래의 `ui.apps` 폴더로 이동합니다.

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

1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)로 이동합니다. `ui.apps` 패키지가 첫 번째로 설치된 패키지로 표시되며 다른 패키지보다 더 최신인 타임스탬프를 갖습니다.

   ![Ui.apps 패키지 설치 완료](assets/project-setup/ui-apps-package.png)

1. 명령줄로 돌아가서 `ui.apps` 폴더 내에서 다음 명령을 실행합니다.

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

   `autoInstallPackagePublish` 프로필은 **4503** 포트에서 실행되는 게시 환경에 패키지를 배포하기 위한 것입니다. http://localhost:4503에서 실행되는 AEM 인스턴스를 찾을 수 없는 경우 위의 오류가 발생할 것으로 예상됩니다.

1. 마지막으로 다음 명령을 실행하여 **4504** 포트에 `ui.apps` 패키지를 배포합니다.

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

   이 경우에도 **4504** 포트에서 실행 중인 AEM 인스턴스가 없는 경우 빌드 실패가 발생할 것으로 예상됩니다. `aem.port` 매개변수는 `aem-guides-wknd/pom.xml`에 위치한 POM 파일에 정의되어 있습니다.

**[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=ko)** 모듈은 **ui.apps** 모듈과 동일한 방식으로 구성됩니다. 유일한 차이점은 **ui.content** 모듈에는 **변경 가능한** 것으로 알려진 콘텐츠가 포함되어 있다는 것입니다. **변경 가능한** 콘텐츠는 기본적으로 source-control에 저장되지&#x200B;**만** AEM 인스턴스에서 직접 수정할 수 없는 템플릿, 정책 또는 폴더 구조 등의 비코드 구성을 지칭합니다. 관련 내용은 페이지 및 템플릿 챕터에서 더 자세히 살펴보겠습니다.

**ui.apps** 모듈을 빌드하는 데 사용되는 Maven 명령은 **ui.content** 모듈을 빌드하는 데 사용될 수도 있습니다. **ui.content** 폴더 내에서 위의 단계를 반복할 수도 있습니다.

## 문제 해결

AEM Project Archetype을 사용하여 프로젝트를 생성하는 데 문제가 있는 경우, [알려진 문제](https://github.com/adobe/aem-project-archetype#known-issues) 리스트 및 공개된 [문제](https://github.com/adobe/aem-project-archetype/issues) 목록을 확인하시기 바랍니다.

## 다시 한번 축하드립니다! {#congratulations-bonus}

축하드립니다! 보너스 자료를 모두 살펴보셨습니다.

### 다음 단계 {#next-steps-bonus}

[구성 요소 기본 사항](component-basics.md) 튜토리얼에서 간단한 `HelloWorld` 예제를 통해 Adobe Experience Manager(AEM) Sites 구성 요소의 기본 기술을 이해합니다.
