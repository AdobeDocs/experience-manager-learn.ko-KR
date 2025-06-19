---
title: AEM Sites 시작하기 - WKND 튜토리얼
description: WKND라는 가상의 라이프스타일 브랜드를 위한 AEM 사이트를 구현하는 방법을 알아봅니다. 프로젝트 설정, Maven Archetype, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트 라이브러리 및 구성 요소 개발 등 Experience Manager의 기본 주제를 단계별로 자세히 살펴봅니다.
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '577'
ht-degree: 100%

---

# AEM Sites 시작하기 - WKND 튜토리얼 {#introduction}

{{traditional-aem}}

Adobe Experience Manager(AEM)를 처음 사용하는 개발자를 위해 설계된 멀티 파트 튜토리얼을 시작합니다. 이 튜토리얼은 WKND라는 가상의 라이프스타일 브랜드를 위한 AEM 사이트의 구현 과정을 안내합니다. 이 튜토리얼은 Adobe Experience Manager Sites를 통한 사용한 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 구성 요소 개발 등의 기본 주제를 다룹니다.

## 개요 {#wknd-tutorial-overview}

이 멀티 파트 튜토리얼의 목표는 개발자를 대상으로 최신 표준 기술을 통해 Adobe Experience Manager(AEM)에서 웹 사이트를 구현하는 방법을 교육하는 것입니다. 이 튜토리얼을 완료하면 개발자는 AEM의 플랫폼 기본 토대와 일반적인 디자인 패턴을 이해하게 됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Sites 프로젝트 시작을 위한 옵션

AEM Sites 프로젝트를 시작하는 데에는 두 가지 기본적인 접근 방식이 있습니다.

**AEM Project Archetype** - Maven 템플릿을 사용하여 최소한의 AEM 프로젝트를 생성하는 전통적인 AEM 개발 방식입니다. 많은 맞춤화 작업이 예상되는 AEM 6.5/6.4 프로젝트와 AEM as a Cloud Service 프로젝트에 권장되는 접근 방식입니다. 이 튜토리얼에서는 AEM 개발에 대해 더 심층적으로 설명합니다.

[AEM Project Archetype으로 튜토리얼 시작](./project-archetype/overview.md)

**AEM 사이트 템플릿** - 빠른 사이트 생성이라고도 하며, 미리 정의된 사이트 템플릿을 사용하여 AEM 사이트를 생성하는 로우 코드 접근법입니다. 기본 제공 구성 요소와 템플릿을 사용하여 사이트를 빠르게 빌드하고 실행할 수 있습니다. 테마 워크플로를 사용하여 CSS와 JavaScript만으로 브랜드별 스타일과 사용자 정의를 적용할 수 있습니다. 새로운 프로젝트와 초보 개발자에게 권장됩니다. AEM as a Cloud Service에서만 사용할 수 있습니다.

[사이트 템플릿을 사용하여 튜토리얼 시작](./site-template/create-site.md)

## Adobe XD UI 키트

이 튜토리얼을 실제 시나리오에 더 가깝게 만들기 위해, Adobe의 재능 있는 UX 디자이너들이 [Adobe XD](https://www.adobe.com/kr/products/xd.html)를 사용하여 사이트의 모형을 제작했습니다. 튜토리얼 전반에 걸쳐 이러한 디자인 요소가 완전히 작성 가능한 AEM 사이트로 구현됩니다. 아름다운 WKND 사이트 디자인을 제작한 **Lorenzo Buosi**&#x200B;와 **Kilian Amendola**&#x200B;에게 특별한 감사를 전합니다.

XD UI 키트를 다운로드하십시오.

* [AEM 핵심 구성 요소 UI 키트](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI 키트](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 참조 사이트 {#reference-site}

완성된 WKND 사이트 버전은 [https://wknd.site/](https://wknd.site/)에서 참조용으로 확인할 수 있습니다.

튜토리얼에서는 AEM 개발자에게 필요한 주요 개발 기술을 다루지만 전체 사이트를 처음부터 끝까지 빌드하지는 *않습니다*. 완성된 참조 사이트는 AEM의 기본 제공 기능을 더 자세히 알아보고 탐색할 수 있는 또 다른 유용한 리소스입니다.

튜토리얼을 시작하기 전에 최신 코드를 테스트하려면 **[GitHub에서 최신 릴리스](https://github.com/adobe/aem-guides-wknd/releases/latest)**&#x200B;를 다운로드하여 설치하십시오.

### Adobe Stock 제공

WKND 참조 웹 사이트의 많은 이미지는 [Adobe Stock](https://stock.adobe.com/)에서 제공되었으며, 데모 자산 추가 약관([https://www.adobe.com/kr/legal/terms.html](https://www.adobe.com/kr/legal/terms.html))에 정의된 서드파티 자료입니다. 이 데모 웹 사이트를 열람하려는 목적 외에 Adobe Stock 이미지를 웹 사이트에 게재하거나 마케팅 자료로 활용하려면 Adobe Stock에서 라이선스를 구매해야 합니다.

Adobe Stock을 이용하면 사진, 그래픽, 비디오, 템플릿 등 1억 4천만 개 이상의 고품질 로열티 프리 이미지를 활용하여 크리에이티브 프로젝트를 빠르게 시작할 수 있습니다.

## 다음 단계 {#next-steps}

지금 바로 [AEM Project Archetype을 사용하여 Adobe Experience Manager 프로젝트를 생성](./project-archetype/overview.md)하는 방법 또는 [사이트 템플릿을 사용하여 사이트를 만드는](./site-template/create-site.md) 방법을 알아보십시오.
