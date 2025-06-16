---
title: AEM SPA 편집기 및 Angular 시작하기
description: Adobe Experience Manager(AEM)에서 WKND SPA를 사용하여 편집할 수 있는 첫 번째 Angular Single Page Application(SPA)을 개발합니다.
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 100%

---

# AEM에서 첫 번째 Angular SPA 만들기 {#introduction}

{{spa-editor-deprecation}}

Adobe Experience Manager(AEM)의 **SPA 편집기** 기능을 처음 사용하는 개발자를 위한 멀티 파트 튜토리얼을 시작합니다. 이 튜토리얼에서는 가상의 라이프스타일 브랜드인 WKND를 위한 React 애플리케이션을 구현하는 과정을 안내합니다. Angular 앱은 AEM의 SPA 편집기와 함께 배포되도록 개발 및 설계되었으며, Angular 구성 요소를 AEM 구성 요소에 매핑합니다. 완성된 SPA는 AEM에 배포되며 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 멀티 파트 튜토리얼의 목표는 개발자에게 AEM의 SPA 편집기 기능을 사용하여 Angular 애플리케이션을 구현하는 방법을 교육하는 것입니다. 실제 시나리오에서는 개발 활동이 특히 **프론트엔드 개발자**&#x200B;와 **백엔드 개발자**&#x200B;와 같은 페르소나별로 나뉘는 경우가 많습니다. 하지만 AEM SPA 편집기 프로젝트에 참여하는 모든 개발자가 이 튜토리얼을 완료하는 것이 유익할 것으로 생각합니다.

이 튜토리얼은 **AEM as a Cloud Service**&#x200B;와 함께 작동하도록 설계되었으며, **AEM 6.5.4 이상** 및 **AEM 6.4.8 이상** 버전과도 하위 호환됩니다. SPA는 다음을 사용해 구현됩니다.

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)
* [AEM SPA 편집기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=ko#content-editing-experience-with-spa)
* [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)
* [Angular](https://angular.io/)

*튜토리얼의 각 부분을 완료하는 데는 약 1~2시간이 소요됩니다.*

## 최신 코드

모든 튜토리얼 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에서 찾을 수 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 제공됩니다.

## 사전 요구 사항

이 튜토리얼을 시작하기 전에 다음이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* [Angular](https://angular.io/)에 대한 기본적인 이해
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4 이상](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html#65) 또는 [AEM 6.4.8 이상](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)&#x200B;(3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

*반드시 필요한 것은 아니지만 [기존 AEM Sites 구성 요소 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko)에 대한 기본적인 이해가 있으면 도움이 됩니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 튜토리얼을 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷과 비디오는 Mac OS 환경에서 [Visual Studio Code](https://code.visualstudio.com/)를 IDE로 사용하고 AEM as a Cloud Service SDK를 실행하여 캡처되었습니다. 달리 명시되지 않는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

>[!NOTE]
>
> **AEM as a Cloud Service를 처음 사용하십니까?** [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)를 확인하십시오.
>
> **AEM 6.5를 처음 사용하십니까?** [로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko)를 확인하십시오.

## 다음 단계 {#next-steps}

지금 바로 [SPA 편집기 프로젝트](create-project.md) 장으로 이동하여 AEM Project Archetype을 사용하여 SPA 편집기가 활성화된 프로젝트를 생성하는 방법을 알아보십시오.

## 이전 버전과의 호환성 {#compatibility}

이 튜토리얼의 프로젝트 코드는 AEM as a Cloud Service용으로 작성되었습니다. **6.5.4 이상** 및 **6.4.8 이상**&#x200B;과의 프로젝트 코드 하위 호환을 위해 몇 가지 수정이 적용되었습니다.

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=ko#what-is-the-uberjar) **v6.4.4** 종속성이 추가되었습니다.

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

AEM 6.x 환경을 대상으로 빌드 구성을 수정하기 위해 `classic`이라는 Maven 프로필이 추가되었습니다.

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

`classic` 프로필은 기본적으로 비활성화되어 있습니다. AEM 6.x을 사용하는 경우, Maven 빌드를 수행하라는 안내가 있을 때마다 `classic` 프로필을 추가하시기 바랍니다.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현을 위한 새 프로젝트를 생성할 때는 항상 최신 버전의 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을 사용하고, 의도한 AEM 버전에 맞도록 `aemVersion`을 업데이트해야 합니다.
