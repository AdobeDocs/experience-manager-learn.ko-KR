---
title: AEM SPA 편집기 및 Angular 시작하기
description: WKND SPA을 사용하여 AEM, Adobe Experience Manager에서 편집할 수 있는 첫 번째 Angular 단일 페이지 애플리케이션(SPA)을 만듭니다. AEM SPA Editor를 사용하여 Angular JS 프레임워크를 사용하여 SPA을 만드는 방법을 살펴봅니다. 이 여러 부분으로 구성된 튜토리얼은 가상 라이프스타일 브랜드인 WKND에 대한 Angular 애플리케이션 구현을 안내합니다. 이 자습서에서는 SPA의 전체 생성 및 AEM와의 통합에 대해 설명합니다.
sub-product: 사이트
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 7%

---


# AEM {#introduction}에서 첫 번째 Angular SPA 만들기

AEM(Adobe Experience Manager)의 **SPA Editor** 기능에 대해 처음 사용하는 개발자를 위해 고안된 여러 부분으로 구성된 자습서입니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대한 Angular 응용 프로그램을 구현합니다. angular 앱은 Angular 구성 요소를 AEM 구성 요소에 매핑하는 AEM SPA Editor를 사용하여 배포되도록 개발 및 설계되었습니다. AEM에 배포된 완성된 SPA은 AEM의 기존 인라인 편집 툴을 사용하여 동적으로 제작할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 여러 부분으로 구성된 자습서의 목적은 개발자에게 AEM의 SPA 편집기 기능과 함께 작동하도록 Angular 애플리케이션을 구현하는 방법을 교육하는 것입니다. 실제 시나리오에서 개발 활동은 가상 사용자로 분류되며, 종종 **프런트 엔드 개발자** 및 **백엔드 개발자**&#x200B;이 포함됩니다. AEM SPA 편집기 프로젝트에 참여하여 이 튜토리얼을 완료하는 것은 모든 개발자에게 이로운 작업이라고 생각합니다.

이 자습서는 **AEM을 Cloud Service**&#x200B;로 사용하도록 설계되었으며 **AEM 6.5.4+** 및 **AEM 6.4.8+**&#x200B;와 역호환됩니다. SPA은 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA 편집기](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*자습서의 각 부분을 통과하는 데 1-2시간을 예상합니다.*

## 최신 코드

모든 자습서 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에서 찾을 수 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 전제 조건

이 튜토리얼을 시작하기 전에 다음을 수행해야 합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* [Angular](https://angular.io/)에 대한 기본 이해
* [AEM을 Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk),  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  또는  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*필수 사항은 아니지만 기존의 AEM Sites 구성 요소 [를 개발하는 방법에 대한 기본적인 이해를 가지는 것이 좋습니다](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)가 IDE로 Mac OS 환경에서 실행되는 Cloud Service SDK로 AEM을 사용하여 캡처됩니다. 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

>[!NOTE]
>
> **Cloud Service으로 AEM을 처음 사용하시나요?** AEM을 Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하는 방법에 대한  [다음 가이드를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하시나요?** 로컬 개발 환경을 설정하는 방법은  [다음 안내서를 참조하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## 다음 단계 {#next-steps}

뭘 기다리고 있어?![SPA Editor 프로젝트](create-project.md) 장으로 이동하여 AEM 프로젝트 원형을 사용하여 SPA Editor 지원 프로젝트를 생성하는 방법을 배워서 자습서를 시작합니다.

## 이전 버전과의 호환성 {#compatibility}

이 튜토리얼에 대한 프로젝트 코드는 Cloud Service으로 AEM용으로 구축되었습니다. 프로젝트 코드가 **6.5.4+** 및 **6.4.8+**&#x200B;에 대해 호환되게 하기 위해 몇 가지 수정이 이루어졌습니다.

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;이(가) 종속성으로 포함되어 있습니다.

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

대상 AEM 6.x 환경에 대한 빌드를 수정하기 위해 `classic`이라는 추가 Maven 프로필이 추가되었습니다.

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

기본적으로 `classic` 프로필은 비활성화됩니다. AEM 6.x에서 자습서를 따르는 경우 Maven 빌드를 수행하도록 명령할 때마다 `classic` 프로필을 추가하십시오.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현을 위해 새 프로젝트를 생성할 때는 항상 최신 버전의 [AEM Project Preanype](https://github.com/adobe/aem-project-archetype)을 사용하고 `aemVersion`를 업데이트하여 의도한 AEM 버전을 대상으로 합니다.
