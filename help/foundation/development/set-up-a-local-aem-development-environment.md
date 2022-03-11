---
title: 로컬 AEM 개발 환경 설정
description: AEM, Adobe Experience Manager에 대한 로컬 개발 설정에 대한 안내서입니다. 로컬 설치, Apache Maven, 통합 개발 환경 및 디버깅/문제 해결에 대한 중요한 항목을 다룹니다. Eclipse IDE, CRXDE-Lite, Visual Studio 코드 및 IntelliJ를 사용한 개발에 대해 설명합니다.
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 1%

---

# 로컬 AEM 개발 환경 설정

AEM, Adobe Experience Manager에 대한 로컬 개발 설정에 대한 안내서입니다. 로컬 설치, Apache Maven, 통합 개발 환경 및 디버깅/문제 해결에 대한 중요한 항목을 다룹니다. 개발 **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] 및[!DNL IntelliJ]** 이 논의될 것입니다.

## 개요

로컬 개발 환경을 설정하는 것은 Adobe Experience Manager 또는 AEM용으로 개발할 때 첫 번째 단계입니다. 시간을 내어 품질 개발 환경을 설정하여 생산성을 높이고 코드를 보다 신속하게 작성할 수 있습니다. AEM 로컬 개발 환경을 4가지 영역으로 나눌 수 있습니다.

* 로컬 AEM 인스턴스
* [!DNL Apache Maven] 프로젝트
* 통합 개발 환경(IDE)
* 문제 해결

## 로컬 AEM 인스턴스 설치

로컬 AEM 인스턴스를 참조할 때 개발자의 개인 컴퓨터에서 실행 중인 Adobe Experience Manager 사본에 대해 이야기하고 있습니다. ***모두*** AEM 개발은 로컬 AEM 인스턴스에 대해 코드를 작성하고 실행하는 것으로 시작해야 합니다.

AEM을 처음 사용하는 경우 두 가지 기본 실행 모드를 설치할 수 있습니다. ***작성자*** 및 ***게시***. 다음 ***작성자*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  는 디지털 마케터가 컨텐츠를 만들고 관리하는 데 사용할 환경입니다. 개발 시 **가장** 작성자 인스턴스에 코드를 배포할 때입니다. 구성 요소를 추가 및 구성할 수 있을 뿐만 아니라 새 페이지를 만들 수도 있습니다. AEM Sites은 WYSIWYG 작성 CMS이므로 대부분의 CSS 및 JavaScript를 작성 인스턴스에 대해 테스트할 수 있습니다.

또한 *중요* 로컬 테스트 코드 ***게시*** 인스턴스. 다음 ***게시*** 인스턴스는 웹 사이트 방문자가 상호 작용하는 AEM 환경입니다. 반면에 ***게시*** 인스턴스는 ***작성자*** 예를 들어 구성 및 권한과 관련된 몇 가지 주요 차이점이 있습니다. 코드가 *항상* 그 지역에 대해 시험하다 ***게시*** 더 높은 수준 환경으로 승격되기 전의 인스턴스입니다.

### 단계

1. 확인 [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) 가 설치되어 있습니다.
   * 기본 설정 [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atologing&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) AEM 6.5 이전 AEM 버전용
