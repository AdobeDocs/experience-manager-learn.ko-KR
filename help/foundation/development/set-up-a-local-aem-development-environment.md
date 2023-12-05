---
title: 로컬 AEM 개발 환경 설정
description: Experience Manager을 위한 로컬 개발 환경을 설정하는 방법에 대해 알아봅니다. 로컬 설치, Apache Maven, 통합 개발 환경, 디버깅 및 문제 해결에 대해 알아봅니다. Eclipse IDE, CRXDE-Lite, Visual Studio Code 및 IntelliJ를 사용합니다.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4693
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 0%

---

# 로컬 AEM 개발 환경 설정

Adobe Experience Manager, AEM용 로컬 개발 설정에 대한 안내서입니다. 로컬 설치, Apache Maven, 통합 개발 환경 및 디버깅/문제 해결의 중요한 주제를 다룹니다. 을 사용한 개발 **Eclipse IDE, CRXDE Lite, Visual Studio Code 및 IntelliJ** 논의됩니다.

## 개요

Adobe Experience Manager 또는 AEM용 개발 시 로컬 개발 환경을 설정하는 것이 첫 번째 단계입니다. 시간을 내어 생산성을 높이고 더 나은 코드를 더 빨리 작성할 수 있는 품질 개발 환경을 설정하십시오. AEM 로컬 개발 환경을 네 가지 영역으로 나눌 수 있습니다.

* 로컬 AEM 인스턴스
* [!DNL Apache Maven] 프로젝트
* IDE(통합 개발 환경)
* 문제 해결

## 로컬 AEM 인스턴스 설치

로컬 AEM 인스턴스를 참조할 때 개발자의 개인 컴퓨터에서 실행되는 Adobe Experience Manager 사본에 대해 이야기하고 있습니다. ***모두*** AEM 개발은 로컬 AEM 인스턴스에 대해 코드를 작성하고 실행하는 것부터 시작해야 합니다.

