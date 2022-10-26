---
title: AEM SPA 편집기 및 Angular 시작하기
description: Adobe Experience Manager(AEM)에서 WKND SPA를 사용하여 편집할 수 있는 첫 번째 Angular Single Page Application(SPA)을 개발합니다.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 12%

---

# AEM에서 첫 번째 Angular SPA 만들기 {#introduction}

을(를) 처음 사용하는 개발자를 위해 디자인된 여러 부분으로 된 자습서를 시작합니다 **SPA 편집기** Adobe Experience Manager(AEM)의 기능입니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대한 Angular 애플리케이션 구현을 설명합니다. angular 앱은 Angular 구성 요소를 AEM 구성 요소에 매핑하는 AEM SPA 편집기로 개발 및 배포하도록 설계되었습니다. AEM에 배포된 완료된 SPA은 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 다중 부분 자습서의 목표는 SPA의 편집기 기능을 사용하여 작업하는 Angular 애플리케이션을 구현하는 방법을 개발자에게 가르치는 것입니다. 실제 시나리오에서 개발 활동은 종종 다음을 포함하는 성향별로 분류됩니다 **프런트엔드 개발자** 그리고 **백엔드 개발자**. AEM SPA 편집기 프로젝트에 참여하는 모든 개발자가 이 자습서를 완료하는 것이 도움이 된다고 생각합니다.

이 튜토리얼은 **AEM as a Cloud Service** 및 는 이전 버전과 호환됩니다. **AEM 6.5.4+** 및 **AEM 6.4.8+**. SPA은 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA 편집기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*자습서의 각 부분을 통과하는 데 1~2시간을 예상합니다.*

## 최신 코드

모든 자습서 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

다음 [최신 코드 기본](https://github.com/adobe/aem-guides-wknd-spa/releases) 는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 시작하기 전에 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식입니다
* 에 대한 기본 정보 [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 또는 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

*필수는 아니지만, 기본적인 이해를 하는 것이 유익하다 [기존 AEM Sites 구성 요소 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/) IDE로 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적이어야 합니다.

>[!NOTE]
>
> **AEM as a Cloud Service을 처음 사용하십니까?** 다음을 확인하십시오 [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 데 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하십니까?** 다음을 확인하십시오 [로컬 개발 환경 설정에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko-KR).

## 다음 단계 {#next-steps}

뭘 기다리고 있는 거야?! 로 이동하여 자습서를 시작합니다 [SPA 편집기 프로젝트](create-project.md) AEM 프로젝트 원형 을 사용하여 SPA 편집기 지원 프로젝트를 생성하는 방법을 장 및 알아봅니다.

## 이전 버전과의 호환성 {#compatibility}

이 자습서의 프로젝트 코드는 AEM as a Cloud Service용으로 빌드되었습니다. 프로젝트 코드를 **6.5.4+** 및 **6.4.8+** 몇 가지 사항이 수정되었습니다.

다음 [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 은 종속성으로 포함되어 있습니다.

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

이름이 지정된 추가 Maven 프로필 `classic` target AEM 6.x 환경에 빌드를 수정하기 위해 가 추가되었습니다.

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

다음 `classic` 프로필은 기본적으로 비활성화되어 있습니다. AEM 6.x에서 자습서를 따르는 경우 `classic` Maven 빌드를 수행하는 것이 지시될 때마다 프로필이 나열됩니다.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현에 대한 새 프로젝트를 생성할 때는 항상 최신 버전의 를 사용하십시오 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 그리고 `aemVersion` 타깃팅할 수 있습니다.
