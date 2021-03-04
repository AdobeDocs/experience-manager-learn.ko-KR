---
title: AEM Sites 시작하기 - WKND 튜토리얼
description: AEM Sites 시작하기 - WKND 튜토리얼. WKND 자습서는 Adobe Experience Manager을 처음 사용하는 개발자를 위해 고안된 다양한 자습서입니다. 이 자습서에서는 가상 라이프스타일 브랜드 WKND를 위한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 전문가 원형, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발과 같은 기본 주제를 다룹니다.
sub-product: 사이트
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 7%

---


# AEM Sites 시작하기 - WKND 튜토리얼 {#introduction}

AEM(Adobe Experience Manager)을 처음 사용하는 개발자를 위해 고안된 여러 부분으로 구성된 자습서입니다. 이 자습서에서는 WKND라는 가상의 라이프스타일 브랜드에 대한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 Adobe Experience Manager Sites와의 구성 요소 개발과 같은 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 여러 부분으로 구성된 튜토리얼의 목표는 AEM(Adobe Experience Manager)의 최신 표준과 기술을 사용하여 웹 사이트를 구현하는 방법을 개발자에게 교육하는 것입니다. 이 자습서를 완료한 후 개발자는 AEM의 일반적인 디자인 패턴에 대한 지식과 함께 플랫폼의 기본 기반을 이해해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

이 자습서는 **AEM을 Cloud Service**&#x200B;로 사용하도록 설계되었으며 **AEM 6.5.5.0+** 및 **AEM 6.4.8.1+**&#x200B;와 역호환됩니다. 사이트는 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)
* [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 모델
* [편집 가능한 템플릿](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [스타일 시스템](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*자습서의 각 부분을 통과하는 데 1-2시간을 예상합니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)가 IDE로 Mac OS 환경에서 실행되는 Cloud Service SDK로 AEM을 사용하여 캡처됩니다. 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적입니다.

### 필수 소프트웨어

다음은 로컬에 설치해야 합니다.

* 로컬 AEM **작성자** 인스턴스(Cloud Service SDK, 6.5.5+ 또는 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js](https://nodejs.org/en/) (LTS - 장기 지원)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) 코드 또는 동급 IDE
   * [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - 튜토리얼 전체에서 사용되는 도구

>[!NOTE]
>
> **Cloud Service으로 AEM을 처음 사용하시나요?** AEM을 Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하는 방법에 대한  [다음 가이드를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하시나요?** 로컬 개발 환경을 설정하는 방법은  [다음 안내서를 참조하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## 자습서 {#about-tutorial} 정보

WKND는 여러 국제 도시에서 밤 문화, 활동, 그리고 이벤트들에 초점을 맞춘 가상의 온라인 잡지 및 블로그입니다.

### Adobe XD UI 키트

이 튜토리얼을 실제 시나리오의 실제 시나리오에 더 가깝게 만들기 위해 Adobe의 재능 있는 UX 디자이너는 [Adobe XD](https://www.adobe.com/products/xd.html)을 사용하여 해당 사이트의 초안을 만들었습니다. 자습서 과정 동안 다양한 디자인 조각이 저작이 가능한 AEM 사이트에 구현됩니다. **로렌조 부오시** 및 WKND 사이트에 대한 아름다운 디자인을 만든 **킬리안 아멘돌라**&#x200B;에 감사드립니다.

XD UI 키트를 다운로드합니다.

* [AEM 코어 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

개발자가 ***weekend***&#x200B;에서 자습서를 완료하기 위해 더 나은 부분을 차지할 것으로 예상하기 때문에 WKND라는 이름이 적합합니다.

### Github {#github}

프로젝트의 모든 코드는 AEM Guide 보고서의 Github에서 찾을 수 있습니다.

**[GitHub:WKND 사이트 프로젝트](https://github.com/adobe/aem-guides-wknd)**

또한 튜토리얼의 각 부분에는 GitHub에 자체 분기가 있습니다. 사용자는 이전 부품에 해당하는 분기를 체크 아웃하면 언제든지 튜토리얼을 시작할 수 있습니다.

>[!NOTE]
>
> 이 자습서의 이전 버전을 사용하여 작업하고 있는 경우에도 GitHub에서 [솔루션 패키지](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) 및 [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)를 찾을 수 있습니다.

## 참조 사이트 {#reference-site}

WKND 사이트의 완성된 버전을 참조로도 사용할 수 있습니다.[https://wknd.site/](https://wknd.site/)

이 자습서에서는 AEM 개발자에게 필요한 주요 개발 기술을 설명하지만 전체 사이트 전체를 완벽한 사이트를 구축하지는 *않습니다.* 완성된 참조 사이트는 즉시 사용 가능한 AEM 기능을 확인하고 확인할 수 있는 또 다른 유용한 리소스입니다.

자습서로 이동하기 전에 최신 코드를 테스트하려면 GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**에서**[&#x200B;최신 릴리스를 다운로드하여 설치하십시오.

### Adobe Stock 제공

WKND 참조 웹 사이트의 많은 이미지는 [Adobe Stock](https://stock.adobe.com/)에 있으며 데모 에셋 추가 약관([https://www.adobe.com/legal/terms.html](https://www.adobe.com/kr/legal/terms.html)에 정의된 바와 같이 제3자 자료입니다. 웹 사이트 또는 마케팅 자료에 게시하는 등 이 데모 웹 사이트를 볼 수 없는 다른 목적으로 Adobe Stock 이미지를 사용하려면 Adobe Stock에서 라이센스를 구입할 수 있습니다.

Adobe Stock을 사용하면 사진, 그래픽, 비디오, 템플릿 등 1억 4천만 개 이상의 고품질의 로열티 프리 이미지를 이용하여 크리에이티브 프로젝트를 바로 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리고 있어?![프로젝트 설정](project-setup.md) 장으로 이동하여 자습서를 시작하고 AEM 프로젝트 원형형을 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아봅니다.
