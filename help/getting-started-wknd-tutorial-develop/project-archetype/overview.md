---
title: AEM Sites 시작하기 - Project Archetype
description: AEM Sites 시작하기 - Project Archetype WKND 튜토리얼은 여러 부분으로 구성된 튜토리얼로, Adobe Experience Manager를 처음 사용하는 개발자를 위해 설계되었습니다. 튜토리얼은 WKND라는 가상의 라이프스타일 브랜드를 위한 AEM 사이트의 구현 과정을 안내합니다. 튜토리얼은 프로젝트 설정, Maven Archetypes, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리, 구성 요소 개발과 같은 기본 주제를 다룹니다
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 74
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '414'
ht-degree: 100%

---

# AEM Sites 시작하기 - Project Archetype {#project-archetype}

{{traditional-aem}}

Adobe Experience Manager(AEM)를 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼을 시작합니다. 이 튜토리얼은 WKND의 가상 라이프스타일 브랜드를 위한 AEM 사이트의 구현 과정을 안내합니다.

이 튜토리얼은 먼저 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)을 사용하여 새로운 프로젝트를 생성합니다.

또한 이 튜토리얼은 **AEM as a Cloud Service**&#x200B;와 함께 작동하도록 설계되었으며 **AEM 6.5.14+**&#x200B;와도 하위 호환됩니다. 다음을 사용하여 사이트를 구현합니다.

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)
* [편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*튜토리얼의 각 부분을 완료하는 데는 약 1~2시간이 소요됩니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 튜토리얼을 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷과 비디오는 macOS 환경에서 [Visual Studio Code](https://code.visualstudio.com/)를 IDE로 사용하고 AEM as a Cloud Service SDK를 실행하여 캡처되었습니다. 달리 명시되지 않는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

### 필수 소프트웨어

다음 항목이 로컬에 설치되어 있어야 합니다.

* [로컬 AEM **작성자** 인스턴스](https://experience.adobe.com/#/downloads)&#x200B;(Cloud Service SDK 또는 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) (LTS - 장기 지원)
* [npm 6 이상](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 또는 이에 상응하는 IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - 튜토리얼 전반에 사용되는 도구

>[!NOTE]
>
> **AEM as a Cloud Service를 처음 사용하십니까?** [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)를 확인하십시오.
>
> **AEM 6.5를 처음 사용하십니까?** [로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko)를 확인하십시오.

## GitHub {#github}

이 튜토리얼의 코드는 GitHub의 AEM 안내서에서 찾을 수 있습니다.

**[GitHub: WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)**

또한 튜토리얼의 각 부분은 GitHub에 자체 분기가 있습니다. 사용자는 이전 부분에 해당하는 분기를 체크아웃하여 언제든지 튜토리얼을 시작할 수 있습니다.

## 다음 단계 {#next-steps}

찾고 있는 항목이 무엇입니까? 튜토리얼을 시작하려면 [프로젝트 설정](project-setup.md) 챕터로 이동하고 AEM Project Archetype을 사용하여 새로운 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아보시기 바랍니다.
