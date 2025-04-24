---
title: 로컬 AEM 개발 환경 설정
description: Experience Manager용 로컬 개발 환경을 설정하는 방법을 알아봅니다. 로컬 설치, Apache Maven, 통합 개발 환경, 디버깅 및 문제 해결에 대해 알아봅니다. Eclipse IDE, CRXDE-Lite, Visual Studio Code 및 IntelliJ를 사용합니다.
version: Experience Manager 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4537
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2408'
ht-degree: 0%

---

# 로컬 AEM 개발 환경 설정

Adobe Experience Manager, AEM에 대한 로컬 개발 설정에 대한 안내서입니다. 로컬 설치, Apache Maven, 통합 개발 환경 및 디버깅/문제 해결의 중요한 주제를 다룹니다. **Eclipse IDE, CRXDE Lite, Visual Studio Code 및 IntelliJ**&#x200B;을 사용한 개발에 대해 설명합니다.

## 개요

Adobe Experience Manager 또는 AEM용 개발 시 로컬 개발 환경을 설정하는 것이 첫 번째 단계입니다. 시간을 내어 생산성을 높이고 더 나은 코드를 더 빨리 작성할 수 있는 품질 개발 환경을 설정하십시오. AEM 로컬 개발 환경을 네 가지 영역으로 나눌 수 있습니다.

* 로컬 AEM 인스턴스
* [!DNL Apache Maven] 프로젝트
* IDE(통합 개발 환경)
* 문제 해결

## 로컬 AEM 인스턴스 설치

로컬 AEM 인스턴스를 참조할 때 개발자의 개인 컴퓨터에서 실행되는 Adobe Experience Manager 사본에 대해 이야기하고 있습니다. ***모두*** AEM 개발은 로컬 AEM 인스턴스에 대해 코드를 작성하고 실행하는 것부터 시작해야 합니다.

AEM을 처음 사용하는 경우 두 가지 기본 실행 모드를 설치할 수 있습니다. ***작성자*** 및 ***게시***. ***작성자*** [실행 모드](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)는 디지털 마케터가 콘텐츠를 만들고 관리하는 데 사용하는 환경입니다. 대부분의 경우 개발하면 작성자 인스턴스에 코드를 배포하게 됩니다. 페이지를 만들고 구성 요소를 추가 및 구성할 수 있습니다. AEM Sites은 WYSIWYG 제작 CMS이므로 작성 인스턴스에 대해 CSS와 JavaScript의 대부분을 테스트할 수 있습니다.

또한 로컬 ***Publish*** 인스턴스에 대해 *중요* 테스트 코드입니다. ***Publish*** 인스턴스는 웹 사이트 방문자가 상호 작용하는 AEM 환경입니다. ***Publish*** 인스턴스는 ***작성자*** 인스턴스와 동일한 기술 스택이지만 일부 주요 구성 및 사용 권한 구분이 있습니다. 더 높은 수준의 환경으로 승격하기 전에 로컬 ***Publish*** 인스턴스에 대해 코드를 테스트해야 합니다.

### 단계

