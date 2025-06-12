---
title: AEM UI 확장성
description: App Builder를 사용하여 확장 기능을 만드는 AEM UI 확장성에 대해 알아봅니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '275'
ht-degree: 100%

---

# AEM UI 확장성 {#aem-ui-extensibility}

Adobe Experience Manager(AEM)는 디지털 경험을 만드는 데 필요한 강력한 사용자 인터페이스(UI)를 제공합니다. UI를 사용자 정의하고 확장하기 위해 Adobe는 App Builder를 출시했습니다. 이 도구를 사용하면 개발자는 JavaScript와 React를 사용하여 복잡한 코딩 없이도 사용자 경험을 개선할 수 있습니다.

App Builder는 AEM에서 명확하게 정의된 확장 지점에 바인딩되는 확장 기능을 만들기 위한 구현 레이어를 제공합니다. App Builder는 AEM과 완벽하게 통합되어 실시간 미리 보기와 테스트가 가능합니다. AEM에 대한 변경 사항을 빠르고 간단하게 배포할 수 있습니다. App Builder를 사용하면 개발자는 빠르게 프로토타입을 제작하고 이해 당사자와의 협업을 용이하게 수행하여 시간과 노력을 절감할 수 있습니다.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Adobe Developer App Builder 및 AEM Headless 시작하기"
>abstract="AEM App Builder를 통해 개발자가 JavaScript와 React를 사용하여 AEM UI를 빠르게 사용자 정의하고 확장하여 원활한 통합과 신속한 배포를 지원하는 방법에 대해 알아봅니다."

## AEM UI 확장 기능 개발

AEM에는 여러 UI 확장 지점이 존재하지만 기본적인 개념은 모든 UI 확장에 동일하게 적용됩니다.

아래 링크된 비디오와 워크스루에서는 콘텐츠 조각 콘솔 확장 기능을 사용하여 다양한 활동을 설명하는 방법을 보여 줍니다. 여기서 다루는 개념은 모든 AEM UI 확장 기능에 적용될 수 있습니다.

1. [Adobe Developer Console 프로젝트 만들기](./adobe-developer-console-project.md)
1. [확장 기능 초기화](./app-initialization.md)
1. [확장 기능 등록](./extension-registration.md)
1. 확장 지점 구현

   확장 지점과 구현은 확장하는 UI에 따라 다릅니다.

   + [콘텐츠 조각 UI 확장 기능 개발](./content-fragments/overview.md)

1. [모달 개발](./modal.md)
1. [Adobe I/O Runtime Action 개발](./runtime-action.md)
1. [확장 기능 확인](./verify.md)
1. [확장 기능 배포](./deploy.md)

## Adobe Developer 설명서

Adobe Developer에는 AEM UI 확장성에 대한 개발자 세부 정보가 포함되어 있습니다. [자세한 기술 정보는 Adobe Developer 콘텐츠를 검토](https://developer.adobe.com/uix/docs/)하십시오.
