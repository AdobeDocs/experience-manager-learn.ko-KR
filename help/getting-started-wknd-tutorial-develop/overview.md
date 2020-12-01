---
title: AEM Sites 시작하기 - WKND 자습서
description: AEM Sites 시작하기 - WKND 자습서. WKND 튜토리얼은 Adobe Experience Manager을 처음 접하는 개발자를 위해 마련된 다양한 자습서입니다. 이 자습서는 가상의 라이프스타일 브랜드인 WKND에 대한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 마비판, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발과 같은 기본 주제를 다룹니다.
sub-product: sites
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
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 6%

---


# AEM Sites 시작하기 - WKND 자습서 {#introduction}

Adobe Experience Manager(AEM)을 처음 사용하는 개발자를 위해 고안된 다양한 기능의 자습서입니다. 이 자습서는 가상 라이프스타일 브랜드 WKND에 대한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리, Adobe Experience Manager Sites을 사용한 구성 요소 개발 등의 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 멀티 파트 자습서의 목표는 개발자에게 Adobe Experience Manager(AEM)의 최신 표준과 기술을 사용하여 웹 사이트를 구현하는 방법을 교육하는 것입니다. 이 자습서를 완료한 후 개발자는 플랫폼의 기본 기반과 AEM의 일반적인 디자인 패턴에 대해 알아야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

이 자습서는 **AEM에서 Cloud Service**&#x200B;로 작동하도록 설계되었으며 **AEM 6.5+** 및 **AEM 6.4.2+**&#x200B;와 역호환됩니다. 사이트는 다음을 사용하여 구현됩니다.

* [Maven AEM 프로젝트 원형](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)
* [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Models
* [편집 가능한 템플릿](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [스타일 시스템](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*자습서의 각 부분을 통과하는 데 1-2시간을 예상합니다.*

## 자습서 {#about-tutorial} 정보

WKND는 가상의 온라인 잡지 및 블로그로서 몇몇 국제 도시들의 밤 문화, 활동, 그리고 이벤트를 다룹니다.

### Adobe XD UI 키트

이 튜토리얼을 실제 시나리오와 가까이 맞추기 위해 Adobe의 재능 있는 UX 디자이너는 [Adobe XD](https://www.adobe.com/products/xd.html)을 사용하여 사이트에 대한 초안을 만들었습니다. 자습서 과정 동안 다양한 디자인 조각이 완성한 AEM 사이트에 구현됩니다. WKND 사이트의 아름다운 디자인을 만든 **로렌조 부오시** 및 **킬리안 아멘돌라**&#x200B;에 대한 특별 감사 드립니다.

XD UI 키트를 다운로드합니다.

* [AEM 코어 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

개발자가 ***weekend***&#x200B;에서 자습서를 완료하기 위해 더 나은 부분을 차지할 것으로 예상되기 때문에 WKND라는 이름이 적합합니다.

### Github {#github}

프로젝트의 모든 코드는 AEM Guide 보고서의 Github에서 찾을 수 있습니다.

**[GitHub:WKND Sites Project](https://github.com/adobe/aem-guides-wknd)**

또한 튜토리얼의 각 부분에는 GitHub에 자체 분기가 있습니다. 사용자는 이전 부품에 해당하는 분기를 체크 아웃하면 언제든지 튜토리얼을 시작할 수 있습니다.

>[!NOTE]
>
> 이전 버전의 이 튜토리얼을 사용하더라도 GitHub에서 [솔루션 패키지](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) 및 [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)을 계속 찾을 수 있습니다.

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. Mac OS 환경에서 실행되는 Cloud Service SDK로 AEM을 사용하여 스크린샷 및 비디오를 캡처합니다. 별도의 언급이 없는 한 명령과 코드는 로컬 운영 체제와 독립되어야 합니다.

**AEM을 Cloud Service으로 처음 사용하는 경우** AEM을 Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하는 방법은  [다음 가이드를 참조하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**AEM 6.5를 처음 사용하시나요?** 로컬 개발 환경을 설정하려면  [다음 가이드를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

### 필수 소프트웨어

다음은 로컬에 설치해야 합니다.

* [AEM의 Cloud Service ](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) SDK 또는  [AEM 6.5](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/technical-requirements.html) 나  [AEM 6.4 + SP2](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) (AEM 6.5+만 해당)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### IDE(Integrated Development Environment)

이 자습서에서는 [Eclipse](https://www.eclipse.org/)와 [AEM Developer Tool 플러그인](https://eclipse.adobe.com/aem/dev-tools/)을(를) IDE로 사용하지만 Java 및 Maven 프로젝트를 지원하는 모든 IDE를 사용할 수 있습니다. 이 튜토리얼에서는 특정 IDE 기능에 대한 의존이 최소화됩니다.

Eclipse 또는 [Visual Studio 코드](https://code.visualstudio.com/) 또는 [IntelliJ](https://www.jetbrains.com/idea/)와 같은 기타 IDE를 사용하기 위한 자세한 단계를 보려면 [다음 안내서](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)를 확인하십시오.

## 참조 사이트 {#reference-site}

WKND 사이트의 완성된 버전을 참조로도 사용할 수 있습니다.[https://wknd.site/](https://wknd.site/)

이 자습서에서는 AEM 개발자에게 필요한 주요 개발 기술을 다루지만 전체 사이트를 종단간 구축할 수는 *없습니다.* 완성된 참조 사이트는 즉시 사용 가능한 다양한 AEM 기능을 살펴보고 확인할 수 있는 또 다른 유용한 리소스입니다.

자습서로 이동하기 전에 최신 코드를 테스트하려면 GitHub **[](https://github.com/adobe/aem-guides-wknd/releases/latest)의 최신 릴리스를 다운로드하여 설치하십시오.**

### Powered by Adobe Stock

WKND 참조 웹 사이트의 많은 이미지는 [Adobe Stock](https://stock.adobe.com/)에 있으며 데모 에셋 추가 약관([https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html)에 정의된 바와 같이 제3자 자료입니다. 웹 사이트 또는 마케팅 자료에 게시하는 등 이 데모 웹 사이트를 보기 위한 이외의 다른 목적으로 Adobe Stock 이미지를 사용하려면 Adobe Stock에서 라이선스를 구입할 수 있습니다.

Adobe Stock을 사용하면 사진, 그래픽, 비디오, 템플릿 등 1억 4천만 개 이상의 고품질의 로열티 프리 이미지를 이용하여 크리에이티브 프로젝트를 신속하게 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리는 거야?[프로젝트 설정](project-setup.md) 장으로 이동하여 자습서를 시작하고 AEM 프로젝트 원형형을 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아봅니다.
