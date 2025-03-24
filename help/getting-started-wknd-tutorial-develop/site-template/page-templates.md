---
title: 페이지 템플릿
description: 페이지 템플릿을 만들고 수정하는 방법을 알아봅니다. 페이지 템플릿과 페이지 간의 관계를 이해합니다. 컨텐츠에 대한 세분화된 거버넌스 및 브랜드 일관성을 제공하기 위해 페이지 템플릿의 정책을 구성하는 방법에 대해 알아봅니다.  잘 구성된 매거진 문서 템플릿은 Adobe XD의 목차를 기반으로 만들어집니다.
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
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# 페이지 템플릿 {#page-templates}

이 장에서는 페이지 템플릿과 페이지 간의 관계를 살펴봅니다. [AdobeXD](https://www.adobe.com/products/xd.html)의 일부 모형을 기반으로 스타일이 지정되지 않은 Magazine Article 템플릿을 빌드합니다. 템플릿을 작성하는 과정에서 핵심 구성 요소 및 고급 정책 구성에 대해 다룹니다.

## 사전 요구 사항 {#prerequisites}

여러 부분으로 구성된 자습서이며 [콘텐츠 작성 및 변경 내용 게시](./author-content-publish.md) 장에 설명된 단계가 완료된 것으로 간주됩니다.

## 목표

1. 페이지 템플릿에 대한 세부 정보와 정책을 사용하여 페이지 콘텐츠를 세부적으로 제어하는 방법을 이해합니다.
1. 템플릿과 페이지가 연결되는 방법에 대해 알아봅니다.
1. 새 템플릿을 만들고 페이지를 작성합니다.

## 빌드할 내용 {#what-you-will-build}

이 자습서 부분에서는 새 잡지 문서를 만들고 공통 구조에 맞게 조정하는 데 사용할 수 있는 새 잡지 문서 페이지 템플릿을 작성합니다. 템플릿은 AdobeXD에서 제작된 디자인과 UI 키트를 기반으로 합니다. 이 장은 템플릿의 구조 또는 뼈대를 작성하는 데에만 중점을 둡니다. 스타일은 구현되지 않지만 템플릿과 페이지는 작동합니다.

## 매거진 문서 페이지 템플릿 만들기

페이지를 만들 때 새 페이지를 만들 때 기준으로 사용되는 템플릿을 선택해야 합니다. 템플릿은 결과 페이지, 초기 콘텐츠 및 허용된 구성 요소의 구조를 정의합니다.

[페이지 템플릿](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=ko)에는 세 가지 기본 영역이 있습니다.

1. **구조** - 템플릿의 일부인 구성 요소를 정의합니다. 콘텐츠 작성자는 편집할 수 없습니다.
1. **초기 콘텐츠** - 템플릿이 시작되는 구성 요소를 정의하며 콘텐츠 작성자는 해당 구성 요소를 편집 및/또는 삭제할 수 있습니다.
1. **정책** - 구성 요소의 작동 방식과 작성자가 사용할 수 있는 옵션에 대한 구성을 정의합니다.

그런 다음 AEM에서 목차의 구조와 일치하는 새 템플릿을 만듭니다. 이 문제는 AEM의 로컬 인스턴스에서 발생합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

다음 썸네일을 사용하여 템플릿을 식별(또는 자체 업로드!)할 수 있습니다.

![문서 페이지 템플릿 썸네일](./assets/page-templates/article-page-template-thumbnail.png)


### 솔루션 패키지

완료된 [매거진 템플릿의 솔루션](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip)은 패키지 관리자를 통해 다운로드하고 설치할 수 있습니다.

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

머리글이나 바닥글과 같은 전역 콘텐츠를 만들 때 일반적으로 사용하는 방법은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)입니다. 경험 조각 을 사용하면 여러 구성 요소를 결합하여 참조 가능한 단일 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [로컬라이제이션](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)을 지원할 수 있는 이점이 있습니다.

사이트 템플릿에서 머리글과 바닥글을 생성했습니다. 그런 다음, mockup과 일치하도록 경험 조각 을 업데이트합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

아래 비디오에 대한 높은 수준의 단계:

1. 샘플 콘텐츠 패키지 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**&#x200B;을(를) 다운로드하십시오.
1. 패키지 관리자를 사용하여 콘텐츠 패키지를 업로드하고 설치합니다.
1. WKND 로고를 사용하도록 머리글 및 바닥글 경험 조각 업데이트

## 매거진 문서 페이지 만들기

그런 다음 잡지 기사 페이지 템플릿을 사용하여 새 페이지를 만듭니다. 사이트 모형과 일치하도록 페이지 콘텐츠를 작성합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

[제공된 텍스트](./assets/page-templates/la-skateparks-copy.txt)를 사용하여 문서 본문을 채우십시오.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites으로 새 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 잡지 기사 페이지와 사이트는 WKND의 브랜드 스타일과 일치하지 않습니다. 사이트에 전역 스타일을 적용하는 데 사용되는 CSS 및 Javascript 프론트엔드 코드를 업데이트하는 모범 사례를 알아보려면 [테마](theming.md) 튜토리얼을 따르십시오.

### 솔루션 패키지

이 장에 대한 솔루션 패키지를 다운로드할 수 있습니다. [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
