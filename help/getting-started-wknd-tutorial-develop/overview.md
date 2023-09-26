---
title: AEM Sites 시작 - WKND 튜토리얼
description: WKND라는 가상의 라이프스타일 브랜드를 위한 AEM 사이트를 구현하는 방법에 대해 알아봅니다. 프로젝트 설정, Maven 원형, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발 등의 기본 Experience Manager 주제에 대한 설명을 살펴보십시오.
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---

# AEM Sites 시작 - WKND 튜토리얼 {#introduction}

{{edge-delivery-services}}

Adobe Experience Manager(AEM)을 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼에 오신 것을 환영합니다. 이 튜토리얼에서는 가상 라이프스타일 브랜드인 WKND를 위한 AEM 사이트의 구현 과정을 안내합니다. 이 튜토리얼은 Adobe Experience Manager Sites를 통한 사용한 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 구성 요소 개발 등의 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 멀티 파트 튜토리얼의 목표는 개발자에게 Adobe Experience Manager(AEM)의 최신 표준 및 기술을 사용하여 웹 사이트를 구현하는 방법을 가르치는 것입니다. 이 자습서를 완료한 후 개발자는 플랫폼의 기본 기반과 AEM의 일반적인 디자인 패턴을 이해해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 사이트 프로젝트 시작 옵션

AEM Sites 프로젝트를 시작하는 데에는 두 가지 기본적인 접근 방식이 있습니다.

**AEM Project Archetype** - Maven 템플릿을 사용하여 최소한의 AEM 프로젝트를 생성하여 AEM 개발에 대한 기존의 접근 방식 대량 맞춤화를 예상하는 AEM 6.5/6.4 프로젝트 및 AEM as a Cloud Service 프로젝트에 대해 권장되는 접근 방식입니다. 이 튜토리얼에서는 AEM 개발에 대해 자세히 살펴볼 수 있습니다.

[AEM Project Archetype으로 자습서 시작](./project-archetype/overview.md)

**AEM 사이트 템플릿** - 사전 정의된 사이트 템플릿을 사용하여 AEM 사이트를 생성하는 로우 코드 방식인 빠른 사이트 생성 이라고도 합니다. 즉시 사용 가능한 구성 요소 및 템플릿을 사용하여 사이트를 빠르게 시작하고 실행할 수 있습니다. 테마 설정 워크플로를 사용하여 CSS와 JavaScript만으로 브랜드별 스타일 및 맞춤화를 적용할 수 있습니다. 새 프로젝트 및 개발자에게 권장됩니다. AEMas a Cloud Service 에서만 사용할 수 있습니다.

[사이트 템플릿을 사용하여 튜토리얼 시작](./site-template/create-site.md)

## Adobe XD UI 키트

이 튜토리얼을 실제 시나리오에 가깝게 만들려면 Adobe의 재능 있는 UX 디자이너가 를 사용하여 사이트에 대한 mockup을 만들었습니다. [Adobe XD](https://www.adobe.com/products/xd.html). 튜토리얼의 과정에서 다양한 디자인 조각이 완전히 작성 가능한 AEM 사이트로 구현됩니다. 특별한 감사 인사 **로렌초 부오시** 및 **킬리안 아멘돌라** WKND 부지를 위한 아름다운 디자인을 만든 사람.

XD UI 키트 다운로드:

* [AEM 핵심 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 참조 사이트 {#reference-site}

WKND Site의 완성된 버전을 참조로도 사용할 수 있습니다. [https://wknd.site/](https://wknd.site/)

이 자습서에서는 AEM 개발자에게 필요한 주요 개발 기술을 다룹니다. *아님* 전체 사이트를 처음부터 끝까지 빌드합니다. 완성된 참조 사이트는 AEM의 더 많은 기능을 즉시 탐색하고 볼 수 있는 또 다른 훌륭한 리소스입니다.

자습서에 들어가기 전에 최신 코드를 테스트하려면 를 다운로드하여 설치하십시오. **[gitHub의 최신 릴리스](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Adobe Stock 제공

WKND 참조 웹 사이트의 이미지 중 대부분은 [Adobe Stock](https://stock.adobe.com/) 및 은 데모 에셋 추가 약관에 정의된 서드파티 자료입니다. [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Adobe Stock 이미지를 웹 사이트 또는 마케팅 자료에서 제공하는 것과 같이, 이 데모 웹 사이트를 보는 것 이상의 다른 용도로 사용하려면, Adobe Stock에서 라이선스를 구입할 수 있습니다.

Adobe Stock을 사용하면 사진, 그래픽, 비디오 및 템플릿을 포함하여 1억 4천만 개 이상의 고품질 로열티가 없는 이미지를 활용하여 크리에이티브 프로젝트를 바로 시작할 수 있습니다.

## 다음 단계 {#next-steps}

무엇을 기다리고 있습니까?! 방법 알아보기 [AEM Project Archetype을 사용하여 새 Adobe Experience Manager 프로젝트 생성](./project-archetype/overview.md) 또는 [사이트 템플릿을 사용하여 사이트 생성](./site-template/create-site.md).
