---
title: AEM Sites 시작하기 - 프로젝트 설정
description: Experience Manager 사이트에 대한 코드 및 구성을 관리하려면 Maven 다중 모듈 프로젝트를 만드십시오.
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '1839'
ht-degree: 4%

---

# 프로젝트 설정 {#project-setup}

이 자습서에서는 Adobe Experience Manager 사이트에 대한 코드 및 구성을 관리하기 위한 Maven 다중 모듈 프로젝트 만들기 를 다룹니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](./overview.md#local-dev-environment). 로컬에서 사용할 수 있는 Adobe Experience Manager의 새 인스턴스가 있고 추가 샘플/데모 패키지가 설치되어 있지 않은지(필수 서비스 팩 제외) 확인하십시오.

## 목표 {#objective}

1. Maven 원형 을 사용하여 새 AEM 프로젝트를 생성하는 방법을 알아봅니다.
1. AEM Project Archetype에서 생성한 다양한 모듈과 이러한 모듈이 어떻게 함께 작동하는지를 이해합니다.
1. AEM 핵심 구성 요소가 AEM 프로젝트에 포함되는 방법을 이해합니다.

## 빌드할 내용 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

이 장에서는 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype). AEM 프로젝트에는 Sites 구현에 사용되는 전체 코드, 콘텐츠 및 구성이 포함되어 있습니다. 이 장에서 생성된 프로젝트는 WKND Site의 구현 기반 역할을 하며, 향후 장에 구축됩니다.

