---
title: AEM Sites 시작하기 - Project Archetype
description: AEM Sites 시작하기 - 프로젝트 원형. WKND 튜토리얼은 Adobe Experience Manager을 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼입니다. 이 튜토리얼은 가상의 라이프스타일 브랜드인 WKND를 위한 AEM 사이트의 구현 과정을 안내합니다. 이 튜토리얼에서는 프로젝트 설정, Maven 원형, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발 등의 기본 주제를 다룹니다.
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
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 35%

---

# AEM Sites 시작하기 - Project Archetype {#project-archetype}

{{traditional-aem}}

Adobe Experience Manager(AEM)를 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼을 시작합니다. 이 튜토리얼에서는 WKND의 가상 라이프스타일 브랜드를 위한 AEM 사이트의 구현 과정을 안내합니다.

이 자습서는 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)을(를) 사용하여 새 프로젝트를 생성하여 시작합니다.

이 자습서는 **AEM as a Cloud Service**&#x200B;에서 작동하도록 설계되었으며 **AEM 6.5.14+**&#x200B;과(와) 하위 호환됩니다. 다음을 사용하여 사이트를 구현합니다.

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)
* [코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=ko)
* [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)
* [편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=ko)
* [스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=ko)

*튜토리얼의 각 부분을 완료하는 데는 약 1~2시간이 소요됩니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 튜토리얼을 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷과 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)이(가) IDE로 있는 macOS 환경에서 실행되는 AEM as a Cloud Service SDK을 사용하여 캡처됩니다. 달리 명시되지 않는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

### 필수 소프트웨어

다음은 로컬에 설치해야 합니다.

* [로컬 AEM **작성자** 인스턴스](https://experience.adobe.com/#/downloads)&#x200B;(Cloud Service SDK 또는 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)&#x200B;(3.3.9 이상)
* [Node.js](https://nodejs.org/en/)&#x200B;(LTS - 장기 지원)
* [npm 6 이상](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 또는 이와 동등한 IDE
   * [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - 자습서 전체에서 사용되는 도구

>[!NOTE]
>
> **AEM as a Cloud Service를 처음 사용하십니까?** [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)를 확인하십시오.
>
> **AEM 6.5를 처음 사용하십니까?** [로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko)를 확인하십시오.

## GitHub {#github}

이 자습서의 코드는 AEM 안내서 저장소의 GitHub에 있습니다.

**[GitHub: WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)**

또한 자습서의 각 부분에는 GitHub에 고유한 분기가 있습니다. 사용자는 이전 부분에 해당하는 분기를 체크아웃하여 언제든지 자습서를 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리는 거야? [프로젝트 설정](project-setup.md) 장으로 이동하여 자습서를 시작하고 AEM Project Archetype을 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아봅니다.
