---
title: 작성 및 게시 소개 | AEM 빠른 사이트 생성
description: Adobe Experience Manager(AEM)의 페이지 편집기를 사용하여 웹 사이트의 콘텐츠를 업데이트합니다. 구성 요소를 사용하여 작성을 용이하게 하는 방법을 알아봅니다. AEM 작성자 및 게시 환경의 차이점을 이해하고 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1285'
ht-degree: 100%

---

# 작성 및 게시 계층 소개 {#author-content-publish}

사용자가 웹 사이트의 콘텐츠를 어떻게 업데이트하는지 이해하는 것이 중요합니다. 이 챕터에서는 **콘텐츠 작성자**&#x200B;의 페르소나를 채택하고 이전 챕터에서 생성한 사이트에 몇 가지 에디토리얼 업데이트를 적용하겠습니다. 챕터 마지막 부분에서는 라이브 사이트가 어떻게 업데이트되는지 이해하기 위해 변경 사항을 게시하겠습니다.

## 사전 요구 사항 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성되어 있으며, [사이트 만들기](./create-site.md) 챕터에 설명된 단계가 완료되었다고 가정합니다.

## 목표 {#objective}

1. AEM Sites의 **페이지** 및 **구성 요소** 개념을 이해합니다.
1. 웹 사이트 콘텐츠를 업데이트하는 방법을 알아봅니다.
1. 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.

## 새 페이지를 만듭니다. {#create-page}

웹 사이트는 일반적으로 여러 페이지로 나뉘어 여러 페이지 환경을 형성합니다. AEM은 동일한 방식으로 콘텐츠를 구성합니다. 다음으로 사이트에 새 페이지를 만듭니다.

1. 이전 챕터에서 사용한 **작성자** 서비스에 로그인합니다.
1. AEM 시작 화면에서 **사이트** > **WKND 사이트** > **영어** > **문서**&#x200B;를 클릭합니다.
1. 오른쪽 상단 모서리에서 **만들기** > **페이지**&#x200B;를 클릭합니다.

   ![페이지 만들기](assets/author-content-publish/create-page-button.png)

   그러면 **페이지 만들기** 마법사가 나타납니다.

1. **문서 페이지** 템플릿을 선택하고 **다음**&#x200B;을 클릭합니다.

   AEM의 페이지는 페이지 템플릿을 기반으로 생성됩니다. 페이지 템플릿은 [페이지 템플릿](page-templates.md) 챕터에서 자세히 살펴보겠습니다.

1. **속성** 아래에 &#39;Hello World&#39;라는 **제목**&#x200B;을 입력합니다.
1. **이름**&#x200B;을 `hello-world`로 설정하고 **만들기**&#x200B;를 클릭합니다.

   ![초기 페이지 속성](assets/author-content-publish/initial-page-properties.png)

1. 대화 상자 팝업에서 **열기**&#x200B;를 클릭하여 새로 만든 페이지를 엽니다.

## 구성 요소 작성 {#author-component}

AEM 구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. UI를 논리적인 덩어리나 구성 요소로 나누면 관리가 훨씬 쉬워집니다. 구성 요소를 재사용하려면 구성 요소가 구성 가능해야 합니다. 이는 작성자 대화 상자를 통해 수행됩니다.

