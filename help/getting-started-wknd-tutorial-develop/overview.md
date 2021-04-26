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
feature: 핵심 구성 요소, 페이지 편집기, 편집 가능한 템플릿, AEM 프로젝트 원형
topic: 컨텐츠 관리, 개발
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 4%

---


# AEM Sites 시작하기 - WKND 튜토리얼 {#introduction}

AEM(Adobe Experience Manager)을 처음 사용하는 개발자를 위해 고안된 여러 부분으로 구성된 자습서입니다. 이 자습서에서는 WKND라는 가상의 라이프스타일 브랜드에 대한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 Adobe Experience Manager Sites와의 구성 요소 개발과 같은 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 여러 부분으로 구성된 튜토리얼의 목표는 AEM(Adobe Experience Manager)의 최신 표준과 기술을 사용하여 웹 사이트를 구현하는 방법을 개발자에게 교육하는 것입니다. 이 자습서를 완료한 후 개발자는 AEM의 일반적인 디자인 패턴에 대한 지식과 함께 플랫폼의 기본 기반을 이해해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 사이트 프로젝트 시작 옵션

AEM Sites 프로젝트를 시작하는 기본적인 방법은 두 가지가 있습니다.

**AEM 프로젝트 원형** - Mahen 템플릿을 사용하여 최소한의 AEM 프로젝트를 생성하여 AEM 개발에 대한 기존의 접근 방식입니다. 이는 과도한 맞춤화를 예상할 수 있는 Cloud Service 프로젝트로 AEM 6.5/6.4 프로젝트와 AEM에 권장되는 방법입니다. 이 튜토리얼에서는 AEM 개발에 대해 자세히 살펴봅니다.

[AEM 프로젝트 원형을 사용하여 자습서 시작](./project-archetype/overview.md)

**AEM 사이트 템플릿**  - 사전 정의된 사이트 템플릿을 사용하여 AEM 사이트를 생성하는 낮은 코드 접근 방식입니다. 즉시 사용 가능한 구성 요소 및 템플릿을 사용하여 사이트를 빠르게 시작하고 실행할 수 있습니다. 테마 워크플로우를 사용하여 CSS 및 JavaScript만으로 브랜드별 스타일과 사용자 지정을 적용할 수 있습니다. 새로운 프로젝트 및 개발자에게 권장됩니다. 현재 Cloud Service으로 AEM에서만 사용할 수 있습니다.

[사이트 템플릿을 사용하여 자습서 시작](./site-template/create-site.md)

## Adobe XD UI 키트

이 튜토리얼을 실제 시나리오의 실제 시나리오에 더 가깝게 만들기 위해 Adobe의 재능 있는 UX 디자이너는 [Adobe XD](https://www.adobe.com/products/xd.html)을 사용하여 해당 사이트의 초안을 만들었습니다. 자습서 과정 동안 다양한 디자인 조각이 저작이 가능한 AEM 사이트에 구현됩니다. **로렌조 부오시** 및 WKND 사이트에 대한 아름다운 디자인을 만든 **킬리안 아멘돌라**&#x200B;에 감사드립니다.

XD UI 키트를 다운로드합니다.

* [AEM 코어 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 참조 사이트 {#reference-site}

WKND 사이트의 완성된 버전을 참조로도 사용할 수 있습니다.[https://wknd.site/](https://wknd.site/)

이 자습서에서는 AEM 개발자에게 필요한 주요 개발 기술을 설명하지만 전체 사이트 전체를 완벽한 사이트를 구축하지는 *않습니다.* 완성된 참조 사이트는 즉시 사용 가능한 AEM 기능을 확인하고 확인할 수 있는 또 다른 유용한 리소스입니다.

자습서로 이동하기 전에 최신 코드를 테스트하려면 GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**에서**[&#x200B;최신 릴리스를 다운로드하여 설치하십시오.

### Adobe Stock 제공

WKND 참조 웹 사이트의 많은 이미지는 [Adobe Stock](https://stock.adobe.com/)에 있으며 데모 에셋 추가 약관([https://www.adobe.com/legal/terms.html](https://www.adobe.com/kr/legal/terms.html)에 정의된 바와 같이 제3자 자료입니다. 웹 사이트 또는 마케팅 자료에 게시하는 등 이 데모 웹 사이트를 볼 수 없는 다른 목적으로 Adobe Stock 이미지를 사용하려면 Adobe Stock에서 라이센스를 구입할 수 있습니다.

Adobe Stock을 사용하면 사진, 그래픽, 비디오, 템플릿 등 1억 4천만 개 이상의 고품질의 로열티 프리 이미지를 이용하여 크리에이티브 프로젝트를 바로 시작할 수 있습니다.

## 다음 단계 {#next-steps}

뭘 기다리고 있어?!AEM 프로젝트 원형](./project-archetype/overview.md) 또는 [사이트 템플릿](./site-template/create-site.md)을 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아봅니다.[
