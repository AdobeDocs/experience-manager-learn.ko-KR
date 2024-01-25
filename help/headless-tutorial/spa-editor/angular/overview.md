---
title: AEM SPA 편집기 및 Angular 시작하기
description: Adobe Experience Manager(AEM)에서 WKND SPA를 사용하여 편집할 수 있는 첫 번째 Angular Single Page Application(SPA)을 개발합니다.
version: Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 152
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 8%

---

# AEM에서 첫 번째 Angular SPA 만들기 {#introduction}

{{edge-delivery-services}}

을 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼 시작 **SPA 편집기** Adobe Experience Manager(AEM)의 기능입니다. 이 튜토리얼에서는 가상 라이프스타일 브랜드인 WKND를 위한 Angular 애플리케이션의 구현 과정을 안내합니다. angular 앱은 Angular 구성 요소를 AEM 구성 요소에 매핑하는 AEM SPA Editor를 사용하여 배포하도록 개발 및 디자인되었습니다. AEM에 배포된 완성된 SPA은 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 멀티 파트 튜토리얼의 목표는 개발자에게 AEM의 SPA 편집기 기능을 사용하여 작업할 수 있도록 Angular 애플리케이션을 구현하는 방법을 가르치는 것입니다. 실제 시나리오에서는 개발 활동이 성향별로 분류되며, 종종 다음을 포함합니다. **프론트엔드 개발자** 및 a **백엔드 개발자**. AEM SPA Editor 프로젝트와 관련된 모든 개발자가 이 자습서를 완료하는 것이 유익하다고 생각합니다.

튜토리얼은 **AEM as a Cloud Service** 이전 버전과 호환 가능 **AEM 6.5.4+** 및 **AEM 6.4.8+**. SPA은 다음을 사용하여 구현됩니다.

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA 편집기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*자습서의 각 부분을 완료하는 데 1~2시간이 걸릴 것으로 예상합니다.*

## 최신 코드

모든 튜토리얼 코드는에서 찾을 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

다음 [최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases) 는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 시작하려면 먼저 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* 에 대한 기본 친숙도 [Angular](https://angular.io/)
* [AEM AS A CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 또는 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

*필수는 아니지만 의 기본 사항을 이해하는 것이 좋습니다. [기존 AEM Sites 구성 요소 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 를 사용하여 Mac OS 환경에서 실행되는 AEM as a Cloud Service SDK를 사용하여 캡처합니다 [Visual Studio 코드](https://code.visualstudio.com/) IDE로. 명령과 코드는 별도로 명시하지 않는 한 로컬 운영 체제와 독립적이어야 합니다.

>[!NOTE]
>
> **AEM을 as a Cloud Service으로 처음 사용하십니까?** 다음을 확인하십시오. [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서입니다](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR).
>
> **AEM 6.5를 처음 사용하십니까?** 다음을 확인하십시오. [로컬 개발 환경 설정에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko-KR).

## 다음 단계 {#next-steps}

무엇을 기다리고 있습니까?! 다음 위치로 이동하여 자습서를 시작하십시오. [SPA 편집기 프로젝트](create-project.md) 챕터 및 AEM Project Archetype을 사용하여 SPA 편집기 활성화 프로젝트를 생성하는 방법에 대해 알아봅니다.

## 이전 버전과의 호환성 {#compatibility}

이 자습서의 프로젝트 코드는 AEM용으로 as a Cloud Service으로 빌드되었습니다. 프로젝트 코드를 의 하위 호환성과 호환되도록 하려면 **6.5.4+** 및 **6.4.8+** 몇 가지 사항이 수정되었습니다.

다음 [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 이(가) 종속으로 포함되었습니다.

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

이름이 인 추가 Maven 프로필 `classic` 이(가) 빌드를 target AEM 6.x 환경으로 수정하기 위해 추가되었습니다.

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

다음 `classic` 프로필은 기본적으로 비활성화되어 있습니다. AEM 6.x에서 자습서를 따르는 경우 `classic` Maven 빌드를 수행하도록 지시받을 때마다 프로필:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현을 위해 새 프로젝트를 생성할 때 항상 최신 버전의 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 및 업데이트 `aemVersion` 대상 버전의 AEM을 타깃팅할 수 있습니다.
