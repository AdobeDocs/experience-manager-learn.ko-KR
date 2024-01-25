---
title: AEM UI 확장성
description: App Builder를 사용하여 확장을 만드는 AEM UI 확장성에 대해 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 66
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# AEM UI 확장성

Adobe Experience Manager(AEM)은 디지털 경험을 만들기 위한 강력한 UI(사용자 인터페이스)를 제공합니다. UI를 사용자 정의하고 확장하기 위해 Adobe에서 App Builder를 도입했습니다. 이 도구를 사용하면 개발자는 JavaScript 및 React를 사용하여 복잡한 코딩 없이 사용자 경험을 향상시킬 수 있습니다.

App Builder는 AEM에서 확장 지점을 잘 정의하도록 바인딩된 확장을 만들 수 있는 구현 계층을 제공합니다. App Builder는 AEM과 원활하게 통합되어 실시간 미리보기 및 테스트를 허용합니다. AEM에 변경 사항을 빠르고 간편하게 배포할 수 있습니다. 개발자는 App Builder를 사용하여 시간과 노력을 절약하여 신속한 프로토타이핑과 관련자와의 협업을 가능하게 합니다.

## AEM UI 확장 개발

AEM 다양한 UI에는 서로 다른 확장 지점이 있지만 기본 개념은 동일합니다.

아래에 연결된 비디오 및 연습에서는 다양한 활동을 보여주기 위해 콘텐츠 조각 콘솔 확장 의 사용을 보여 줍니다. 그러나 위에서 설명한 개념은 모든 AEM UI 확장에 적용할 수 있습니다.

1. [Adobe Developer 콘솔 프로젝트 만들기](./adobe-developer-console-project.md)
1. [확장 초기화](./app-initialization.md)
1. [확장 등록](./extension-registration.md)
1. 확장 지점 구현

   확장 지점 및 해당 구현은 확장 중인 UI에 따라 달라집니다.

   + [콘텐츠 조각 UI 확장 개발](./content-fragments/overview.md)

1. [양식 개발](./modal.md)
1. [Adobe I/O Runtime 작업 개발](./runtime-action.md)
1. [확장 확인](./verify.md)
1. [확장 배포](./deploy.md)

## Adobe Developer 설명서

Adobe Developer에는 AEM UI 확장성에 대한 개발자 세부 사항이 포함되어 있습니다. 다음을 검토하십시오. [자세한 기술 정보는 Adobe Developer 콘텐츠 를 참조하십시오.](https://developer.adobe.com/uix/docs/).
