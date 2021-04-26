---
title: 페이지 템플릿
seo-title: AEM Sites 시작하기 - 페이지 템플릿
description: 페이지 템플릿을 만들고 수정하는 방법을 알아봅니다. 페이지 템플릿과 페이지 간의 관계를 파악합니다. 컨텐트에 대한 세부적인 거버넌스 및 브랜드 일관성을 제공하기 위해 페이지 템플릿의 정책을 구성하는 방법에 대해 알아봅니다.  Adobe XD의 초안을 기반으로 잘 구성된 잡지 아티클 템플릿이 생성됩니다.
sub-product: 사이트
version: Cloud Service
type: Tutorial
topic: 컨텐츠 관리
feature: 핵심 구성 요소, 편집 가능한 템플릿, 페이지 편집기
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# 페이지 템플릿 {#page-templates}

>[!CAUTION]
>
> 여기에 소개된 빠른 사이트 제작 기능은 2021년 하반기에 출시될 예정입니다. 관련 설명서는 미리 보기 목적으로 사용할 수 있습니다.

이 장에서는 페이지 템플릿과 페이지 간의 관계를 살펴봅니다. [AdobeXD](https://www.adobe.com/products/xd.html)의 일부 초안을 기반으로 스타일이 지정되지 않은 잡지 아티클 템플릿을 작성합니다. 템플릿을 작성하는 과정에서 핵심 구성 요소 및 고급 정책 구성에 대해 다룹니다.

## 전제 조건 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성되어 있으며 [콘텐츠 작성 및 변경 내용 게시](./author-content-publish.md) 장에 설명된 단계가 완료되었다고 가정합니다.

## 목표

1. Inspect Adobe XD에서 만든 페이지 디자인을 만들어 핵심 구성 요소에 매핑합니다.
1. 페이지 템플릿의 세부 사항과 정책을 사용하여 페이지 컨텐츠를 세부적으로 제어하는 방법을 이해합니다.
1. 템플릿 및 페이지 연결 방법을 알아봅니다.

## {#what-you-will-build} 빌드할 항목

자습서의 이 부분에서는 새 잡지 아티클을 만들고 일반적인 구조에 맞게 정렬할 수 있는 새 잡지 아티클 페이지 템플릿을 만듭니다. 이 템플릿은 AdobeXD로 제작된 디자인과 UI 키트를 기반으로 합니다. 이 장은 템플릿의 구조나 뼈대를 빌드하는 데에만 중점을 둡니다. 스타일은 구현되지 않지만 템플릿과 페이지가 작동합니다.

## Adobe XD {#adobexd}의 UI 계획

대부분의 경우 새 웹 사이트를 계획하는 것은 목업 및 정적 디자인으로 시작됩니다. [Adobe ](https://www.adobe.com/products/xd.html) XD는 사용자 경험을 구축하기 위한 디자인 툴입니다. 다음으로 아티클 페이지 템플릿의 구조를 계획하는 데 도움이 되는 UI 키트 및 초안을 검사합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**WKND  [아티클 디자인 파일을 다운로드합니다](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 일반 [AEM 코어 구성 요소 UI 키트도 사용자 지정 프로젝트의 시작점으로 사용할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd).

## 잡지 기사 페이지 템플릿 만들기

페이지를 생성할 때 새 페이지를 만드는 기준으로 사용할 템플릿을 선택해야 합니다. 템플릿은 결과 페이지의 구조, 초기 컨텐츠 및 허용된 구성 요소의 구조를 정의합니다.

[페이지 템플릿](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)에는 3개의 기본 영역이 있습니다.

1. **구조**  - 템플릿의 일부인 구성 요소를 정의합니다. 컨텐츠 작성자가 편집할 수 없습니다.
1. **초기 컨텐츠**  - 템플릿이 시작할 구성 요소를 정의하며 컨텐츠 작성자가 편집 및/또는 삭제할 수 있습니다.
1. **정책**  - 구성 요소의 작동 방식과 작성자가 사용할 수 있는 옵션에 대한 구성을 정의합니다.

다음으로 목업의 구조와 일치하는 새 템플릿을 AEM에서 만듭니다. AEM의 로컬 인스턴스에서 발생합니다. 아래 비디오의 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### 솔루션 패키지

Package Manager를 통해 잡지 템플릿](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)의 완료된 [솔루션을 다운로드하여 설치할 수 있습니다.

## 경험 조각 {#experience-fragments}으로 머리글 및 바닥글 업데이트

머리글 또는 바닥글과 같은 글로벌 컨텐츠를 만들 때 일반적으로 사용하는 방법은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)을 사용하는 것입니다. 경험 조각을 사용하면 여러 구성 요소를 결합하여 하나의 참조 가능 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [현지화](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)를 지원하는 이점이 있습니다.

사이트 템플릿에서 머리글과 바닥글을 만들었습니다. 다음으로 경험 조각을 업데이트하여 목업을 일치시킵니다. 아래 비디오의 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

아래 비디오에 대한 높은 단계:

1. 샘플 컨텐츠 패키지 **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**&#x200B;을 다운로드합니다.
1. 패키지 관리자를 사용하여 컨텐츠 패키지를 업로드하고 설치합니다.
1. WKND 로고를 사용하도록 머리글 및 바닥글 경험 조각을 업데이트합니다.

## 잡지 기사 페이지 만들기

그런 다음 잡지 기사 페이지 템플릿을 사용하여 새 페이지를 만듭니다. 사이트 목업에 맞게 페이지의 컨텐츠를 작성합니다. 아래 비디오의 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## 축하합니다!{#congratulations}

축하합니다. Adobe Experience Manager Sites에서 새 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 잡지 기사 페이지와 사이트가 WKND의 브랜드 스타일과 일치하지 않습니다. 사이트에 전역 스타일을 적용하는 데 사용되는 CSS 및 Javascript 프런트 엔드 코드를 업데이트하는 우수 사례를 살펴보려면 [테마](theming.md) 자습서를 따르십시오.

### 솔루션 패키지

이 장의 솔루션 패키지를 다운로드할 수 있습니다.[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)
