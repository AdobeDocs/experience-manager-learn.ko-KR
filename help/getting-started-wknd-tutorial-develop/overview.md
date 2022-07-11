---
title: AEM Sites 시작하기 - WKND 자습서
description: WKND라는 가상 라이프스타일 브랜드에 AEM 사이트를 구현하는 방법을 알아봅니다. 프로젝트 설정, maven 원형, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발과 같은 기본 Experience Manager 주제에 대한 연습을 수행할 수 있습니다.
sub-product: sites
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 72abe1cddcf6a012403887203d38509bde8f2d23
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 3%

---

# AEM Sites 시작하기 - WKND 자습서 {#introduction}

AEM(Adobe Experience Manager)을 처음 사용하는 개발자를 위해 고안된 다양한 자습서를 시작합니다. 이 자습서에서는 가상 라이프스타일 브랜드인 WKND에 대해 AEM 사이트를 구현하는 방법을 설명합니다. 이 자습서에서는 프로젝트 설정, 코어 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 Adobe Experience Manager Sites을 사용한 구성 요소 개발과 같은 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 다중 부분 자습서의 목표는 AEM(Adobe Experience Manager)의 최신 표준 및 기술을 사용하여 웹 사이트를 구현하는 방법을 개발자에게 가르치는 것입니다. 이 자습서를 완료하고 개발자는 플랫폼의 기본 기반과 AEM의 일반적인 디자인 패턴을 이해해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Sites 프로젝트를 시작하는 옵션

AEM Sites 프로젝트를 시작하는 데에는 두 가지 기본 접근 방법이 있습니다.

**AEM 프로젝트 원형** - Maven 템플릿을 사용하여 최소한의 AEM 프로젝트를 생성하여 AEM 개발에 대한 기존 접근 방식. 이는 과도한 사용자 지정을 예상하는 AEM 6.5/6.4 프로젝트 및 AEM as a Cloud Service 프로젝트에 대한 권장 접근 방식입니다. 이 자습서에서는 AEM 개발을 자세히 살펴봅니다.

[AEM 프로젝트 원형 을 사용하여 자습서를 시작합니다](./project-archetype/overview.md)

**AEM 사이트 템플릿** - 사전 정의된 사이트 템플릿을 사용하여 AEM 사이트를 생성하는 낮은 코드 접근 방법인 빠른 사이트 작성이라고도 합니다. 즉시 사용 가능한 구성 요소 및 템플릿을 사용하여 사이트를 신속하게 설치 및 실행할 수 있습니다. CSS 및 JavaScript만 사용하여 브랜드별 스타일 및 사용자 지정을 적용하려면 테마 워크플로우를 사용하십시오. 새 프로젝트 및 개발자에게 권장됩니다. AEM as a Cloud Service에만 사용할 수 있습니다.

[사이트 템플릿을 사용하여 자습서 시작](./site-template/create-site.md)

## Adobe XD UI 키트

이 자습서를 실제 시나리오 Adobe의 재능 있는 UX 디자이너에 더 가까이 만들기 위해 를 사용하여 사이트에 대한 모집단 생성을 만들었습니다 [Adobe XD](https://www.adobe.com/products/xd.html). 자습서의 교육 과정 동안 다양한 디자인의 조각이 완전한 작성 가능한 AEM 사이트에 구현됩니다. 특별한 감사 **로렌조 부오시** 및 **킬리안 아멘돌라** WKND 사이트를 위한 아름다운 디자인을 창조한 사람입니다.

XD UI 키트를 다운로드합니다.

* [AEM 코어 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 참조 사이트 {#reference-site}

WKND Site의 완성된 버전을 참조로도 사용할 수 있습니다. [https://wknd.site/](https://wknd.site/)

이 자습서에서는 AEM 개발자에게 필요한 주요 개발 기술을 설명하지만, *not* 전체 사이트를 처음부터 끝까지 구축 완료된 참조 사이트는 즉시 사용 가능한 AEM 기능을 탐색하고 확인할 수 있는 또 다른 유용한 리소스입니다.

자습서에 이동하기 전에 최신 코드를 테스트하려면 을(를) 다운로드하여 설치합니다 **[GitHub의 최신 릴리스](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

WKND 참조 웹 사이트의 많은 이미지는 [Adobe Stock](https://stock.adobe.com/) 및 은 의 데모 자산 추가 용어에 정의된 타사 자료입니다. [https://www.adobe.com/legal/terms.html](https://www.adobe.com/kr/legal/terms.html). Adobe Stock 이미지를 웹 사이트나 마케팅 자료에서 제공하는 것과 같이 이 데모 웹 사이트를 보는 것 이상으로 사용하려면 Adobe Stock에서 라이센스를 구입할 수 있습니다.

Adobe Stock을 사용하면 사진, 그래픽, 비디오 및 템플릿을 포함한 1억 4,000만 개 이상의 고품질의 로열티가 없는 이미지에 액세스하여 크리에이티브 프로젝트를 신속하게 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리고 있는 거야?! 방법 알아보기 [AEM 프로젝트 원형 을 사용하여 새 Adobe Experience Manager 프로젝트 생성](./project-archetype/overview.md) 또는 [사이트 템플릿을 사용하여 사이트 만들기](./site-template/create-site.md).
