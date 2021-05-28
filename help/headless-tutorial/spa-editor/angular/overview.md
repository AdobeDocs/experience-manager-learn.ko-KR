---
title: AEM SPA 편집기 및 Angular 시작하기
description: AEM에서 WKND SPA을 사용하여 Adobe Experience Manager에서 편집할 수 있는 첫 번째 Angular Single Page Application(SPA)을 만들 수 있습니다. AEM SPA Editor에서 Angular JS 프레임워크를 사용하여 SPA을 생성하는 방법을 알아봅니다. 이 여러 부분으로 구성된 튜토리얼에서는 가상 라이프스타일 브랜드인 WKND에 대한 Angular 애플리케이션을 구현하는 과정을 설명합니다. 이 자습서에서는 SPA 만들기 종단간 및 AEM과의 통합을 다룹니다.
sub-product: 사이트
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA 편집기
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 7%

---


# AEM에서 첫 번째 Angular SPA 만들기 {#introduction}

Adobe Experience Manager(AEM)의 **SPA Editor** 기능을 처음 사용하는 개발자를 위해 고안된 여러 부분으로 된 자습서를 시작합니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대한 Angular 애플리케이션 구현을 설명합니다. angular 앱은 개발 및 배포하도록 설계되며, Angular 구성 요소를 AEM 구성 요소에 매핑합니다. AEM에 배포된 완료된 SPA은 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 다중 부분 자습서의 목표는 SPA의 편집기 기능을 사용하여 작업하는 Angular 애플리케이션을 구현하는 방법을 개발자에게 가르치는 것입니다. 실제 시나리오에서는 개발 활동이 성향별로 분류되는데, 여기에는 **프런트 엔드 개발자** 및 **백 엔드 개발자**&#x200B;가 포함됩니다. Adobe는 AEM SPA 편집기 프로젝트에 참여할 모든 개발자가 이 자습서를 완료하는 것이 유익하다고 생각합니다.

이 자습서는 **AEM as a1/>Cloud Service으로 작동하도록 설계되었으며** AEM 6.5.4+**및** AEM 6.4.8+**와 이전 버전과 호환됩니다.** SPA은 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA 편집기](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [코어 구성 요소](https://docs.adobe.com/content/help/ko/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*자습서의 각 부분을 통과하는 데 1~2시간을 예상합니다.*

## 최신 코드

모든 자습서 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 전제 조건

이 자습서를 시작하기 전에 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식입니다
* [Angular](https://angular.io/)에 대한 기본 친숙함
* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk),  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  또는  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*필수는 아니지만 기존  [AEM Sites 구성 요소 개발에 대한 기본 이해를 갖는 것이 좋습니다](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)를 IDE로 사용하여 Mac OS 환경에서 실행되는 Cloud Service SDK로 AEM을 사용하여 캡처됩니다. 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적이어야 합니다.

>[!NOTE]
>
> **AEM as a Cloud Service을 처음 사용하십니까?** AEM as a  [Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하려면 다음 안내서를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하십니까?** 로컬 개발 환경을  [설정하려면 다음 안내서를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## 다음 단계 {#next-steps}

뭘 기다리고 있는 거야?![SPA Editor 프로젝트](create-project.md) 장으로 이동하여 자습서를 시작하고 AEM Project Archetype을 사용하여 SPA Editor 지원 프로젝트를 생성하는 방법을 알아봅니다.

## 이전 버전과의 호환성 {#compatibility}

이 자습서의 프로젝트 코드는 AEM as a Cloud Service용으로 빌드되었습니다. 프로젝트 코드를 **6.5.4+** 및 **6.4.8+**&#x200B;에 대해 이전 버전과 호환되도록 하기 위해 몇 가지 수정 사항이 수행되었습니다.

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;이 종속으로 포함되어 있습니다.

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

Target AEM 6.x 환경에 빌드를 수정하기 위해 `classic` 이름의 추가 Maven 프로필이 추가되었습니다.

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

기본적으로 `classic` 프로필이 비활성화됩니다. AEM 6.x와 함께 자습서를 따르는 경우 Maven 빌드를 수행하라는 지시가 있을 때마다 `classic` 프로필을 추가하십시오.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현에 대한 새 프로젝트를 생성할 때 항상 최신 버전의 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을 사용하고 `aemVersion`를 업데이트하여 의도한 AEM 버전을 타깃팅하십시오.
