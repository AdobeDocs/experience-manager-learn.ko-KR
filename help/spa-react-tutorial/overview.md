---
title: AEM SPA 편집기 시작 및 반응
description: WKND SPA를 사용하여 Adobe Experience Manager AEM에서 편집할 수 있는 간단한 SPA(Response Single Page Application)를 만들 수 있습니다. AEM SPA Editor를 사용하여 React JS 프레임워크를 사용하여 SPA를 만드는 방법을 살펴봅니다. 이 다중 부분 자습서는 가상의 라이프스타일 브랜드인 WKND에 대해 Responsive 응용 프로그램을 구현하는 과정을 안내합니다. 이 자습서에서는 SPA의 전체 생성 및 AEM과의 통합에 대해 설명합니다.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# AEM에서 반응형 SPA 만들기 {#overview}

Adobe Experience Manager(AEM)의 **SPA Editor** 기능을 처음 사용하는 개발자를 위해 마련된 다양한 기능의 자습서를 시작합니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대해 React 응용 프로그램의 구현을 안내합니다. React 앱은 개발되어 AEM SPA Editor와 함께 배포할 예정입니다. 이 경우 Reimate 구성 요소를 AEM 구성 요소에 매핑합니다. AEM에 배포된 완성된 SPA는 AEM의 기존 인라인 편집 툴을 사용하여 동적으로 제작될 수 있습니다.

![최종 SPA 구현](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 멀티 파트 튜토리얼의 목표는 AEM의 SPA Editor 기능을 사용하여 작동하는 Responsive 애플리케이션을 구현하는 방법을 개발자에게 교육하는 것입니다. 실제 시나리오에서 개발 활동은 **프런트 엔드 개발자** 및 **백 엔드 개발자가 관여하는 등 개인별로 분류됩니다**. AEM SPA Editor 프로젝트에 참여할 모든 개발자에게 이 튜토리얼을 완료하는 것이 유익하다고 생각합니다.

이 자습서는 **AEM에서 Cloud Service으로** 작동하도록 설계되었으며 **AEM 6.5.4+** 및 **AEM 6.4.8+**&#x200B;와 역호환됩니다. SPA는 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [반응형 JS](https://reactjs.org/)
* [반응형 앱 만들기](https://create-react-app.dev/)

*자습서의 각 부분을 통과하는 데 1-2시간을 예상합니다.*

## 최신 코드

모든 자습서 코드는 [GitHub에서 찾을 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa).

최신 [코드 베이스는](https://github.com/adobe/aem-guides-wknd-spa/releases) 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 전제 조건

이 자습서를 시작하기 전에 다음을 수행해야 합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* 반응형 [사용](https://reactjs.org/tutorial/tutorial.html)
* [AEM(Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 또는 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

*필수 사항은 아니지만 기존 AEM Sites 구성 요소[를 개발하는 것에 대한 기본적인 이해를 갖는 것이 좋다](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 AEM을 [Visual Studio 코드를 IDE로 Mac OS 환경에서 실행되는 Cloud Service SDK로](https://code.visualstudio.com/) 캡처됩니다. 별도의 언급이 없는 한 명령과 코드는 로컬 운영 체제와 독립되어야 합니다.

>[!NOTE]
>
> **AEM을 Cloud Service으로 처음 사용하는 경우** AEM을 Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하는 방법은 [다음 가이드를 참조하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하시나요?** 로컬 개발 환경을 설정하려면 [다음 가이드를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## 다음 단계 {#next-steps}

뭘 기다리는 거야?SPA Editor 프로젝트 [](create-project.md) 장으로 이동하여 AEM Project Tranype을 사용하여 SPA Editor 지원 프로젝트를 생성하는 방법을 학습하여 자습서를 시작합니다.

## 이전 버전과의 호환성 {#compatibility}

이 튜토리얼에 대한 프로젝트 코드는 AEM용으로 Cloud Service에 만들어졌습니다. 프로젝트 코드가 **6.5.4+** 및 **6.4.8+** 몇 개의 수정에 맞게 이전 버전에서 호환되도록 하기 위해 자습서의 POM 파일을 수정했습니다.

UberJar [](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) v6.4.4 **** 가 종속성으로 포함되었습니다.

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

AEM 6.x 환경을 대상으로 하는 빌드를 수정하기 위해 이름이 지정된 추가 Maven 프로필 `classic` 이 추가되었습니다.

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

기본적으로 프로필 `classic` 은 비활성화됩니다. AEM 6.x에서 자습서를 따르는 경우, Maven 빌드(예: `classic`

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현을 위한 새 프로젝트를 생성할 때는 항상 최신 버전의 [AEM Project Pretype](https://github.com/adobe/aem-project-archetype) 을 사용하고 대상 `aemVersion` 의 AEM 버전을 업데이트합니다.