AEM을 처음 사용하는 경우 두 가지 기본 실행 모드를 설치할 수 있습니다. ***작성자*** 및 ***게시***. 다음 ***작성자*** [실행 모드](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  은 디지털 마케터가 콘텐츠를 만들고 관리하는 데 사용하는 환경입니다. 대부분의 경우 개발하면 작성자 인스턴스에 코드를 배포하게 됩니다. 페이지를 만들고 구성 요소를 추가 및 구성할 수 있습니다. AEM Sites은 WYSIWYG 작성 CMS이므로 작성 인스턴스에 대해 CSS 및 JavaScript의 대부분을 테스트할 수 있습니다.

또한 *중요* 로컬에 대해 코드 테스트 ***게시*** 인스턴스. 다음 ***게시*** 인스턴스는 웹 사이트 방문자가 상호 작용하는 AEM 환경입니다. 동안 ***게시*** 인스턴스는 와 동일한 기술 스택입니다. ***작성자*** 예를 들어, 구성 및 권한에는 몇 가지 중요한 차이점이 있습니다. 코드를 로컬에 대해 테스트해야 함 ***게시*** 상위 수준 환경으로 승격되기 전의 인스턴스.

### 단계

1. Java™이 설치되어 있는지 확인합니다.
   * 기본 설정 [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) AEM 6.5+용
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) AEM 6.5 이전 버전의 AEM용
1. 사본 받기 [AEM 빠른 시작 Jar 및 [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html).
1. 다음과 같이 컴퓨터에 폴더 구조를 만듭니다.

```plain
~/aem-sdk
    /author
    /publish
```

1. 이름 바꾸기 [!DNL QuickStart] JAR 대상 ***aem-author-p4502.jar*** 아래에 위치시킵니다. `/author` 디렉토리. 추가 ***[!DNL license.properties]*** 아래에 있는 파일 `/author` 디렉토리.

1. 다음을 복사하십시오. [!DNL QuickStart] JAR의 이름을 로 바꿉니다. ***aem-publish-p4503.jar*** 아래에 위치시킵니다. `/publish` 디렉토리. 의 복사본 추가 ***[!DNL license.properties]*** 아래에 있는 파일 `/publish` 디렉토리.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. 를 두 번 클릭합니다. ***aem-author-p4502.jar*** 설치할 파일 **작성자** 인스턴스. 이렇게 하면 작성자 인스턴스가 시작되고 포트에서 실행됩니다. **4502** 로컬 컴퓨터에서.

를 두 번 클릭합니다. ***aem-publish-p4503.jar*** 설치할 파일 **게시** 인스턴스. 이렇게 하면 포트에서 실행 중인 게시 인스턴스가 시작됩니다 **4503** 로컬 컴퓨터에서.

>[!NOTE]
>
>개발 시스템의 하드웨어에 따라 **작성 및 게시** 동시에 실행되는 인스턴스. 로컬 설정에서 두 가지를 동시에 실행해야 하는 경우는 거의 없습니다.

### 명령줄 사용

JAR 파일을 두 번 클릭할 수 있는 다른 방법은 명령줄에서 AEM을 시작하거나 스크립트(`.bat` 또는 `.sh`) 로컬 운영 체제의 종류에 따라 다릅니다. 다음은 샘플 명령의 예입니다.

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

여기, `-X` jvm 옵션 및 `-D` 추가 프레임워크 속성입니다. 자세한 내용은 [AEM 인스턴스 배포 및 유지 관리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) 및 [빠른 시작 파일에서 사용할 수 있는 추가 옵션](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Apache Maven 설치

***[!DNL Apache Maven]*** 는 Java 기반 프로젝트의 빌드 및 배포 절차를 관리하는 도구입니다. AEM은 Java 기반 플랫폼이며 [!DNL Maven] 는 AEM 프로젝트에 대한 코드를 관리하는 표준 방법입니다. 말할 때 ***AEM Maven 프로젝트*** 또는 ***AEM 프로젝트***, 는 모든 을 포함하는 Maven 프로젝트를 나타냅니다. *사용자 정의* 사이트에 대한 코드입니다.

모든 AEM 프로젝트는 최신 버전의 **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). 다음 [!DNL AEM Project Archetype] 일부 샘플 코드 및 콘텐츠가 있는 AEM 프로젝트의 부트스트랩을 제공합니다. 다음 [!DNL AEM Project Archetype] 또한 다음을 포함 **[!DNL AEM WCM Core Components]** 프로젝트에서 사용하도록 구성되었습니다.

>[!CAUTION]
>
>새 프로젝트를 시작할 때 최신 버전의 Archetype을 사용하는 것이 좋습니다. 여러 버전의 Archetype이 있으며 일부 버전이 이전 버전의 AEM과 호환되는 것은 아니라는 점을 기억하십시오.

### 단계

1. 다운로드 [Apache Maven](https://maven.apache.org/download.cgi)
2. 설치 [Apache Maven](https://maven.apache.org/install.html) 명령줄에 설치가 추가되었는지 확인합니다. `PATH`.
   * [!DNL macOS] 사용자는 다음을 사용하여 Maven을 설치할 수 있습니다. [홈브루](https://brew.sh/)
3. 다음을 확인합니다. **[!DNL Maven]** 는 새 명령줄 터미널을 열고 다음 작업을 실행하여 설치됩니다.

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
> 에서, 지난 추가 `adobe-public` 가리키려면 Maven 프로필이 필요함 `nexus.adobe.com` AEM 아티팩트를 다운로드하려면 다음을 수행하십시오. 이제 모든 AEM 아티팩트를 Maven Central 및 `adobe-public` 프로필이 필요하지 않습니다.

## 통합 개발 환경 설정

통합 개발 환경 또는 IDE는 텍스트 편집기, 구문 지원 및 빌드 도구를 결합한 응용 프로그램입니다. 수행하는 개발 유형에 따라 한 IDE가 다른 IDE보다 선호될 수 있습니다. IDE와 관계없이 주기적으로 수행할 수 있는 것이 중요합니다 ***푸시*** 테스트하기 위해 로컬 AEM 인스턴스에 코드를 추가합니다. 가끔 하는 것이 중요하다 ***가져오기*** git과 같은 소스 제어 관리 시스템으로 지속하기 위해 로컬 AEM 인스턴스에서 AEM 프로젝트로 구성

다음은 로컬 AEM 인스턴스와의 통합을 보여 주는 해당 비디오와 함께 AEM 개발에 사용되는 보다 인기 있는 몇 가지 IDE입니다.

>[!NOTE]
>
> WKND 프로젝트가 기본적으로 AEM as a Cloud Service으로 작동하도록 업데이트되었습니다. 다음과 같이 업데이트되었습니다. [6.5/6.4와 이전 버전과 호환](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). AEM 6.5 또는 6.4를 사용하는 경우 `classic` 모든 Maven 명령에 대한 프로필

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDE를 사용할 때 다음을 확인하십시오. `classic` Maven 프로필 탭에서

![Maven 프로필 탭](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven 프로필*

### [!DNL Eclipse] IDE

다음 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 는 오픈 소스이며 Java™ 개발에 널리 사용되는 IDE 중 하나입니다. ***무료***! Adobe은 플러그인을 제공합니다. **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, [!DNL Eclipse] 로컬 AEM 인스턴스와 코드를 동기화할 수 있는 GUI를 사용하여 보다 쉽게 개발할 수 있습니다. 다음 [!DNL Eclipse] IDE는 AEM을 처음 사용하는 개발자에게 권장되는데, 이는 의 GUI 지원 때문입니다. [!DNL AEM Developer Tools].

#### 설치 및 설정

1. 다운로드 및 설치 [!DNL Eclipse] 에 대한 IDE [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. 지침에 따라 다음을 설치합니다. [!DNL AEM Developer Tools] 플러그인: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 01:24 - Maven과 소스 코드 작성 및 배포
* 04:33 - 푸시 코드 변경 와 AEM 개발자 도구
* 10:55 - Pull code changes 와 AEM 개발자 도구
* 13:12 - Eclipse의 통합 디버깅 도구 사용

### IntelliJ IDEA

다음 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 는 전문적인 Java™ 개발을 위한 강력한 IDE입니다. [!DNL IntelliJ IDEA] 두 가지 맛으로 제공됩니다. ***무료*** [!DNL Community] 에디션 및 커머셜 (유료) [!DNL Ultimate] 버전. 더 프리 [!DNL Community] 버전 [!DNL IntellIJ IDEA] 는 더 많은 AEM 개발에 충분하지만, [!DNL Ultimate] [기능 세트 확장](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. 다운로드 및 설치 [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 설치 [!DNL Repo] (명령줄 도구): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Maven 프로젝트 가져오기
* 05:47 - Maven과 소스 코드 작성 및 배포
* 08:17 - 푸시 changes 와 Repo
* 14:39 - Pull changes 와 보고
* 17:25 - IntelliJ IDEA의 통합 디버깅 도구 사용

### [!DNL Visual Studio Code]

**[Visual Studio 코드](https://code.visualstudio.com/)** 은(는) 의 인기 도구가 되었습니다. ***프론트엔드 개발자*** 향상된 JavaScript 지원을 통해 [!DNL Intellisense], 브라우저 디버깅 지원. **[!DNL Visual Studio Code]** 는 오픈 소스, 무료, 많은 강력한 확장을 가지고 있습니다. [!DNL Visual Studio Code] 은 Adobe 도구를 사용하여 AEM과 통합하도록 설정할 수 있습니다. **[보고](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** AEM과 통합하기 위해 설치할 수 있는 몇 가지 커뮤니티 지원 확장도 있습니다.

[!DNL Visual Studio Code] 는 AEM 클라이언트 라이브러리를 만들기 위해 CSS/LESS 및 JavaScript 코드를 주로 작성하는 프론트엔드 개발자에게 적합합니다. 노드 정의(대화 상자, 구성 요소)는 원시 XML에서 편집해야 하므로 이 도구는 새 AEM 개발자에게 최선의 선택이 아닐 수 있습니다. 에 사용할 수 있는 몇 가지 Java™ 확장이 있습니다. [!DNL Visual Studio Code], 하지만 주로 Java™ 개발을 수행하는 경우 [!DNL Eclipse IDE] 또는 [!DNL IntelliJ] 선호될 수 있습니다.

#### 중요 링크

* [**다운로드**](https://code.visualstudio.com/Download) **Visual Studio 코드**
* **[보고](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR 콘텐츠용 FTP와 유사한 도구
* **[비어 있음](https://aemfed.io/)** - AEM 프론트엔드 워크플로 가속화
* **[AEM 동기화](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - 지원되는 커뮤니티&#42; visual Studio 코드용 확장

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 00:53 - Maven과 소스 코드 작성 및 배포
* 04:03 - Push code changes 와 Repo 명령줄 도구
* 08:29 - Pull code changes 와 Repo 명령줄 도구
* 10:40 - aemfed 도구로 코드 변경 푸시
* 14:24 - 문제 해결, 클라이언트 라이브러리 다시 빌드

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) 는 AEM 저장소의 브라우저 기반 보기입니다. [!DNL CRXDE Lite] 는 AEM에 포함되어 있으며 개발자가 파일 편집, 구성 요소 정의, 대화 상자 및 템플릿과 같은 표준 개발 작업을 수행할 수 있도록 해 줍니다. [!DNL CRXDE Lite] 은(는) ***아님*** 전체 개발 환경을 위한 것이지만 디버깅 도구로 효과적입니다. [!DNL CRXDE Lite] 는 코드 베이스 외부의 제품 코드를 확장하거나 간단히 이해할 때 유용합니다. [!DNL CRXDE Lite] 는 저장소에 대한 강력한 보기와 권한을 효과적으로 테스트 및 관리하는 방법을 제공합니다.

[!DNL CRXDE Lite] 는 다른 IDE와 함께 사용하여 코드를 테스트하고 디버깅해야 하지만 기본 개발 도구로는 사용할 수 없습니다. 구문 지원이 제한적이고 자동 완성 기능이 없으며 소스 제어 관리 시스템과의 통합이 제한적입니다.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 문제 해결

***살려줘!*** 코드가 작동하지 않습니다! 모든 개발과 마찬가지로 코드가 예상대로 작동하지 않는 경우가 있습니다(많은 수). AEM은 강력한 플랫폼이지만 강력한 힘을 가진... 엄청난 복잡성을 가져옵니다. 다음은 문제 해결 및 추적 시 몇 가지 높은 수준의 시작 지점입니다(하지만 잘못될 수 있는 전체 목록은 제외).

### 코드 배포 확인

문제가 발생하면 첫 번째 단계에서 코드가 AEM에 성공적으로 배포 및 설치되었는지 확인하는 것이 좋습니다.

1. **확인 [!UICONTROL 패키지 관리자]** 코드 패키지가 업로드 및 설치되었는지 확인하려면 다음을 수행하십시오. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 타임스탬프를 확인하여 패키지가 최근에 설치되었는지 확인합니다.
1. 다음과 같은 도구를 사용하여 증분 파일 업데이트를 수행하는 경우 [!DNL Repo] 또는 [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** 파일을 로컬 AEM 인스턴스로 푸시하고 파일 내용을 업데이트합니다. [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **번들이 업로드되었는지 확인합니다.** osgi 번들에서 Java™ 코드와 관련된 문제가 발생하는 경우. 를 엽니다. [!UICONTROL Adobe Experience Manager 웹 콘솔]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 번들을 검색합니다. 번들에 **[!UICONTROL 활성]** 상태. 의 번들 문제 해결과 관련된 자세한 내용은 아래를 참조하십시오. **[!UICONTROL 설치됨]** 주.

#### 로그 확인

AEM은 수다스러운 플랫폼이며 **error.log**. 다음 **error.log** AEM이 설치된 위치에서 찾을 수 있습니다. &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

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

기본적으로 **error.log** 이(가) 기록하도록 구성되었습니다. *[!DNL INFO]* 명령문입니다. 로그 수준을 변경하려는 경우 [!UICONTROL 로그 지원]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 또한 다음을 찾을 수 있습니다. **error.log** 너무 수다스러워요 다음을 사용할 수 있습니다. [!UICONTROL 로그 지원] 지정된 Java™ 패키지에 대해서만 로그 문을 구성합니다. 이는 사용자 지정 코드 문제를 OOTB AEM 플랫폼 문제와 쉽게 분리하기 위한 프로젝트에 대한 우수 사례입니다.

![AEM의 구성 로깅](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 번들이 설치됨 상태입니다. {#bundle-active}

모든 번들(조각 제외)은 **[!UICONTROL 활성]** 주. 에 코드 번들이 표시되면 [!UICONTROL 설치됨] state, 그러면 해결해야 할 문제가 있습니다. 대부분의 경우 이는 종속성 문제입니다.

![AEM의 번들 오류](assets/set-up-a-local-aem-development-environment/bundle-error.png)

위의 스크린샷에서 [!DNL WKND Core bundle] 은(는) [!UICONTROL 설치됨] 주. 번들에 다른 버전의 가 필요하기 때문입니다. `com.adobe.cq.wcm.core.components.models` 는 AEM 인스턴스에서 사용할 수 있습니다.

사용할 수 있는 유용한 도구는 다음과 같습니다. [!UICONTROL 종속성 파인더]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEM 인스턴스에서 사용할 수 있는 버전을 검사하려면 Java™ 패키지 이름을 추가합니다.

![핵심 구성 요소](assets/set-up-a-local-aem-development-environment/core-components.png)

위의 예를 계속 진행하여 AEM 인스턴스에 설치된 버전이 **12.2** 및 **12.6** 번들에 예상된 결과입니다. 거기에서 뒤로 작업하여 [!DNL Maven] AEM에 대한 종속성이 [!DNL Maven] AEM 프로젝트에 종속됩니다. 에서, 위의 예 [!DNL Core Components] **v2.2.0** 이 AEM 인스턴스에 설치되지만 코드 번들은 다음에 대한 종속성을 사용하여 빌드되었습니다. **v2.2.2**&#x200B;종속성 문제가 발생하는 이유입니다.

#### Sling 모델 등록 확인 {#osgi-component-sling-models}

AEM 구성 요소는 [!DNL Sling Model] 비즈니스 논리를 캡슐화하고 HTL 렌더링 스크립트가 깔끔하게 유지되도록 합니다. 슬링 모델을 찾을 수 없는 문제가 발생하는 경우 다음을 확인하는 것이 좋습니다. [!DNL Sling Models] 콘솔에서: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 이는 슬링 모델이 등록되었는지 여부와 해당 모델이 연결된 리소스 유형(구성 요소 경로)을 알려줍니다.

![Sling 모델 상태](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

의 등록을 표시합니다. [!DNL Sling Model], `BylineImpl` 다음의 구성 요소 리소스 유형에 연결되어 있습니다. `wknd/components/content/byline`.

#### CSS 또는 JavaScript 문제

대부분의 CSS 및 JavaScript 문제에 대해 브라우저의 개발 도구를 사용하는 것이 문제를 해결하는 가장 효과적인 방법입니다. AEM 작성자 인스턴스에 대해 개발할 때 문제를 좁히려면 페이지를 &quot;게시됨&quot;으로 보는 것이 좋습니다.

![CSS 또는 JS 문제](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

를 엽니다. [!UICONTROL 페이지 속성] 메뉴 및 클릭 [!UICONTROL 게시됨으로 보기]. 이렇게 하면 AEM 편집기 없이 쿼리 매개 변수가 로 설정된 페이지가 열립니다 **wcmmode=disabled**. 이렇게 하면 AEM 작성 UI가 효과적으로 비활성화되고 프론트엔드 문제 해결/디버깅이 훨씬 쉬워집니다.

프론트엔드 코드를 개발할 때 자주 발생하는 다른 문제는 오래되었거나 오래된 CSS/JS가 로드되고 있습니다. 첫 번째 단계로, 브라우저 기록이 지워졌는지 확인하고 필요한 경우 시크릿 브라우저 또는 새 세션을 시작합니다.

#### 클라이언트 라이브러리 디버깅

여러 클라이언트 라이브러리를 포함하도록 다양한 카테고리 및 임베디드 방법을 사용하면 문제를 해결하는 데 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 노출합니다. 가장 중요한 도구 중 하나는 [!UICONTROL 클라이언트 라이브러리 다시 빌드] AEM에서 LESS 파일을 다시 컴파일하고 CSS를 생성해야 합니다.

* [라이브러리 덤프](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 모든 클라이언트 라이브러리를 나열합니다. &lt;host>/libs/granite/ui/content/dumplibs.html
* [출력 테스트](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 범주를 기반으로 하는 clientlib의 예상 HTML 출력을 볼 수 있습니다. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [라이브러리 종속성 유효성 검사](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [클라이언트 라이브러리 다시 빌드](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 모든 클라이언트 라이브러리를 다시 작성하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM에서 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS로 개발할 때 효과적입니다. 일반적으로 캐시를 무효화한 다음 페이지 새로 고침을 수행하는 것이 모든 라이브러리를 다시 작성하는 것보다 더 효과적입니다. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Clientlibs 디버깅](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>를 사용하여 캐시를 계속 무효화해야 하는 경우 [!UICONTROL 클라이언트 라이브러리 다시 빌드] 모든 클라이언트 라이브러리를 한 번에 다시 빌드하는 것이 좋습니다. 이 작업은 약 15분 정도 소요될 수 있지만 일반적으로 나중에 발생하는 캐싱 문제를 제거합니다.