AEM은 프로덕션에 바로 사용할 수 있는 [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 세트를 제공합니다. **핵심 구성 요소**&#x200B;는 [텍스트](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 및 [이미지](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 등의 기본 요소부터 [슬라이드](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)와 같은 복잡한 UI 요소까지 다양합니다.

다음으로 AEM 페이지 편집기를 사용하여 몇 가지 구성 요소를 작성합니다.

1. 이전 연습에서 만든 **Hello World** 페이지로 이동합니다.
1. **편집** 모드인지 확인한 다음 및 왼쪽 측면 레일에서 **구성 요소** 아이콘을 클릭합니다.

   ![페이지 편집기 측면 레일](assets/author-content-publish/page-editor-siderail.png)

   이렇게 하면 구성 요소 라이브러리가 열리고 페이지에서 사용할 수 있는 구성 요소가 열거됩니다.

1. 아래로 스크롤하여 **텍스트(v2)** 구성 요소를 페이지의 주요 편집 영역에 **드래그 앤 드롭**&#x200B;합니다.

   ![텍스트 구성 요소 드래그 앤 드롭](assets/author-content-publish/drag-drop-text-cmp.png)

1. **텍스트** 구성 요소를 클릭하여 강조 표시한 다음 **렌치** 아이콘 ![렌치 아이콘](assets/author-content-publish/wrench-icon.png)을 클릭하여 구성 요소의 대화 상자를 엽니다. 텍스트를 입력하고 대화 상자에 변경 사항을 저장합니다.

   ![리치 텍스트 구성 요소](assets/author-content-publish/rich-text-populated-component.png)

   이제 **텍스트** 구성 요소가 페이지에 리치 텍스트를 표시합니다.

1. 위의 단계를 반복하되 이번에는 **이미지(v2)** 구성 요소의 인스턴스를 페이지로 드래그합니다. **이미지** 구성 요소의 대화 상자를 엽니다.

1. 왼쪽 레일에서 **자산** 아이콘 ![자산 아이콘](assets/author-content-publish/asset-icon.png)을 클릭하여 **자산 파인더**&#x200B;로 전환합니다.
1. 이미지를 구성 요소의 대화 상자에 **드래그 앤 드롭**&#x200B;한 다음 **완료**&#x200B;를 클릭하여 변경 사항을 저장합니다.

   ![대화 상자에 자산 추가](assets/author-content-publish/add-asset-dialog.png)

1. 페이지에 **제목**, **탐색**, **검색** 등 고정된 구성 요소가 있는지 확인합니다. 이러한 영역은 페이지 템플릿의 일부로 구성되며 개별 페이지에서 수정할 수 없습니다. 관련 내용은 다음 챕터에서 더 자세히 살펴보겠습니다.

다른 구성 요소도 자유롭게 시도해 보시기 바랍니다. 각 [핵심 구성 요소 관련 문서는 여기에서 확인](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)할 수 있습니다. [페이지 작성 관련 상세 비디오는 여기에서 확인](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)할 수 있습니다.

## 업데이트 게시 {#publish-updates}

AEM 환경은 **작성 서비스** 및 **게시 서비스** 간에 분할됩니다. 이 챕터에서는 **작성 서비스**&#x200B;에서 사이트에 여러 가지 수정 사항을 적용했습니다. 사이트 방문자가 변경 사항을 볼 수 있도록 하려면 변경 사항을 **게시 서비스**&#x200B;에 게시해야 합니다.

![상위 수준 다이어그램](assets/author-content-publish/author-publish-high-level-flow.png)

*작성자에서 게시에 이르는 상위 수준 콘텐츠 흐름*

**1.** 콘텐츠 작성자는 사이트 콘텐츠를 업데이트합니다. 업데이트를 미리 보고 검토한 후 승인을 받아 실시간으로 적용할 수 있습니다.

**2.** 콘텐츠가 게시됩니다. 게시는 요청 시 진행되거나 추후 일정에 따라 진행될 수 있습니다.

**3.** 사이트 방문자는 게시 서비스에 반영된 변경 사항을 볼 수 있습니다.

### 변경 사항 게시

다음으로 변경 사항을 게시해 보겠습니다.

1. AEM 시작 화면에서 **사이트**&#x200B;로 이동한 다음 **WKND 사이트**&#x200B;를 선택합니다.
1. 메뉴 바에서 **게시 관리**&#x200B;를 클릭합니다.

   ![게시 관리](assets/author-content-publish/click-manage-publiciation.png)

   이 사이트는 새로운 사이트이므로 모든 페이지를 게시하고, 게시 관리 마법사를 사용하여 정확히 무엇을 게시할지 정해 보겠습니다.

1. **옵션**&#x200B;에서 기본 설정을 **게시**&#x200B;로 그대로 둔 다음 **지금**&#x200B;으로 예약합니다. **다음**&#x200B;을 클릭합니다.
1. **범위**&#x200B;에서 **WKND 사이트**&#x200B;를 선택하고 **하위 설정 포함**&#x200B;을 클릭합니다. 대화 상자에서 **하위 포함**&#x200B;을 선택합니다. 나머지 상자의 선택을 해제하여 사이트 전체가 게시되도록 합니다.

   ![게시 범위 업데이트](assets/author-content-publish/update-scope-publish.png)

1. **게시된 참조** 버튼을 클릭합니다. 대화 상자에서 모든 항목이 선택되어 있는지 확인합니다. 여기에는 **표준 사이트 템플릿**&#x200B;과 사이트 템플릿에서 생성된 여러 구성이 포함됩니다. **완료**&#x200B;를 클릭하여 업데이트합니다.

   ![참조 게시](assets/author-content-publish/publish-references.png)

1. 마지막으로 **WKND 사이트**&#x200B;옆에 있는 상자를 선택하고 오른쪽 상단 모서리에 있는 **다음**&#x200B;을 클릭합니다.
1. **워크플로** 단계에서 **워크플로 제목**&#x200B;을 입력합니다. 제목은 어떠한 텍스트든 가능하며 나중에 감사 추적의 일부로 유용할 수 있습니다. &#39;초기 게시&#39;를 입력하고 **게시**&#x200B;를 클릭합니다.

![워크플로 단계 초기 게시](assets/author-content-publish/workflow-step-publish.png)

## 게시된 콘텐츠 보기 {#publish}

다음으로 게시 서비스로 이동하여 변경 사항을 확인합니다.

1. 작성자 URL을 복사하고 `author`라는 단어를 `publish`로 바꾸면 게시 서비스의 URL을 쉽게 얻을 수 있습니다. 예:

   * **작성자 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **게시 URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. `/content/wknd.html`을 추가하여 최종 URL이 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`처럼 보이도록 합니다.

   >[!NOTE]
   >
   > [사이트 생성](create-site.md) 중에 고유한 이름을 부여한 경우, `wknd.html`을 사이트의 이름과 일치하도록 변경합니다.

1. 게시 URL로 이동하면 AEM 작성 기능이 전혀 없는 사이트가 표시됩니다.

   ![게시된 사이트](assets/author-content-publish/publish-url-update.png)

1. **탐색** 메뉴에서 **문서** > **Hello World**&#x200B;를 클릭하여 이전에 만든 Hello World 페이지로 이동합니다.
1. **AEM 작성 서비스**&#x200B;로 돌아가서 페이지 편집기에서 추가로 콘텐츠 변경 사항을 적용합니다.
1. **페이지 속성** 아이콘 > **페이지 게시**&#x200B;를 클릭하여 페이지 편집기 내에서 변경 사항을 직접 게시합니다.

   ![직접 게시](assets/author-content-publish/page-editor-publish.png)

1. **AEM 게시 서비스**&#x200B;로 돌아가 변경 사항을 확인합니다. 즉시 업데이트를 확인하지는 **못할** 가능성이 큽니다. **AEM 게시 서비스**&#x200B;에 [Apache 웹 서버와 CDN을 통한 캐싱](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)이 포함되어 있기 때문입니다. 기본적으로 HTML 콘텐츠는 약 5분 동안 캐시됩니다.

1. 테스트/디버깅 목적으로 캐시를 우회하려면 `?nocache=true`와 같은 쿼리 매개변수를 추가하기만 하면 됩니다. URL은 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`와 비슷합니다. 캐싱 전략 및 사용 가능한 구성에 대한 자세한 내용은 [여기에서 확인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)할 수 있습니다.

1. Cloud Manager에서 게시 서비스의 URL을 찾을 수도 있습니다. **Cloud Manager 프로그램** > **환경** > **환경**&#x200B;으로 이동합니다.

   ![게시 서비스 보기](assets/author-content-publish/view-environment-segments.png)

   **환경 세그먼트**&#x200B;에서 **작성자** 및 **게시** 서비스에 대한 링크를 찾을 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다! AEM 사이트에 대한 변경 사항을 작성하고 게시했습니다.

### 다음 단계 {#next-steps}

실제 구현 계획에서는 일반적으로 사이트 생성에 앞서 목업 및 UI 디자인을 포함한 사이트를 계획합니다. Adobe XD UI 키트를 사용하여 [Adobe XD를 사용한 UI 계획 수립](./ui-planning-adobe-xd.md)에서 Adobe Experience Manager 사이트 구현을 설계하고 가속화하는 방법을 알아보십시오.

AEM Sites 기능을 계속 살펴보시겠습니까? 페이지 템플릿과 페이지의 관계를 이해하려면 [페이지 템플릿](./page-templates.md) 챕터를 바로 읽어 보시기 바랍니다.


