---
title: AEM as a Cloud Service 개발을 위한 개발 도구 설정
description: 로컬에서 AEM에 대해 개발하는 데 필요한 모든 기준선 도구를 사용하여 로컬 개발 시스템을 설정합니다.
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3592
source-git-commit: 23ae098a1fcb2e6c47fee30f6f45b10e7ff97824
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 5%

---

# 개발 도구 설정 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="개발 도구 설정"
>abstract="AEM(Adobe Experience Manager) 개발을 위해 최소한의 개발 도구 세트를 설치하고 개발자 시스템에 설정해야 합니다. 이러한 도구로는 Java, Maven, Adobe I/O CLI, 개발 IDE 등이 있습니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ko-KR" text="개발 지침"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="개발 기본 사항"

AEM(Adobe Experience Manager) 개발을 위해 최소한의 개발 도구 세트를 설치하고 개발자 시스템에 설정해야 합니다. 이러한 도구는 AEM 프로젝트의 개발 및 빌드를 지원합니다.

참고: `~` 는 사용자 디렉토리의 축약으로 사용됩니다. Windows에서 이는 `%HOMEPATH%`.

## Java 설치

Experience Manager은 Java 애플리케이션이므로 개발 및 AEM as a Cloud Service SDK를 지원하려면 Java SDK가 필요합니다.

1. [최신 릴리스 Java 11 SDK 다운로드 및 설치](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 다음 명령을 실행하여 Oracle Java 11 SDK가 설치되어 있는지 확인합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB 리눅스]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Homebrew 설치

_Homebrew의 사용은 선택 사항이지만 권장됩니다._

Homebrew는 macOS, Windows 및 Linux용 오픈 소스 패키지 관리자입니다. 모든 지원 도구를 별도로 설치할 수 있으며, Homebrew는 Experience Manager 개발에 필요한 다양한 개발 도구를 편리하게 설치하고 업데이트할 수 있는 방법을 제공합니다.

1. 터미널 열기
1. 다음 명령을 실행하여 Homebrew가 이미 설치되어 있는지 확인합니다. `brew --version`.
1. Homebrew가 설치되지 않은 경우 Homebrew를 설치합니다.

>[!BEGINTABS]

>[!TAB macOS]

[홈브루 온 macOS](https://brew.sh/) 필수 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 또는 [명령줄 도구](https://developer.apple.com/download/more/), 다음 명령을 통해 설치 가능:

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Windows 10에 Homebrew 설치](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB 리눅스]

