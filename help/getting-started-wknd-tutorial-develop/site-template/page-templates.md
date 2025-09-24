---
title: 페이지 템플릿
description: 페이지 템플릿 생성 및 수정 방법을 알아봅니다. 페이지 템플릿과 페이지의 관계를 이해합니다. 콘텐츠에 대한 세부적인 거버넌스와 브랜드 일관성을 제공하도록 페이지 템플릿의 정책을 구성하는 방법을 알아봅니다.  Adobe XD의 목업을 기반으로 잘 구성된 매거진 문서 템플릿이 생성되었습니다.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '628'
ht-degree: 100%

---

# 페이지 템플릿 {#page-templates}

이 챕터에서는 페이지 템플릿과 페이지의 관계를 살펴봅니다. [AdobeXD](https://www.adobe.com/kr/products/xd.html)의 목업 몇 가지를 기반으로 스타일이 지정되지 않은 잡지 기사 템플릿을 만들어 보겠습니다. 템플릿을 빌드하는 과정에서 핵심 구성 요소와 고급 정책 구성을 다룹니다.

## 사전 요구 사항 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성되어 있으며 [콘텐츠 작성 및 변경 사항 게시](./author-content-publish.md) 챕터에 설명된 단계가 완료되었다고 가정합니다.

## 목표

1. 페이지 템플릿의 세부 사항과 정책을 사용하여 페이지 콘텐츠에 대한 세부적인 제어를 시행하는 방법을 알아봅니다.
1. 템플릿과 페이지가 어떻게 연결되는지 알아봅니다.
1. 새로운 템플릿을 만들고 페이지를 작성합니다.

## 빌드 결과물 {#what-you-will-build}

튜토리얼의 이 부분에서는 새로운 매거진 문서를 만들고 공통 구조에 맞춰 일관성을 부여하는 데 사용할 수 있는 새로운 매거진 문서 페이지 템플릿을 만들어 보겠습니다. 이 템플릿은 AdobeXD로 제작된 디자인과 UI 키트를 기반으로 합니다. 이 챕터에서는 템플릿의 구조나 뼈대를 구성하는 데에만 초점을 맞춥니다. 스타일은 구현되지 않았지만 템플릿과 페이지는 기능적입니다.

## 매거진 문서 페이지 템플릿 만들기

페이지를 만들 때 새 페이지를 만드는 기준으로 사용할 템플릿을 선택해야 합니다. 템플릿은 결과 페이지의 구조, 초기 콘텐츠 및 허용되는 구성 요소를 정의합니다.

[페이지 템플릿](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=ko-kr)에는 3가지 주요 영역이 있습니다.

1. **구조** - 템플릿의 일부인 구성 요소를 정의합니다. 이러한 구성 요소는 콘텐츠 작성자가 편집할 수 없습니다.
1. **초기 콘텐츠** - 템플릿에서 시작 단계에 사용하는 구성 요소를 정의합니다. 이러한 구성 요소는 콘텐츠 작성자가 편집 및/또는 삭제할 수 있습니다.
1. **정책** - 구성 요소의 동작 방식과 작성자가 사용할 수 있는 옵션에 대한 구성을 정의합니다.

다음으로 목업의 구조와 일치하는 새 템플릿을 AEM에서 만듭니다. 이 작업은 AEM의 로컬 인스턴스에서 수행됩니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

다음 썸네일을 사용하거나 직접 업로드하여 템플릿을 식별할 수 있습니다.

![문서 페이지 템플릿 썸네일](./assets/page-templates/article-page-template-thumbnail.png)


### 솔루션 패키지

완성된 [매거진 템플릿 솔루션](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip)은 패키지 관리자를 통해 다운로드하여 설치할 수 있습니다.

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

헤더나 푸터와 같은 글로벌 콘텐츠를 만들 때 일반적인 관행은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ko)을 사용하는 것입니다. 경험 조각을 사용하면 사용자가 여러 구성 요소를 결합하여 참조 가능한 단일 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [현지화](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=ko#localized-site-structure)를 지원한다는 장점이 있습니다.

사이트 템플릿이 머리글과 바닥글을 생성했습니다. 다음으로 목업과 일치하도록 경험 조각을 업데이트합니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

아래 비디오에 대한 상위 수준 단계:

1. 샘플 콘텐츠 패키지인 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**&#x200B;을 다운로드합니다.
1. 패키지 관리자를 사용하여 콘텐츠 패키지를 업로드하고 설치합니다.
1. WKND 로고를 사용하도록 머리글 및 바닥글 경험 조각을 업데이트

## 매거진 문서 페이지 만들기

다음으로 매거진 문서 페이지 템플릿을 사용하여 새 페이지를 만듭니다. 사이트 목업에 맞게 페이지 콘텐츠를 작성합니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

[제공된 텍스트](./assets/page-templates/la-skateparks-copy.txt)를 사용하여 문서 본문을 채웁니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites를 사용하여 새 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 매거진 문서 페이지와 사이트는 WKND의 브랜드 스타일과 일치하지 않습니다. 사이트에 전역적으로 스타일을 적용하는 데 사용되는 CSS 및 Javascript 프론트엔드 코드를 업데이트하는 모범 사례를 알아보려면 [테마 설정](theming.md) 튜토리얼을 참조하시기 바랍니다.

### 솔루션 패키지

이 챕터에 대한 솔루션 패키지인 [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)을 다운로드할 수 있습니다.
