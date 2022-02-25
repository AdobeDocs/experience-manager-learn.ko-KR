---
title: AEM Sites 시작하기 - Project Archetype
description: AEM Sites 시작하기 - Project Archetype WKND 자습서는 Adobe Experience Manager을 처음 사용하는 개발자를 위해 디자인된 여러 부분으로 된 자습서입니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대해 AEM 사이트를 구현하는 방법을 설명합니다. 이 자습서에서는 프로젝트 설정, maven 원형, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발과 같은 기본 주제를 다룹니다.
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 8%

---

# AEM Sites 시작하기 - Project Archetype {#project-archetype}

AEM(Adobe Experience Manager)을 처음 사용하는 개발자를 위해 고안된 다양한 자습서를 시작합니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대한 AEM 사이트 구현을 안내합니다.

이 튜토리얼은 [AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 새 프로젝트를 생성합니다.

이 튜토리얼은 **AEM as a Cloud Service** 및 는 이전 버전과 호환됩니다. **AEM 6.5.10+**. 사이트는 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 모델
* [편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*자습서의 각 부분을 통과하는 데 1~2시간을 예상합니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/) IDE로 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적이어야 합니다.

### 필수 소프트웨어

로컬에 설치해야 합니다.

* [로컬 AEM **작성자** 인스턴스](https://experience.adobe.com/#/downloads) (Cloud Service SDK, 6.5.10+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) (LTS - 장기 지원)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio 코드](https://code.visualstudio.com/) 또는 이에 상응하는 IDE
   * [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - 자습서 전체에서 사용되는 도구

>[!NOTE]
>
> **AEM as a Cloud Service을 처음 사용하십니까?** 다음을 확인하십시오 [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 데 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하십니까?** 다음을 확인하십시오 [로컬 개발 환경 설정에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Github {#github}

프로젝트의 모든 코드는 AEM 안내서 보고서의 Github에 있습니다.

**[GitHub: WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)**

또한 자습서의 각 부분에는 GitHub에 고유한 분기가 있습니다. 사용자는 이전 부품에 해당하는 분기를 체크 아웃하면 언제든지 자습서를 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리고 있는 거야?! 로 이동하여 자습서를 시작합니다 [프로젝트 설정](project-setup.md) AEM 프로젝트 원형 을 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 장 및 알아봅니다.