2. 의 사본 가져오기 [AEM QuickStart Jar 및 [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. 다음과 같이 컴퓨터에 폴더 구조를 만듭니다.

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 이름 바꾸기 [!DNL QuickStart] JAR에서 로 ***aem-author-p4502.jar*** 그리고 그 밑에 놓으시면 `/author` 디렉토리. 추가 ***[!DNL license.properties]*** 파일 아래 `/author` 디렉토리.
5. 의 사본 만들기 [!DNL QuickStart] JAR, 이름을 로 변경합니다. ***aem-publish-p4503.jar*** 그리고 그 밑에 놓으시면 `/publish` 디렉토리. 의 사본 추가 ***[!DNL license.properties]*** 파일 아래 `/publish` 디렉토리.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 를 두 번 클릭합니다. ***aem-author-p4502.jar*** 설치할 파일 **작성자** 인스턴스. 포트에서 실행되는 작성자 인스턴스가 시작됩니다 **4502년** 로컬 컴퓨터에 있는 Analytics Mobile Apps 또는 Analytics Premium에서 사용할 수 없습니다.

   를 두 번 클릭합니다. ***aem-publish-p4503.jar*** 설치할 파일 **게시** 인스턴스. 포트에서 실행되는 게시 인스턴스가 시작됩니다 **4503년** 로컬 컴퓨터에 있는 Analytics Mobile Apps 또는 Analytics Premium에서 사용할 수 없습니다.

   >[!NOTE]
   >
   >개발 시스템의 하드웨어에 따라 두 가지 모두 **작성자 및 게시** 인스턴스가 동시에 실행됩니다. 로컬 설정에서 둘 다 동시에 실행할 필요가 거의 없습니다.

   자세한 내용은 [AEM 인스턴스 배포 및 유지 관리](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Apache Maven 설치

***[!DNL Apache Maven]*** 는 Java 기반 프로젝트에 대한 빌드 및 배포 절차를 관리하는 도구입니다. AEM은 Java 기반 플랫폼이며 [!DNL Maven] 는 AEM 프로젝트에 대한 코드를 관리하는 표준 방법입니다. 우리가 말할 때 ***AEM Maven 프로젝트*** 또는 ***AEM 프로젝트***, 다음을 모두 포함하는 Maven 프로젝트를 참조합니다. *사용자 지정* 코드에 연결할 수도 있습니다.

모든 AEM 프로젝트는 최신 버전의 **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). 다음 [!DNL AEM Project Archetype] 은 일부 샘플 코드와 컨텐츠로 AEM 프로젝트의 부트스트랩을 만듭니다. 다음 [!DNL AEM Project Archetype] 또한 **[!DNL AEM WCM Core Components]** 프로젝트에서 사용할 수 있도록 구성되었습니다.

>[!CAUTION]
>
>새 프로젝트를 시작할 때 최신 버전의 Archetype을 사용하는 것이 좋습니다. AEM에는 여러 버전의 기본 버전이 있으며 일부 버전은 이전 버전과 호환되지 않습니다.

### 단계

1. 다운로드 [Apache Maven](https://maven.apache.org/download.cgi)
2. 설치 [Apache Maven](https://maven.apache.org/install.html) 설치 작업이 명령줄에 추가되었는지 확인합니다. `PATH`.
   * [!DNL macOS] 사용자는 [홈브루](https://brew.sh/)
3. 확인 **[!DNL Maven]** 는 새 명령줄 터미널을 열고 다음을 실행하여 설치됩니다.

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
   > 과거에 `adobe-public` Maven 프로필을 가리키는 데 필요합니다. `nexus.adobe.com` AEM 객체를 다운로드하려면 다음을 수행하십시오. 이제 Maven Central 및 `adobe-public` 프로필이 필요하지 않습니다.

## 통합 개발 환경 설정

통합 개발 환경 또는 IDE는 텍스트 편집기, 구문 지원 및 빌드 도구를 결합하는 애플리케이션입니다. 개발 유형에 따라 다른 IDE보다 한 IDE가 더 선호될 수 있습니다. IDE에 관계없이 주기적으로 IDE를 사용할 수 있어야 합니다 ***푸시*** 코드를 로컬 AEM 인스턴스에 매핑하여 테스트할 수 있습니다. 때때로 그것은 또한 중요할 것입니다 ***가져오기*** Git과 같은 소스 제어 관리 시스템으로 유지하기 위해 로컬 AEM 인스턴스에서 AEM 프로젝트에 대한 구성.

다음은 로컬 AEM 인스턴스와의 통합을 보여주는 해당 비디오와 함께 AEM 개발에 사용되는 몇 가지 인기 있는 IDE입니다.

>[!NOTE]
>
> WKND 프로젝트가 AEM as a Cloud Service에서 작동하도록 기본적으로 업데이트되었습니다. 업데이트 날짜: [이전 버전 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 Maven 명령에 추가합니다.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDE를 사용할 때는 반드시 확인하십시오 `classic` maven 프로필 탭에서 다음을 수행합니다.

![Maven 프로필 탭](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven 프로필*

### [!DNL Eclipse] IDE

다음 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 는 오픈 소스 및 오픈 소스이므로 Java 개발을 위해 가장 많이 사용되는 IDE 중 하나입니다 ***무료***! Adobe은 플러그인을 제공하며, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**&#x200B;에 대해 [!DNL Eclipse] 코드를 로컬 AEM 인스턴스와 동기화할 수 있도록 좋은 GUI를 사용하여 쉽게 개발할 수 있습니다. 다음 [!DNL Eclipse] AEM을 처음 사용하는 개발자에게는 IDE가 권장됩니다. 이는 GUI를 지원하기 때문입니다 [!DNL AEM Developer Tools].

#### 설치 및 설정

1. 를 다운로드하여 설치합니다. [!DNL Eclipse] IDE 대상 [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. 지침에 따라 을 설치합니다 [!DNL AEM Developer Tools] 플러그인: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 01:24 - Maven을 사용하여 소스 코드 빌드 및 배포
* 04:33 - AEM 개발자 도구를 사용한 푸시 코드 변경
* 10:55 - AEM 개발자 도구를 사용하여 코드 변경 사항 가져오기
* 13:12 - Eclipse의 통합 디버깅 도구 사용

### IntelliJ IDEA

다음 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 는 전문적인 Java 개발을 위한 강력한 IDE입니다. [!DNL IntelliJ IDEA] 두 가지 맛이 있는데 ***무료*** [!DNL Community] 에디션 및 상업(유료) [!DNL Ultimate] 버전. 무료 [!DNL Community] 버전 [!DNL IntellIJ IDEA] 는 더 많은 AEM 개발을 위해 충분하지만, [!DNL Ultimate] [기능 집합을 확장합니다.](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. 를 다운로드하여 설치합니다. [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 설치 [!DNL Repo] (명령줄 도구): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Maven 프로젝트 가져오기
* 05:47 - Maven을 사용하여 소스 코드 빌드 및 배포
* 08:17 - Repo를 사용한 변경 내용 푸시
* 14:39 - Repo를 사용하여 변경 사항 가져오기
* 17:25 - IntelliJ IDEA의 통합 디버깅 도구 사용

### [!DNL Visual Studio Code]

**[Visual Studio 코드](https://code.visualstudio.com/)** 는 빠르게 을 위한 즐겨찾는 도구가 되었습니다 ***프런트엔드 개발자*** 향상된 JavaScript 지원, [!DNL Intellisense], 및 브라우저 디버깅 지원. **[!DNL Visual Studio Code]** 는 오픈 소스이며, 자유롭게 사용할 수 있으며, 강력한 확장이 있습니다. [!DNL Visual Studio Code] Adobe 도구의 도움으로 AEM과 통합하도록 설정할 수 있습니다. **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** AEM과 통합하도록 설치할 수 있는 몇 가지 커뮤니티에서 지원하는 확장도 있습니다.

[!DNL Visual Studio Code] 는 주로 AEM 클라이언트 라이브러리를 만들기 위해 CSS/LESS 및 JavaScript 코드를 작성하는 프런트 엔드 개발자에게 적합합니다. 노드 정의(대화 상자, 구성 요소)는 모두 원시 XML로 편집해야 하므로 이 도구는 새 AEM 개발자에게 가장 적합한 방법이 아닐 수 있습니다. 에 사용할 수 있는 몇 가지 Java 확장이 있습니다 [!DNL Visual Studio Code]그러나 주로 Java 개발을 수행하는 경우 [!DNL Eclipse IDE] 또는 [!DNL IntelliJ] 선호될 수 있습니다.

#### 중요 링크

* [**다운로드**](https://code.visualstudio.com/Download) **Visual Studio 코드**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR 컨텐츠를 위한 FTP와 유사한 도구
* **[aemfed](https://aemfed.io/)** - AEM 프런트엔드 워크플로우 속도 향상
* **[AEM 동기화](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Visual Studio 코드에 대해 커뮤니티 지원* 확장

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Maven 프로젝트 가져오기
* 00:53 - Maven을 사용하여 소스 코드 빌드 및 배포
* 04:03 - Repo 명령줄 도구를 사용하여 푸시 코드 변경
* 08:29 - Repo 명령줄 도구를 사용하여 코드 변경 내용 가져오기
* 10:40 - aemed 도구를 사용하여 코드 변경 푸시
* 14:24 - 문제 해결, 클라이언트 라이브러리 다시 작성

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) AEM 저장소의 브라우저 기반 보기입니다. [!DNL CRXDE Lite] 가 AEM에 포함되어 있으므로 개발자가 파일 편집, 구성 요소 정의, 대화 상자 및 템플릿과 같은 표준 개발 작업을 수행할 수 있습니다. [!DNL CRXDE Lite] is ***not*** 는 전체 개발 환경이지만 디버깅 도구로 매우 효과적입니다. [!DNL CRXDE Lite] 는 코드 베이스의 외부에서 제품 코드를 확장하거나 간단히 이해할 때 유용합니다. [!DNL CRXDE Lite] 는 리포지토리에 대한 강력한 보기를 제공하고 권한을 효과적으로 테스트 및 관리하는 방법을 제공합니다.

[!DNL CRXDE Lite] 는 코드를 테스트하고 디버깅하기 위해 항상 다른 IDE와 함께 사용해야 하지만 기본 개발 도구로 사용해서는 안 됩니다. 여기에는 제한된 구문 지원, 자동 완료 기능 없음, 소스 제어 관리 시스템과의 제한된 통합 기능이 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 문제 해결

***도움말!*** 코드가 작동하지 않습니다! 모든 개발 상태에서 마찬가지로 코드가 예상대로 작동하지 않는 경우가 있을 수 있습니다. AEM은 강력한 플랫폼이지만 강력한 성능을 갖춘 매우 복잡한 플랫폼입니다. 다음은 문제 해결 및 추적 측면에서 볼 때 몇 가지 높은 수준의 시작 지점입니다(하지만 잘못될 수 있는 완전한 목록은 아님).

### 코드 배포 확인

문제가 발생할 때 첫 번째 단계는 코드가 성공적으로 AEM에 배포되어 설치되었는지 확인하는 것입니다.

1. **확인 [!UICONTROL 패키지 관리자]** 코드 패키지가 업로드 및 설치되었는지 확인하려면: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 최근에 패키지가 설치되었는지 확인하려면 타임스탬프를 확인하십시오.
1. 다음과 같은 도구를 사용하여 증분 파일 업데이트를 수행하는 경우 [!DNL Repo] 또는 [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** 파일이 로컬 AEM 인스턴스에 푸시되고 파일 내용이 업데이트되는지 확인합니다. [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **번들이 업로드되었는지 확인합니다** osgI 번들에 Java 코드와 관련된 문제가 표시되는 경우. 를 엽니다. [!UICONTROL Adobe Experience Manager 웹 콘솔]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 번들을 검색합니다. 번들에 **[!UICONTROL 활성]** 상태. 의 번들 문제 해결과 관련된 자세한 내용은 아래를 참조하십시오. **[!UICONTROL 설치됨]** state.

#### 로그 확인

AEM은 채팅 플랫폼이며 **error.log**. 다음 **error.log** AEM이 설치된 위치를 찾을 수 있습니다. &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

문제를 추적하는 유용한 기법은 Java 코드에 로그 문을 추가하는 것입니다.

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

기본적으로 **error.log** 이(가) 로그하도록 구성되어 있습니다. *[!DNL INFO]* 문. 로그 수준을 변경하려면 [!UICONTROL 로그 지원]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 또한 **error.log** 너무 수다스러워요 를 사용할 수 있습니다 [!UICONTROL 로그 지원] 를 입력하여 지정된 Java 패키지에 대한 로그 명령문을 구성할 수 있습니다. 사용자 지정 코드 문제를 OOTB AEM 플랫폼 문제와 쉽게 구분하기 위해 프로젝트에 가장 좋습니다.

![AEM에서 구성 로깅](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 번들이 설치됨 상태입니다. {#bundle-active}

모든 번들(조각 제외)은 **[!UICONTROL 활성]** state. 코드 번들이 [!UICONTROL 설치됨] state에서 해결해야 하는 문제가 있습니다. 대부분의 경우 종속성 문제입니다.

![AEM의 번들 오류](assets/set-up-a-local-aem-development-environment/bundle-error.png)

위의 스크린샷에서는 [!DNL WKND Core bundle] is [!UICONTROL 설치됨] state. 이는 번들에 다른 버전의 가 필요하기 때문입니다 `com.adobe.cq.wcm.core.components.models` AEM 인스턴스에서 사용할 수 있는 것입니다.

사용할 수 있는 유용한 도구는 [!UICONTROL 종속성 파인더]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Java 패키지 이름을 추가하여 AEM 인스턴스에서 사용할 수 있는 버전을 검사합니다.

![코어 구성 요소](assets/set-up-a-local-aem-development-environment/core-components.png)

위의 예를 계속 선택하면 AEM 인스턴스에 설치된 버전이 **12.2** vs **12.6** 그 다발이 기대되어있는 것. 여기서 뒤로 이동하여 [!DNL Maven] AEM에 대한 종속성이 일치함 [!DNL Maven] AEM 프로젝트에 대한 종속성. 위의 예에서 [!DNL Core Components] **v2.2.0** 가 AEM 인스턴스에 설치되어 있지만 코드 번들은 **v2.2.2**&#x200B;따라서 종속성 문제의 원인이 됩니다.

#### Sling 모델 등록 확인 {#osgi-component-sling-models}

AEM 구성 요소는 항상 [!DNL Sling Model] 비즈니스 로직을 캡슐화하고 HTL 렌더링 스크립트가 깨끗한지 확인합니다. Sling 모델을 찾을 수 없는 문제가 발생하는 경우 다음을 확인하는 것이 도움이 될 수 있습니다 [!DNL Sling Models] 콘솔에서: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Sling 모델이 등록되었는지 여부 및 어떤 리소스 유형(구성 요소 경로)에 연결되어 있는지 알 수 있습니다.

![Sling 모델 상태](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

등록 [!DNL Sling Model], `BylineImpl` 구성 요소 리소스 유형에 연결되어 있습니다. `wknd/components/content/byline`.

#### CSS 또는 JavaScript 문제

대부분의 CSS 및 JavaScript 문제의 경우 브라우저의 개발 도구를 사용하는 것이 문제를 해결하는 가장 효과적인 방법입니다. AEM 작성자 인스턴스에 대해 개발할 때 문제를 좁히려면 페이지를 &quot;게시됨으로&quot;보는 것이 좋습니다.

![CSS 또는 JS 문제](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

를 엽니다. [!UICONTROL 페이지 속성] 메뉴를 클릭하고 [!UICONTROL 게시됨으로 보기]. 이렇게 하면 AEM 편집기 없이 쿼리 매개 변수가 로 설정된 채 페이지가 열립니다 **wcmmode=disabled**. 이렇게 하면 AEM 작성 UI가 효과적으로 비활성화되고 프런트 엔드 문제를 훨씬 쉽게 문제 해결/디버깅할 수 있습니다.

프런트 엔드 코드 개발 시 오래된 CSS/JS가 로드되거나 오래된 다른 문제가 자주 발생합니다. 첫 번째 단계로 브라우저 기록이 지워졌는지, 필요한 경우 시크릿 브라우저 또는 새 세션을 시작해야 합니다.

#### 클라이언트 라이브러리 디버깅

여러 가지 카테고리 및 포함 방법을 사용하여 여러 클라이언트 라이브러리를 포함하려면 문제를 해결하는 것이 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 표시합니다. 가장 중요한 도구 중 하나는 [!UICONTROL 클라이언트 라이브러리 다시 작성] 이렇게 하면 AEM에서 LESS 파일을 다시 컴파일하고 CSS를 생성합니다.

* [Libs 덤프](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 모든 클라이언트 라이브러리를 나열합니다. &lt;host>/libs/granite/ui/content/dumplibs.html
* [테스트 출력](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 사용자가 카테고리를 기반으로 clientlib의 예상 HTML 출력을 볼 수 있습니다. &lt;host>/libs/granite/ui/content/dumplibs.test
* [라이브러리 종속성 유효성 검사](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. &lt;host>/libs/granite/ui/content/dumplibs.validate
* [클라이언트 라이브러리 다시 작성](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 모든 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM이 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS를 사용하여 개발할 때 특히 유용합니다. 일반적으로 캐시를 무효화한 다음 페이지 새로 고침을 수행하고 모든 라이브러리를 다시 빌드하는 것보다 더 효과적입니다. &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![Clientlibs 디버깅](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>을 사용하여 캐시를 지속적으로 무효화해야 하는 경우 [!UICONTROL 클라이언트 라이브러리 다시 작성] 도구를 사용하면 모든 클라이언트 라이브러리를 한 번 다시 빌드할 수 있습니다. 이 작업은 약 15분 정도 걸릴 수 있지만 일반적으로 나중에 캐싱 문제가 해결됩니다.
