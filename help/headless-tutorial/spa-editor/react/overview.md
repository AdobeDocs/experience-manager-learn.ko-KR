---
title: AEM SPA Editor 및 React 시작하기
description: Adobe Experience Manager(AEM)에서 WKND SPA를 사용하여 편집할 수 있는 첫 번째 React Single Page Application(SPA)을 제작해 보십시오. AEM의 SPA 편집기와 React JS 프레임워크를 사용하여 SPA를 제작하는 방법에 대해 알아봅니다. 여러 부문으로 구성된 이 튜토리얼은 WKND의 가상 라이프스타일 브랜드를 위한 React 애플리케이션의 구현 과정을 안내합니다. 이 튜토리얼은 전체적인 SPA 개발 및 AEM과의 통합에 대한 정보를 담고 있습니다.
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 20%

---

# AEM에서 첫 번째 React SPA 만들기 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager(AEM)의 **SPA 편집기** 기능을 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼에 오신 것을 환영합니다. 이 튜토리얼에서는 가상 라이프스타일 브랜드인 WKND에 대한 React 애플리케이션의 구현을 안내합니다. React 앱은 React 구성 요소를 AEM 구성 요소에 매핑하는 AEM의 SPA Editor와 함께 배포되도록 개발 및 설계되었습니다. AEM에 배포된 완성된 SPA는 AEM의 기존 인라인 편집 도구를 사용하여 동적으로 작성할 수 있습니다.

![최종 SPA 구현](assets/wknd-spa-implementation.png)

*WKND SPA 구현*

## 정보

이 자습서는 **AEM as a Cloud Service**&#x200B;에서 작동하도록 설계되었으며 **AEM 6.5.4+** 및 **AEM 6.4.8+**&#x200B;과(와) 역으로 호환됩니다.

## 최신 코드

모든 튜토리얼 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-spa)에서 찾을 수 있습니다.

[최신 코드 베이스](https://github.com/adobe/aem-guides-wknd-spa/releases)는 다운로드 가능한 AEM 패키지로 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 시작하려면 먼저 다음 항목이 필요합니다.

* HTML, CSS 및 JavaScript에 대한 기본 지식
* [React](https://reactjs.org/tutorial/tutorial.html)에 대한 기본 친숙도

*필요하지 않지만 [기존 AEM Sites 구성 요소 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR)에 대한 기본적인 이해를 하는 것이 좋습니다.*

## 로컬 개발 환경 {#local-dev-environment}

이 자습서를 완료하려면 로컬 개발 환경이 필요합니다. 스크린샷과 비디오는 [Visual Studio 코드](https://code.visualstudio.com/)이(가) IDE로 있는 Mac OS 환경에서 실행되는 AEM as a Cloud Service SDK을 사용하여 캡처됩니다. 명령과 코드는 별도로 명시하지 않는 한 로컬 운영 체제와 독립적이어야 합니다.

### 필수 소프트웨어

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 또는 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)&#x200B;(3.3.9 이상)
* [Node.js](https://nodejs.org/en/) 및 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> AEM as a Cloud Service을 처음 사용하십니까?**&#x200B;** AEM as a Cloud Service SDK을 사용하여 로컬 개발 환경을 설정하는 방법에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko).
>
> **AEM 6.5를 처음 사용하십니까?** 로컬 개발 환경 설정에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ko).

## 다음 단계 {#next-steps}

무엇을 기다리고 있습니까?! [프로젝트 만들기](create-project.md) 장으로 이동하여 자습서를 시작하고 AEM Project Archetype을 사용하여 SPA 편집기 사용 프로젝트를 생성하는 방법을 알아봅니다.
