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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 8%

---

# AEM에서 첫 번째 Angular SPA 만들기 {#introduction}

{{edge-delivery-services}}

Adobe Experience Manager(AEM)의 **SPA 편집기** 기능을 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼에 오신 것을 환영합니다. 이 튜토리얼에서는 가상 라이프스타일 브랜드인 WKND를 위한 Angular 애플리케이션의 구현 과정을 안내합니다. Angular 앱은 Angular 구성 요소를 AEM 구성 요소에 매핑하는 AEM의 SPA 편집기를 사용하여 배포되도록 개발 및 디자인되었습니다. AEM에 배포된 완성된 SPA는 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![최종 SPA 구현](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 멀티 파트 튜토리얼의 목표는 개발자에게 AEM의 SPA 편집기 기능을 사용하여 작동할 Angular 애플리케이션을 구현하는 방법을 가르치는 것입니다. 실제 시나리오에서는 개발 활동이 성향별로 분류되며, 종종 **프론트엔드 개발자** 및 **백엔드 개발자**&#x200B;와 관련되어 있습니다. AEM SPA Editor 프로젝트와 관련된 개발자가 이 자습서를 완료하는 것이 유용할 것으로 생각합니다.

이 자습서는 **AEM as a Cloud Service**&#x200B;에서 작동하도록 설계되었으며 **AEM 6.5.4+** 및 **AEM 6.4.8+**&#x200B;과(와) 역으로 호환됩니다. SPA는 다음을 사용하여 구현됩니다.

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)
* [AEM SPA 편집기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=ko#content-editing-experience-with-spa)
* [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)
* [Angular](https://angular.io/)

*자습서의 각 부분을 완료하는 데 1~2시간을 예상하십시오.*

## 최신 코드

모든 튜토리얼 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에서 찾을 수 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 시작하려면 먼저 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* [Angular](https://angular.io/)에 대한 기본 친숙도
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html#65) 또는 [AEM 6.4.8+](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)&#x200B;(3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

*필요하지 않지만 [기존 AEM Sites 구성 요소 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR)에 대한 기본적인 이해를 하는 것이 좋습니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷과 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)이(가) IDE로 있는 Mac OS 환경에서 실행되는 AEM as a Cloud Service SDK을 사용하여 캡처됩니다. 명령과 코드는 별도로 명시하지 않는 한 로컬 운영 체제와 독립적이어야 합니다.

>[!NOTE]
>
> AEM as a Cloud Service을 처음 사용하십니까?**&#x200B;** AEM as a Cloud Service SDK을 사용하여 로컬 개발 환경을 설정하는 방법에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko).
>
> **AEM 6.5를 처음 사용하십니까?** 로컬 개발 환경 설정에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko).

## 다음 단계 {#next-steps}

무엇을 기다리고 있습니까?! [SPA 편집기 프로젝트](create-project.md) 장으로 이동하여 자습서를 시작하고 AEM Project Archetype을 사용하여 SPA 편집기 사용 프로젝트를 생성하는 방법을 알아봅니다.

## 이전 버전과의 호환성 {#compatibility}

이 자습서의 프로젝트 코드는 AEM as a Cloud Service용으로 빌드되었습니다. 프로젝트 코드가 **6.5.4+** 및 **6.4.8+**&#x200B;에 대해 역으로 호환되도록 몇 가지 수정되었습니다.

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=ko#what-is-the-uberjar) **v6.4.4**&#x200B;이(가) 종속성으로 포함되었습니다.

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

대상 AEM 6.x 환경으로 빌드를 수정하기 위해 이름이 `classic`인 추가 Maven 프로필이 추가되었습니다.

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

`classic` 프로필은 기본적으로 비활성화되어 있습니다. AEM 6.x를 사용하여 자습서를 따르는 경우 Maven 빌드를 수행하도록 지시받을 때마다 `classic` 프로필을 추가하십시오.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM 구현을 위해 새 프로젝트를 생성할 때 항상 최신 버전의 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을(를) 사용하고 `aemVersion`을(를) 업데이트하여 의도한 버전의 AEM을 대상으로 합니다.