1. Java™이 설치되어 있는지 확인합니다.
   * AEM 6.5+용 [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 기본 설정
   * AEM 6.5 이전 AEM 버전용 [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/)
1. [AEM QuickStart Jar 및  [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html)의 복사본을 가져옵니다.
1. 다음과 같이 컴퓨터에 폴더 구조를 만듭니다.

```plain
~/aem-sdk
    /author
    /publish
```

1. [!DNL QuickStart] JAR의 이름을 ***aem-author-p4502.jar***(으)로 바꾸고 `/author` 디렉터리 아래에 놓습니다. `/author` 디렉터리 아래에 ***[!DNL license.properties]*** 파일을 추가합니다.

1. [!DNL QuickStart] JAR의 복사본을 만들고 이름을 ***aem-publish-p4503.jar***(으)로 변경하고 `/publish` 디렉터리 아래에 놓습니다. `/publish` 디렉터리 아래에 ***[!DNL license.properties]*** 파일의 복사본을 추가합니다.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. ***aem-author-p4502.jar*** 파일을 두 번 클릭하여 **Author** 인스턴스를 설치합니다. 로컬 컴퓨터의 **4502** 포트에서 실행되는 작성자 인스턴스를 시작합니다.

***aem-publish-p4503.jar*** 파일을 두 번 클릭하여 **Publish** 인스턴스를 설치합니다. 로컬 컴퓨터의 **4503** 포트에서 실행되는 게시 인스턴스가 시작됩니다.

>[!NOTE]
>
>개발 컴퓨터의 하드웨어에 따라 **작성자 및 게시** 인스턴스를 동시에 실행하는 것이 어려울 수 있습니다. 로컬 설정에서 두 가지를 동시에 실행해야 하는 경우는 거의 없습니다.

### 명령줄 사용

JAR 파일을 두 번 클릭할 수 있는 또 다른 방법은 명령줄에서 AEM을 시작하거나 로컬 운영 체제 종류에 따라 스크립트(`.bat` 또는 `.sh`)를 만드는 것입니다. 다음은 샘플 명령의 예입니다.

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

여기서 `-X`은(는) JVM 옵션이고 `-D`은(는) 추가 프레임워크 속성입니다. 자세한 내용은 [AEM 인스턴스 배포 및 유지 관리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) 및 [빠른 시작 파일에서 사용할 수 있는 추가 옵션](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file)을 참조하십시오.

## Apache Maven 설치

***[!DNL Apache Maven]***&#x200B;은(는) Java 기반 프로젝트의 빌드 및 배포 절차를 관리하는 도구입니다. AEM은 Java 기반 플랫폼이며 [!DNL Maven]은(는) AEM 프로젝트에 대한 코드를 관리하는 표준 방법입니다. ***AEM Maven 프로젝트*** 또는 ***AEM 프로젝트***&#x200B;이라고 할 때 사이트에 대한 모든 *사용자 지정* 코드를 포함하는 Maven 프로젝트를 참조합니다.

모든 AEM 프로젝트는 **[!DNL AEM Project Archetype]**&#x200B;의 최신 버전을 사용하여 빌드해야 합니다. [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). [!DNL AEM Project Archetype]은(는) 일부 샘플 코드와 콘텐츠를 사용하여 AEM 프로젝트의 부트스트랩을 제공합니다. [!DNL AEM Project Archetype]에는 프로젝트에서 사용하도록 구성된 **[!DNL AEM WCM Core Components]**&#x200B;도 포함됩니다.

>[!CAUTION]
>
>새 프로젝트를 시작할 때 최신 버전의 Archetype을 사용하는 것이 좋습니다. 여러 버전의 Archetype이 있으며 일부 버전이 이전 버전의 AEM과 호환되는 것은 아니라는 점을 기억하십시오.

### 단계

1. [Apache Maven](https://maven.apache.org/download.cgi) 다운로드
2. [Apache Maven](https://maven.apache.org/install.html)을 설치하고 명령줄 `PATH`에 설치가 추가되었는지 확인하십시오.
   * [!DNL macOS] 사용자는 [Homebrew](https://brew.sh/)을(를) 사용하여 Maven을 설치할 수 있습니다.
3. 새 명령줄 터미널을 열고 다음을 실행하여 **[!DNL Maven]**&#x200B;이(가) 설치되었는지 확인합니다.

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> `nexus.adobe.com`에서 AEM 아티팩트를 다운로드하도록 하려면 이전에 `adobe-public` Maven 프로필을 추가해야 했습니다. 이제 모든 AEM 아티팩트를 Maven Central을 통해 사용할 수 있으며 `adobe-public` 프로필이 필요하지 않습니다.

## 통합 개발 환경 설정

통합 개발 환경 또는 IDE는 텍스트 편집기, 구문 지원 및 빌드 도구를 결합한 응용 프로그램입니다. 수행하는 개발 유형에 따라 한 IDE가 다른 IDE보다 선호될 수 있습니다. IDE에 관계없이 테스트하려면 로컬 AEM 인스턴스에 정기적으로 ***푸시*** 코드를 사용할 수 있어야 합니다. Git과 같은 소스 제어 관리 시스템에 지속하려면 때때로 로컬 AEM 인스턴스에서 AEM 프로젝트로 ***끌어오기*** 구성을 만들어야 합니다.

다음은 로컬 AEM 인스턴스와의 통합을 보여 주는 해당 비디오와 함께 AEM 개발에 사용되는 보다 인기 있는 몇 가지 IDE입니다.

>[!NOTE]
>
> WKND 프로젝트가 AEM as a Cloud Service에서 작동하도록 기본값으로 업데이트되었습니다. [이전 버전 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)과(와) 호환되도록 업데이트되었습니다. AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 Maven 명령에 추가합니다.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDE를 사용하는 경우 Maven 프로필 탭에서 `classic`을(를) 확인하십시오.

![Maven 프로필 탭](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven 프로필*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;은(는) 대부분 오픈 소스이고 ***무료***&#x200B;이기 때문에 Java™ 개발용으로 더 많이 사용되는 IDE 중 하나입니다. Adobe에서는 코드를 로컬 AEM 인스턴스와 동기화하는 유용한 GUI를 사용하여 보다 쉽게 개발할 수 있도록 [!DNL Eclipse]에 대해 **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)** 플러그인을 제공합니다. [!DNL AEM Developer Tools]의 GUI 지원 때문에 AEM을 처음 사용하는 개발자에게 [!DNL Eclipse] IDE를 사용하는 것이 좋습니다.

#### 설치 및 설정

1. [!DNL Java™ EE Developers]에 대한 [!DNL Eclipse] IDE를 다운로드하여 설치하십시오. [https://www.eclipse.org](https://www.eclipse.org/)
1. 다음 지침에 따라 [!DNL AEM Developer Tools] 플러그인을 설치하십시오. [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 01:24 - Maven과 소스 코드 작성 및 배포
* 04:33 - 푸시 코드 변경 와 AEM 개발자 도구
* 10:55 - AEM 개발자 도구로 코드 변경 가져오기
* 13:12 - Eclipse의 통합 디버깅 도구 사용

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;은(는) 전문적인 Java™ 개발을 위한 강력한 IDE입니다. [!DNL IntelliJ IDEA]은(는) 두 가지 버전, ***무료*** [!DNL Community] 버전 및 상업용(유료) [!DNL Ultimate] 버전으로 제공됩니다. 사용 가능한 [!DNL Community] 버전의 [!DNL IntellIJ IDEA]은(는) 더 많은 AEM 개발에 충분하지만, [!DNL Ultimate] [은(는) 기능 집합을 확장](https://www.jetbrains.com/idea/download)합니다.

#### [!DNL Installation and Setup]

1. [!DNL IntelliJ IDEA] 다운로드 및 설치: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. [!DNL Repo] 설치(명령줄 도구): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Maven 프로젝트 가져오기
* 05:47 - Maven과 소스 코드 작성 및 배포
* 08:17 - 푸시 changes 와 Repo
* 14:39 - Pull changes 와 보고
* 17:25 - IntelliJ IDEA의 통합 디버깅 도구 사용

### [!DNL Visual Studio Code]

**[Visual Studio 코드](https://code.visualstudio.com/)**&#x200B;은(는) 향상된 JavaScript 지원, [!DNL Intellisense] 및 브라우저 디버깅 지원을 통해 ***프론트엔드 개발자***&#x200B;에게 빠르게 인기 있는 도구가 되었습니다. **[!DNL Visual Studio Code]**&#x200B;은(는) 무료 오픈 소스이며 강력한 확장이 많습니다. [!DNL Visual Studio Code]은(는) Adobe 도구인 **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)을(를) 통해 AEM과 통합되도록 설정할 수 있습니다.** AEM과 통합하기 위해 설치할 수 있는 커뮤니티 지원 확장도 여러 개 있습니다.

[!DNL Visual Studio Code]은(는) 주로 CSS/LESS 및 JavaScript 코드를 작성하여 AEM 클라이언트 라이브러리를 만드는 프론트엔드 개발자에게 적합한 선택입니다. 노드 정의(대화 상자, 구성 요소)는 원시 XML에서 편집해야 하므로 이 도구는 새 AEM 개발자에게 최선의 선택이 아닐 수 있습니다. [!DNL Visual Studio Code]에 사용할 수 있는 여러 Java™ 확장이 있지만 주로 Java™ 개발 [!DNL Eclipse IDE] 또는 [!DNL IntelliJ]을(를) 수행하는 경우 선호될 수 있습니다.

#### 중요 링크

* [**다운로드**](https://code.visualstudio.com/Download) **Visual Studio 코드**
* **[보고](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR 콘텐츠용 FTP와 유사한 도구
* Visual Studio 코드용 **[AEM 동기화](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - 커뮤니티 지원&#42; 확장
* **[WKND 프로젝트](https://github.com/adobe/aem-guides-wknd)** - 이 비디오에 표시된 AEM 프로젝트의 예입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 00:53 - Maven과 소스 코드 작성 및 배포
* 04:03 - Push code changes 와 Repo 명령줄 도구
* 08:29 - Pull code changes 와 Repo 명령줄 도구
* 10:32 - 문제 해결, 클라이언트 라이브러리 다시 빌드

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html)은(는) AEM 저장소의 브라우저 기반 보기입니다. [!DNL CRXDE Lite]은(는) AEM에 포함되어 있으며 개발자가 파일 편집, 구성 요소 정의, 대화 상자 및 템플릿과 같은 표준 개발 작업을 수행할 수 있도록 해 줍니다. [!DNL CRXDE Lite]은(는) 전체 개발 환경을 위한 ***not***&#x200B;이지만 디버깅 도구로 유효합니다. [!DNL CRXDE Lite]은(는) 코드 베이스 외부의 제품 코드를 확장하거나 이해할 때 유용합니다. [!DNL CRXDE Lite]은(는) 리포지토리의 강력한 보기와 권한을 효과적으로 테스트 및 관리할 수 있는 방법을 제공합니다.

[!DNL CRXDE Lite]은(는) 다른 IDE와 함께 사용하여 코드를 테스트하고 디버깅해야 하지만 기본 개발 도구로는 사용할 수 없습니다. 구문 지원이 제한적이고 자동 완성 기능이 없으며 소스 제어 관리 시스템과의 통합이 제한적입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 문제 해결

***도움말!*** 내 코드가 작동하지 않습니다! 모든 개발과 마찬가지로 코드가 예상대로 작동하지 않는 경우가 있습니다(많은 수). AEM은 강력한 플랫폼이지만 강력한 힘을 가진... 엄청난 복잡성을 가져옵니다. 다음은 문제 해결 및 추적 시 몇 가지 높은 수준의 시작 지점입니다(하지만 잘못될 수 있는 전체 목록은 제외).

### 코드 배포 확인

문제가 발생하면 첫 번째 단계에서 코드가 AEM에 성공적으로 배포 및 설치되었는지 확인하는 것이 좋습니다.

1. **코드 패키지가 업로드되고 설치되었는지 확인하려면 [!UICONTROL 패키지 관리자]**&#x200B;를 확인하십시오. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 타임스탬프를 확인하여 패키지가 최근에 설치되었는지 확인합니다.
1. [!DNL Repo] 또는 [!DNL AEM Developer Tools]과(와) 같은 도구를 사용하여 증분 파일 업데이트를 수행하는 경우 **파일이 로컬 AEM 인스턴스로 푸시되었고 파일 내용이 업데이트되었는지[!DNL CRXDE Lite]**&#x200B;확인합니다. [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **OSGi 번들에서 Java™ 코드와 관련된 문제가 표시되면 번들이 업로드되었는지 확인**. [!UICONTROL Adobe Experience Manager 웹 콘솔]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)을 열고 번들을 검색합니다. 번들의 상태가 **[!UICONTROL 활성]**&#x200B;인지 확인하십시오. **[!UICONTROL 설치됨]** 상태에서 번들 문제 해결에 대한 자세한 내용은 아래를 참조하십시오.

#### 로그 확인

AEM은 채팅 플랫폼이며 **error.log**&#x200B;에 유용한 정보를 기록합니다. AEM이 설치된 위치에서 **error.log**&#x200B;을(를) 찾을 수 있습니다. &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

문제를 추적하는 데 유용한 기술은 Java™ 코드에 로그 문을 추가하는 것입니다.

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

기본적으로 **error.log**&#x200B;은(는) *[!DNL INFO]* 문을 기록하도록 구성되어 있습니다. 로그 수준을 변경하려면 [!UICONTROL 로그 지원]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)&#x200B;(으)로 이동하십시오. **error.log**&#x200B;이(가) 너무 수다스럽습니다. [!UICONTROL 로그 지원]을 사용하여 지정된 Java™ 패키지에 대한 로그 문을 구성할 수 있습니다. 이는 사용자 지정 코드 문제를 OOTB AEM 플랫폼 문제와 쉽게 분리하기 위한 프로젝트에 대한 우수 사례입니다.

![AEM의 구성 로깅](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 번들이 설치됨 상태입니다. {#bundle-active}

모든 번들(조각 제외)은 **[!UICONTROL Active]** 상태여야 합니다. 코드 번들이 [!UICONTROL 설치됨] 상태로 표시되면 해결해야 하는 문제가 있습니다. 대부분의 경우 이는 종속성 문제입니다.

![AEM의 번들 오류](assets/set-up-a-local-aem-development-environment/bundle-error.png)

위의 스크린샷에서 [!DNL WKND Core bundle]은(는) [!UICONTROL 설치됨] 상태입니다. 이는 번들에 AEM 인스턴스에서 사용할 수 있는 것과 다른 버전의 `com.adobe.cq.wcm.core.components.models`이(가) 필요하기 때문입니다.

사용할 수 있는 유용한 도구는 [!UICONTROL 종속성 파인더]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)입니다. AEM 인스턴스에서 사용할 수 있는 버전을 검사하려면 Java™ 패키지 이름을 추가합니다.

![핵심 구성 요소](assets/set-up-a-local-aem-development-environment/core-components.png)

위의 예를 계속 진행하면 AEM 인스턴스에 설치된 버전이 번들에 필요한 **12.2**&#x200B;와 **12.6**&#x200B;임을 알 수 있습니다. 거기에서 거꾸로 작업하여 AEM의 [!DNL Maven] 종속성이 AEM 프로젝트의 [!DNL Maven] 종속성과 일치하는지 확인할 수 있습니다. 에서 위의 예 [!DNL Core Components] **v2.2.0**&#x200B;이(가) AEM 인스턴스에 설치되었지만 코드 번들이 **v2.2.2**&#x200B;에 종속되어 빌드되었으므로 종속성 문제가 발생합니다.

#### Sling 모델 등록 확인 {#osgi-component-sling-models}

AEM 구성 요소는 모든 비즈니스 논리를 캡슐화하고 HTL 렌더링 스크립트가 깔끔하게 유지되도록 [!DNL Sling Model]에 의해 지원되어야 합니다. 슬링 모델을 찾을 수 없는 문제가 발생하는 경우 콘솔에서 [!DNL Sling Models]을(를) 확인하는 것이 좋습니다. [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 이는 슬링 모델이 등록되었는지 여부와 해당 모델이 연결된 리소스 유형(구성 요소 경로)을 알려줍니다.

![슬링 모델 상태](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

구성 요소 리소스 유형 `wknd/components/content/byline`에 연결된 [!DNL Sling Model], `BylineImpl`의 등록을 표시합니다.

#### CSS 또는 JavaScript 문제

대부분의 CSS 및 JavaScript 문제의 경우 브라우저의 개발 도구를 사용하는 것이 문제를 해결하는 가장 효과적인 방법입니다. AEM 작성자 인스턴스에 대해 개발할 때 문제를 좁히려면 페이지를 &quot;게시됨&quot;으로 보는 것이 좋습니다.

![CSS 또는 JS 문제](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

[!UICONTROL 페이지 속성] 메뉴를 열고 [!UICONTROL 게시됨으로 보기]를 클릭합니다. 이렇게 하면 AEM 편집기 없이 쿼리 매개 변수가 **wcmmode=disabled**(으)로 설정된 페이지가 열립니다. 이렇게 하면 AEM 작성 UI가 효과적으로 비활성화되고 프론트엔드 문제 해결/디버깅이 훨씬 쉬워집니다.

프론트엔드 코드를 개발할 때 자주 발생하는 다른 문제는 오래되었거나 오래된 CSS/JS가 로드되고 있습니다. 첫 번째 단계로, 브라우저 기록이 지워졌는지 확인하고 필요한 경우 시크릿 브라우저 또는 새 세션을 시작합니다.

#### 클라이언트 라이브러리 디버깅

여러 클라이언트 라이브러리를 포함하도록 다양한 카테고리 및 임베디드 방법을 사용하면 문제를 해결하는 데 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 제공합니다. 가장 중요한 도구 중 하나는 AEM에서 LESS 파일을 다시 컴파일하고 CSS를 생성하도록 하는 [!UICONTROL 클라이언트 라이브러리 다시 빌드]입니다.

* [덤프 라이브러리](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 모든 클라이언트 라이브러리를 나열합니다. &lt;host>/libs/granite/ui/content/dumplibs.html
* [테스트 출력](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 사용자가 범주를 기반으로 하는 clientlib의 예상 HTML 출력을 볼 수 있습니다. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [라이브러리 종속성 유효성 검사](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [클라이언트 라이브러리 다시 빌드](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 모든 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM에서 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS로 개발할 때 효과적입니다. 일반적으로 캐시를 무효화한 다음 페이지 새로 고침을 수행하는 것이 모든 라이브러리를 다시 작성하는 것보다 더 효과적입니다. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Clientlibs 디버깅](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>[!UICONTROL 클라이언트 라이브러리 다시 빌드] 도구를 사용하여 캐시를 계속 무효화해야 하는 경우 모든 클라이언트 라이브러리를 한 번에 다시 빌드하는 것이 좋습니다. 이 작업은 약 15분 정도 소요될 수 있지만 일반적으로 나중에 발생하는 캐싱 문제를 제거합니다.
