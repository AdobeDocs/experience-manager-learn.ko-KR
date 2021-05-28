---
title: AEM SPA Editor 및 React 시작하기
description: WKND SPA을 사용하여 Adobe Experience Manager AEM에서 편집할 수 있는 첫 번째 React Single Page Application(SPA)을 만듭니다. AEM SPA Editor에서 React JS 프레임워크를 사용하여 SPA을 만드는 방법을 알아봅니다. 이 여러 부분으로 구성된 자습서에서는 가상 라이프스타일 브랜드인 WKND에 React 애플리케이션을 구현하는 과정을 설명합니다. 이 자습서에서는 SPA 만들기 종단간 및 AEM과의 통합을 다룹니다.
sub-product: 사이트
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA 편집기
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 4%

---


# AEM에서 첫 번째 React SPA 만들기 {#overview}

Adobe Experience Manager(AEM)의 **SPA Editor** 기능을 처음 사용하는 개발자를 위해 고안된 여러 부분으로 된 자습서를 시작합니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대한 React 애플리케이션의 구현을 안내합니다. React 앱은 개발 및 배포하도록 설계되며, React 구성 요소를 AEM 구성 요소에 매핑합니다. AEM에 배포된 완료된 SPA은 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![구현된 최종 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 자습서는 **AEM as a1/>Cloud Service으로 작동하도록 설계되었으며** AEM 6.5.4+**및** AEM 6.4.8+**와 이전 버전과 호환됩니다.**

## 최신 코드

모든 자습서 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 전제 조건

이 자습서를 시작하기 전에 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식입니다
* [React](https://reactjs.org/tutorial/tutorial.html)에 대한 기본 친숙함

*필수는 아니지만 기존  [AEM Sites 구성 요소 개발에 대한 기본 이해를 갖는 것이 좋습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷 및 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)를 IDE로 사용하여 Mac OS 환경에서 실행되는 Cloud Service SDK로 AEM을 사용하여 캡처됩니다. 별도의 설명이 없는 한 명령과 코드는 로컬 운영 체제와 독립적이어야 합니다.

### 필수 소프트웨어

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html),  [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65)  또는  [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 이상)
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **AEM as a Cloud Service을 처음 사용하십니까?** AEM as a  [Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하려면 다음 안내서를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **AEM 6.5를 처음 사용하십니까?** 로컬 개발 환경을  [설정하려면 다음 안내서를 확인하십시오](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## 다음 단계 {#next-steps}

뭘 기다리고 있는 거야?![프로젝트 만들기](create-project.md) 장으로 이동하여 자습서를 시작하고 AEM Project Archetype을 사용하여 SPA 편집기 지원 프로젝트를 생성하는 방법을 알아봅니다.
