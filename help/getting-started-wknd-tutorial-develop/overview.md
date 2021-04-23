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
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 5%

---


# AEM Sites 시작하기 - WKND 튜토리얼 {#introduction}

AEM(Adobe Experience Manager)을 처음 사용하는 개발자를 위해 고안된 여러 부분으로 구성된 자습서입니다. 이 자습서에서는 WKND라는 가상의 라이프스타일 브랜드에 대한 AEM 사이트 구현을 안내합니다. 이 자습서에서는 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 Adobe Experience Manager Sites와의 구성 요소 개발과 같은 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 여러 부분으로 구성된 튜토리얼의 목표는 AEM(Adobe Experience Manager)의 최신 표준과 기술을 사용하여 웹 사이트를 구현하는 방법을 개발자에게 교육하는 것입니다. 이 자습서를 완료한 후 개발자는 AEM의 일반적인 디자인 패턴에 대한 지식과 함께 플랫폼의 기본 기반을 이해해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 자습서 {#about-tutorial} 정보

WKND는 여러 국제 도시에서 밤 문화, 활동, 그리고 이벤트들에 초점을 맞춘 가상의 온라인 잡지 및 블로그입니다.

### Adobe XD UI 키트

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

뭘 기다리고 있어?!자습서를 시작하고 AEM 프로젝트 원형](./project-archetype/overview.md)을(를) 사용하여 새 Adobe Experience Manager 프로젝트를 생성하는 방법을 알아봅니다.[