**Maven 프로젝트란?** - [Apache Maven](https://maven.apache.org/) 는 프로젝트를 빌드하기 위한 소프트웨어 관리 도구입니다. *모든 Adobe Experience Manager* 구현에서는 Maven 프로젝트를 사용하여 AEM 맨 위에 사용자 지정 코드를 작성, 관리 및 배포할 수 있습니다.

**Maven 원형은 무엇입니까?** - A [Maven 원형](https://maven.apache.org/archetype/index.html) 는 새 프로젝트를 생성하기 위한 템플릿 또는 패턴입니다. AEM 프로젝트 원형은 사용자 지정 네임스페이스로 새 프로젝트를 생성하고 우수 사례를 따르는 프로젝트 구조를 포함하므로 프로젝트 개발을 크게 가속화합니다.

## 프로젝트를 만듭니다 {#create}

AEM용 Maven 다중 모듈 프로젝트를 만드는 두 가지 옵션이 있습니다. 이 자습서에서는 [Maven AEM 프로젝트 원형 **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager도 [ui 마법사 제공](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html) AEM 응용 프로그램 프로젝트 생성을 시작하기 위해 Cloud Manager UI에서 생성한 기본 프로젝트는 원형 그대로 사용하는 구조와 같습니다.

>[!NOTE]
>
>이 자습서에서는 버전을 사용합니다 **35** 원형. 항상 을 사용하는 것이 가장 좋습니다 **최신** 새 프로젝트를 생성할 원형 버전입니다.

다음 일련의 단계는 UNIX® 기반 명령줄 터미널을 사용하여 수행되지만 Windows 터미널을 사용하는 경우 유사해야 합니다.

1. 명령줄 터미널을 엽니다. Maven이 설치되어 있는지 확인합니다.

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. AEM 프로젝트를 생성할 디렉토리로 이동합니다. 이 디렉토리는 프로젝트의 소스 코드를 유지 관리할 모든 디렉토리일 수 있습니다. 예를 들어 이름이 인 디렉토리는 `code` 사용자의 홈 디렉토리 아래

   ```shell
   $ cd ~/code
   ```

1. 다음 내용을 명령줄에서 [일괄 처리 모드에서 프로젝트 생성](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

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
   > AEM 6.5.14+ 바꾸기를 타깃팅하려면 다음을 수행하십시오 `aemVersion="cloud"` with `aemVersion="6.5.14"`.
   >
   > 또한 항상 최신 `archetypeVersion` 을 참조하여 [AEM 프로젝트 원형 > 사용](https://github.com/adobe/aem-project-archetype#usage)

   프로젝트 구성에 사용 가능한 속성의 전체 목록입니다 [여기에서 찾을 수 있습니다.](https://github.com/adobe/aem-project-archetype#available-properties).

1. 다음 폴더 및 파일 구조는 로컬 파일 시스템의 Maven 원형에 의해 생성됩니다.

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

프로젝트 코드를 작성하고 AEM의 로컬 인스턴스에 배포합니다.

1. 포트에서 로컬로 실행되는 AEM의 작성자 인스턴스가 있는지 확인합니다. **4502년**.
1. 명령줄에서 `aem-guides-wknd` 프로젝트 디렉터리입니다.

   ```shell
   $ cd aem-guides-wknd
   ```

1. 다음 명령을 실행하여 전체 프로젝트를 AEM에 빌드 및 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   빌드는 약 1분 정도 걸리며 다음 메시지로 끝나야 합니다.

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

   Maven 프로필 `autoInstallSinglePackage` 프로젝트의 개별 모듈을 컴파일하고 단일 패키지를 AEM 인스턴스에 배포합니다. 기본적으로 이 패키지는 포트에서 로컬로 실행되는 AEM 인스턴스에 배포됩니다 **4502년** 그리고 `admin:admin`.

1. 로컬 AEM 인스턴스의 패키지 관리자로 이동합니다. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 에 대한 패키지가 표시됩니다. `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`, 및 `aem-guides-wknd.all`.

1. 사이트 콘솔로 이동합니다. [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND 사이트는 사이트 중 하나입니다. 여기에는 미국 및 언어 마스터 계층 구조의 사이트 구조가 포함됩니다. 이 사이트 계층 은 `language_country` 및 `isSingleCountryWebsite` 원형 을 사용하여 프로젝트를 생성하는 경우

1. 를 엽니다. **미국** `>` **영어** 페이지를 선택하고 **편집** 메뉴 모음의 단추:

   ![사이트 콘솔](assets/project-setup/aem-sites-console.png)

1. 스타터 컨텐츠가 이미 만들어졌고 페이지에 여러 구성 요소를 추가할 수 있습니다. 이러한 구성 요소를 실험하여 기능에 대한 아이디어를 얻습니다. 다음 장에서는 구성 요소의 기본 사항을 학습합니다.

   ![홈 스타터 콘텐츠](assets/project-setup/start-home-page.png)

   *Archetype에서 생성된 샘플 컨텐츠*

## Inspect 프로젝트 {#project-structure}

생성된 AEM 프로젝트는 각각 다른 역할을 가진 개별 Maven 모듈로 구성됩니다. 이 자습서와 대부분의 개발에서는 다음 모듈에 중점을 둡니다.

* [코어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java 코드, 주로 백엔드 개발자입니다.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) - CSS, JavaScript, Sas, TypeScript에 대한 소스 코드를 주로 프런트엔드 개발자에게 포함합니다.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) - 구성 요소 및 대화 상자 정의를 포함하고, 컴파일된 CSS 및 JavaScript를 클라이언트 라이브러리로 포함합니다.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) - 편집 가능한 템플릿, 메타데이터 스키마(/content, /conf)와 같은 구조적 컨텐츠 및 구성을 포함합니다.

* **모두** - 위의 모듈을 AEM 환경에 배포할 수 있는 단일 패키지에 결합하는 빈 Maven 모듈입니다.

![Maven 프로젝트 다이어그램](assets/project-setup/project-pom-structure.png)

자세한 내용은 [AEM 프로젝트 원형 설명서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 의 세부 사항을 자세히 알아보기 **모두** maven 모듈입니다.

### 핵심 구성 요소 포함 {#core-components}

[AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 는 AEM용 WCM(표준화된 웹 컨텐츠 관리) 구성 요소 세트입니다. 이러한 구성 요소는 기능의 기본 세트를 제공하며 개별 프로젝트에 대해 스타일 지정, 사용자 지정 및 확장됩니다.

AEM as a Cloud Service 환경에는 최신 버전의 [AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko). 따라서 AEM as a Cloud Service에 대해 생성된 프로젝트 **not** AEM 코어 구성 요소 포함.

AEM 6.5/6.4 생성 프로젝트의 경우 원형 이 자동으로 포함됩니다 [AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko) 를 입력합니다. AEM 6.5/6.4에서 AEM 코어 구성 요소를 포함하여 최신 버전이 프로젝트와 함께 배포되도록 하는 것이 좋습니다. 핵심 구성 요소 구성 요소에 대한 자세한 정보 [프로젝트에 포함된 내용은 여기에서 확인할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 소스 제어 관리 {#source-control}

응용 프로그램에서 코드를 관리하는 데 소스 제어 형식을 사용하는 것이 항상 좋습니다. 이 자습서에서는 Git 및 GitHub를 사용합니다. SCM에서 무시해야 하는 Maven 및/또는 선택한 IDE에서 생성되는 몇 가지 파일이 있습니다.

Maven은 코드 패키지를 작성하고 설치할 때마다 대상 폴더를 만듭니다. 대상 폴더 및 콘텐츠는 SCM에서 제외해야 합니다.

아래에 있는 `ui.apps` 모듈은 많은 것을 관찰합니다 `.content.xml` 파일이 만들어집니다. 이러한 XML 파일은 JCR에 설치된 컨텐츠의 노드 유형과 속성을 매핑합니다. 이러한 파일은 매우 중요하며 **사용할 수 없음** 무시하십시오.

AEM 프로젝트 원형은 샘플을 생성합니다 `.gitignore` 파일을 안전하게 무시할 수 있는 시작점으로 사용할 수 있는 파일입니다. 파일은 다음 위치에 생성됩니다 `<src>/aem-guides-wknd/.gitignore`.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM 프로젝트를 만들었습니다!

### 다음 단계 {#next-steps}

간단한 방법을 통해 AEM(Adobe Experience Manager) 사이트 구성 요소의 기본 기술을 이해합니다 `HelloWorld` 예 [구성 요소 기본 사항](component-basics.md) 자습서입니다.

## 고급 Maven 명령(보너스) {#advanced-maven-commands}

개발 중에는 모듈 중 하나만으로 작업하고 있으므로 시간을 절약하기 위해 전체 프로젝트를 빌드하지 않으려고 할 수 있습니다. AEM 게시 인스턴스에 직접 배포하거나 포트 4502에서 실행되지 않는 AEM 인스턴스에 배포할 수도 있습니다.

다음으로 개발 중에 보다 유연하게 사용할 수 있는 몇 가지 추가 Maven 프로필 및 명령을 살펴보겠습니다.

### 코어 모듈 {#core-module}

다음 **[코어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 모듈에는 프로젝트와 연결된 모든 Java™ 코드가 포함되어 있습니다. 빌드 **코어** 모듈이 AEM에 OSGi 번들을 배포합니다. 이 모듈만 빌드하려면

1. 로 이동합니다. `core` 폴더(아래) `aem-guides-wknd`):

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

1. 전환 **Id** 열을 정렬하면 WKND 번들이 설치 및 활성 상태로 표시됩니다.

   ![코어 번들](assets/project-setup/wknd-osgi-console.png)

1. 여기에서 jar의 &#39;물리적&#39; 위치를 볼 수 있습니다 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Jar의 CRXDE 위치](assets/project-setup/jcr-bundle-location.png)

### Ui.apps 및 Ui.content 모듈 {#apps-content-module}

다음 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven 모듈에는 아래의 사이트에 필요한 모든 렌더링 코드가 포함되어 있습니다 `/apps`. 여기에는 다음과 같은 AEM 형식으로 저장된 CSS/JS가 포함됩니다 [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 여기에는 다음과 같은 항목이 포함되어 있습니다 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 동적 HTML 렌더링용 스크립트 이 경우 **ui.apps** 모듈이 JCR의 구조에 매핑되지만 파일 시스템에 저장하고 소스 제어에 커밋할 수 있는 형식으로 매핑됩니다. 다음 **ui.apps** 모듈에는 코드만 포함되어 있습니다.

이 모듈만 빌드하려면

1. 명령줄에서 로 이동합니다. `ui.apps` 폴더(아래) `aem-guides-wknd`):

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

1. 다음으로 이동 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 다음을 볼 수 있습니다. `ui.apps` 패키지를 처음 설치한 패키지로 사용하고 다른 패키지와 비교하여 최신 타임스탬프가 있어야 합니다.

   ![Ui.apps 패키지가 설치됨](assets/project-setup/ui-apps-package.png)

1. 명령줄로 돌아가서 다음 명령을 실행합니다 `ui.apps` 폴더):

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

   프로필 `autoInstallPackagePublish` 은 포트에서 실행 중인 게시 환경에 패키지를 배포하기 위한 것입니다 **4503년**. http://localhost:4503에서 실행 중인 AEM 인스턴스를 찾을 수 없는 경우 위의 오류가 발생합니다.

1. 마지막으로 다음 명령을 실행하여 `ui.apps` 포트에서 패키지 **4504년**:

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

   포트에서 실행 중인 AEM 인스턴스가 없을 경우 다시 빌드 오류가 발생합니다 **4504년** 사용할 수 있습니다. 매개 변수 `aem.port` 의 POM 파일에 정의됩니다 `aem-guides-wknd/pom.xml`.

다음 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 모듈은 과 같은 방식으로 구조화됩니다 **ui.apps** 모듈. 유일한 차이점은 **ui.content** 모듈에는 **가변 가능** 컨텐츠 배포. **가변 가능** 컨텐츠는 기본적으로 소스 제어에 저장된 템플릿, 정책 또는 폴더 구조와 같은 비코드 구성을 나타냅니다 **그러나** AEM 인스턴스에서 직접 수정할 수 있습니다. 이는 페이지 및 템플릿 장의 보다 자세한 내용을 살펴봅니다.

Maven 명령을 빌드하는 데 사용되는 것과 동일한 **ui.apps** 모듈을 사용하여 **ui.content** 모듈. 에서 위의 단계를 자유롭게 반복할 수 있습니다. **ui.content** 폴더를 입력합니다.

## 문제 해결

AEM Project Archetype을 사용하여 프로젝트를 생성하는 데 문제가 있는 경우 [알려진 문제](https://github.com/adobe/aem-project-archetype#known-issues) 및 열기 목록 [문제](https://github.com/adobe/aem-project-archetype/issues).

## 다시 축하합니다! {#congratulations-bonus}

축하합니다, 보너스 자료를 검토하겠습니다.

### 다음 단계 {#next-steps-bonus}

간단한 방법을 통해 AEM(Adobe Experience Manager) 사이트 구성 요소의 기본 기술을 이해합니다 `HelloWorld` 예 [구성 요소 기본 사항](component-basics.md) 자습서입니다.