[Linux에 Homebrew 설치](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. 다음 명령을 실행하여 Homebrew가 설치되었는지 확인합니다. `brew --version`

![홈브루](./assets/development-tools/homebrew.png)

Homebrew를 사용하는 경우 다음을 따르십시오. __Homebrew를 사용하여 설치__ 아래 섹션의 지침 다음과 같은 경우 __아님__ homebrew를 사용하여 OS별 링크를 사용하여 도구를 설치합니다.

## Git 설치

[Git](https://git-scm.com/) 은(는) 다음에서 사용하는 소스 제어 관리 시스템입니다. [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html), 따라서 개발에 필요합니다.

>[!BEGINTABS]

>[!TAB Homebrew를 사용하여 Git 설치]

1. 터미널/명령 프롬프트 열기
1. 다음 명령을 실행합니다. `$ brew install git`
1. 다음 명령을 사용하여 Git이 설치되었는지 확인합니다. `$ git --version`

>[!TAB Git 다운로드 및 설치]

1. [Git 다운로드 및 설치](https://git-scm.com/downloads)
1. 터미널/명령 프롬프트 열기
1. 다음 명령을 사용하여 Git이 설치되었는지 확인합니다. `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Node.js(및 npm) 설치{#node-js}

[Node.js](https://nodejs.org) 는 AEM 프로젝트의 프론트엔드 에셋으로 작업하는 데 사용되는 JavaScript 런타임 환경입니다. __ui.frontend__ 하위 프로젝트. Node.js는 [npm](https://www.npmjs.com/)는 JavaScript 종속성을 관리하는 데 사용되는 실질적인 Node.js 패키지 관리자입니다.

>[!BEGINTABS]

>[!TAB Homebrew를 사용하여 Node.js 설치]

1. 터미널/명령 프롬프트 열기
1. 다음 명령을 실행합니다. `$ brew install node`
1. 다음 명령을 사용하여 Node.js가 설치되었는지 확인합니다. `$ node -v`
1. 다음 명령을 사용하여 npm이 설치되었는지 확인합니다. `$ npm -v`

>[!TAB Node.js 다운로드 및 설치]

1. [Node.js 다운로드 및 설치](https://nodejs.org/en/download/)
2. 터미널/명령 프롬프트 열기
3. 다음 명령을 사용하여 Node.js가 설치되었는지 확인합니다. `$ node -v`
4. 다음 명령을 사용하여 npm이 설치되었는지 확인합니다. `$ npm -v`

>[!ENDTABS]

![Node.js 및 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)-기반 AEM Projects는 빌드 시 Node.js의 격리된 버전을 설치합니다. AEM Maven 프로젝트의 Reactor pom.xml에 지정된 Node.js 및 npm 버전과 로컬 개발 시스템의 버전을 동기화하거나 가까운 상태로 유지하는 것이 좋습니다.
>
>이 예제 참조 [AEM 프로젝트 반응기 pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 를 사용하여 Node.js 및 npm 빌드 버전을 찾을 수 있습니다.

## Maven 설치

Apache Maven은 AEM Project Maven Archetype에서 생성된 AEM 프로젝트를 빌드하는 데 사용되는 오픈 소스 Java 명령줄 툴입니다. 모든 주요 IDE([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio 코드](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/)등) 통합된 Maven 지원


>[!BEGINTABS]

>[!TAB Homebrew를 사용하여 Maven 설치]

1. 터미널/명령 프롬프트 열기
1. 다음 명령을 실행합니다. `$ brew install maven`
1. 다음 명령을 사용하여 Maven이 설치되었는지 확인합니다. `$ mvn -v`

>[!TAB Maven 다운로드 및 설치]

1. [Maven 다운로드](https://maven.apache.org/download.cgi)
1. [Maven 설치](https://maven.apache.org/install.html)
1. 터미널/명령 프롬프트 열기
1. 다음 명령을 사용하여 Maven이 설치되었는지 확인합니다. `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Adobe I/O CLI 설정{#aio-cli}

다음 [ADOBE I/O CLI](https://github.com/adobe/aio-cli), 또는 `aio`는 다음을 포함한 다양한 Adobe 서비스에 대한 명령줄 액세스를 제공합니다. [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 및 [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O AEM CLI는 개발자에게 다음과 같은 기능을 제공하므로 as a Cloud Service에서 개발에 필수적인 역할을 합니다.

+ AEM as a Cloud Service 서비스의 테일 로그
+ CLI에서 Cloud Manager 파이프라인 관리
+ 배포 대상 [AEM 신속한 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### Adobe I/O CLI 설치

1. 확인 [Node.js가 설치되었습니다.](#node-js) Adobe I/O CLI는 npm 모듈이므로
   + 실행 `node --version` 확인하려면
1. 실행 `npm install -g @adobe/aio-cli` 을(를) 설치하려면 `aio` npm 모듈 전역

### Adobe I/O CLI Cloud Manager 플러그인 설정{#aio-cloud-manager}

Adobe I/O Cloud Manager 플러그인을 사용하면 aio CLI가 다음을 통해 Adobe Cloud Manager와 상호 작용할 수 있습니다. `aio cloudmanager` 명령입니다.

1. 실행 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 을(를) 설치하려면 [aio Cloud Manager 플러그인](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Adobe I/O CLI 인증 설정

Adobe I/O CLI가 Cloud Manager와 통신하려면 [Cloud Manager 통합은 Adobe I/O 콘솔에서 생성해야 합니다.](https://github.com/adobe/aio-cli-plugin-cloudmanager)을(를) 성공적으로 인증하려면 및 자격 증명을 가져와야 합니다.

1. 에 로그인 [console.adobe.io](https://console.adobe.io)
1. 연결할 Cloud Manager 제품이 포함된 조직이 Adobe 조직 전환기에서 활성화되어 있는지 확인합니다.
1. 새로 만들기 또는 기존 열기 [Adobe I/O 프로그램](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O 콘솔 프로젝트는 통합 관리 방법을 기반으로 통합을 만들거나 사용 및 기존 프로젝트를 조직으로 그룹화한 것입니다.
   + 새 프로젝트를 만드는 경우 메시지가 표시되면 &quot;빈 프로젝트&quot;를 선택합니다(또는 &quot;템플릿에서 만들기&quot;).
   + Adobe I/O 콘솔 프로그램은 Cloud Manager 프로그램과 다른 개념입니다
1. 새 Cloud Manager API 통합 만들기
   + 더 이상 사용되지 않는 &quot;서비스 계정(JWT)&quot; 인증 유형을 선택합니다(OAuth는 현재 CLI에 대해 지원되지 않음).
   + 키를 만들거나 업로드합니다.
   + &quot;개발자 - Cloud Service&quot; 제품 프로필 선택
1. 서비스 계정(JWT) 자격 증명을 얻으려면 Adobe I/O CLI를 채워야 함 [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)

   ```json
   //config.json 
   {
      "client_id": "Client ID from Service Account (JWT) credential",
      "client_secret": "Client Secret from Service Account (JWT) credential",
      "technical_account_id": "Technical Account ID from Service Account (JWT) credential",
      "ims_org_id": "Organization ID from Service Account (JWT) credential",
      "meta_scopes": [
        "ent_cloudmgr_sdk"
      ]
   }
   ```

1. 을(를) 로드합니다 `config.json` Adobe I/O CLI에 파일 삽입
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager ./path/to/config.json --file --json`
1. 을(를) 로드합니다 `private.key` Adobe I/O CLI에 파일 삽입
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key ./path/to/private.key --file`

시작 [명령 실행](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) Adobe I/O CLI를 통해 Cloud Manager용

### AEM Rapid Development Environment 플러그인 설정{#rde}

AEM Rapid Development Environment 플러그인을 사용하면 aio CLI가 AEM as a Cloud Service과 상호 작용할 수 있습니다 [신속한 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 를 통해 `aio aem:rde` 명령입니다.

1. 실행 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` 을(를) 설치하려면 [AEM Rapid Development Environments 플러그인](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Adobe I/O CLI Asset compute 플러그인 설정{#aio-asset-compute}

Adobe I/O Cloud Manager 플러그인을 사용하면 aio CLI에서 를 통해 Asset compute 작업자를 생성하고 실행할 수 있습니다. `aio asset-compute` 명령입니다.

1. 실행 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 을(를) 설치하려면 [aio Asset compute 플러그인](https://github.com/adobe/aio-cli-plugin-asset-compute).

## 개발 IDE 설정

AEM 개발은 주로 Java 및 프론트엔드(JavaScript, CSS 등) 개발 및 XML 관리로 구성됩니다. 다음은 AEM 개발에 가장 인기 있는 IDE입니다.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 는 Java 개발에 강력한 IDE입니다. IntelliJ IDEA는 무료 커뮤니티 에디션과 상용(유료) Ultimate 버전의 두 가지 버전으로 제공됩니다. 무료 커뮤니티 버전은 AEM 개발에 충분하지만, Ultimate [기능 세트 확장](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [IntelliJ IDEA 다운로드](https://www.jetbrains.com/idea/download)
+ [저장소 도구 다운로드](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio 코드

__[Visual Studio 코드](https://code.visualstudio.com/)__ (VS 코드)는 프론트엔드 개발자를 위한 무료 오픈 소스 도구입니다. Visual Studio Code는 Adobe 도구를 사용하여 AEM과 컨텐츠 동기화를 통합하도록 설정할 수 있습니다. __[보고](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code는 주로 프론트엔드 코드(JavaScript, CSS 및 HTML)를 만드는 프론트엔드 개발자에게 이상적인 선택입니다. 반면 VS 코드는 다음을 통해 Java를 지원합니다. [확장](https://code.visualstudio.com/docs/java/java-tutorial), 더 많은 Java 관련 기능에서 제공하는 일부 고급 기능이 부족할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio 코드 다운로드](https://code.visualstudio.com/Download)
+ [저장소 도구 다운로드](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Aemfed VS Code 확장 다운로드](https://aemfed.io/)
+ [AEM Sync VS 코드 확장 다운로드](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[이클립스](https://www.eclipse.org/ide/)__ 는 Java 개발에 널리 사용되는 IDE이며  __[AEM 개발자 도구](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ Adobe에서 제공하는 플러그인으로, 작성을 위한 in-IDE GUI를 제공하고 JCR 컨텐츠를 로컬 AEM 인스턴스와 동기화합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse 다운로드](https://www.eclipse.org/ide/)
+ [Eclipse 개발 도구 다운로드](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
