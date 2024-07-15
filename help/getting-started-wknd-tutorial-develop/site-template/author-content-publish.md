---
title: 작성 및 게시 소개 | AEM 빠른 사이트 생성
description: Adobe Experience Manager, AEM의 페이지 편집기를 사용하여 웹 사이트의 콘텐츠를 업데이트합니다. 작성을 용이하게 하기 위해 구성 요소를 사용하는 방법을 알아봅니다. AEM Author와 Publish 환경의 차이점을 이해하고 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# 작성 및 게시 소개 {#author-content-publish}

사용자가 웹 사이트에 대한 콘텐츠를 업데이트하는 방법을 이해하는 것이 중요합니다. 이 장에서는 **콘텐츠 작성자**&#x200B;의 담당자를 채택하고 이전 장에서 생성한 사이트에 대한 일부 에디토리얼 업데이트를 수행합니다. 챕터 끝에 라이브 사이트가 업데이트되는 방식을 이해하는 변경 사항을 게시합니다.

## 사전 요구 사항 {#prerequisites}

여러 부분으로 구성된 자습서이며 [사이트 만들기](./create-site.md) 장에 설명된 단계가 완료된 것으로 간주됩니다.

## 목표 {#objective}

1. AEM Sites의 **페이지** 및 **구성 요소**&#x200B;에 대한 개념을 이해합니다.
1. 웹 사이트의 콘텐츠를 업데이트하는 방법을 알아봅니다.
1. 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.

## 새 페이지 만들기 {#create-page}

웹 사이트는 일반적으로 여러 페이지로 나뉘어 다중 페이지 경험을 형성합니다. AEM은 동일한 방식으로 콘텐츠를 구조화합니다. 그런 다음 사이트에 대한 새 페이지를 만듭니다.

1. 이전 장에서 사용한 AEM **작성자** 서비스에 로그인합니다.
1. AEM 시작 화면에서 **사이트** > **WKND 사이트** > **영어** > **기사**&#x200B;를 클릭합니다
1. 오른쪽 상단에서 **만들기** > **페이지**&#x200B;를 클릭합니다.

   ![페이지 만들기](assets/author-content-publish/create-page-button.png)

   **페이지 만들기** 마법사가 실행됩니다.

1. **문서 페이지** 템플릿을 선택하고 **다음**&#x200B;을(를) 클릭합니다.

   AEM의 페이지는 페이지 템플릿을 기반으로 만들어집니다. 페이지 템플릿은 [페이지 템플릿](page-templates.md) 챕터에서 자세히 살펴봅니다.

1. **속성**&#x200B;에서 &quot;Hello World&quot;의 **제목**&#x200B;을 입력하십시오.
1. **이름**&#x200B;을(를) `hello-world`(으)로 설정하고 **만들기**&#x200B;를 클릭합니다.

   ![초기 페이지 속성](assets/author-content-publish/initial-page-properties.png)

1. 대화 상자 팝업에서 **열기**&#x200B;를 클릭하여 새로 만든 페이지를 엽니다.

## 구성 요소 작성 {#author-component}

AEM 구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. UI를 논리적 청크 또는 구성 요소로 분류하면 관리가 훨씬 쉬워집니다. 구성 요소를 다시 사용하려면 구성 요소를 구성할 수 있어야 합니다. 이 작업은 작성자 대화 상자를 통해 수행됩니다.

AEM은(는) 프로덕션에서 사용할 준비가 된 [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko-KR) 집합을 제공합니다. **핵심 구성 요소**&#x200B;의 범위는 [Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 및 [Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)와 같은 기본 요소에서 [회전 메뉴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)와 같은 보다 복잡한 UI 요소까지 다양합니다.

그런 다음 AEM 페이지 편집기를 사용하여 몇 가지 구성 요소를 작성합니다.

1. 이전 연습에서 만든 **Hello World** 페이지로 이동합니다.
1. **편집** 모드에 있는지 확인하고 왼쪽 레일에서 **구성 요소** 아이콘을 클릭합니다.

   ![페이지 편집기 쪽 레일](assets/author-content-publish/page-editor-siderail.png)

   이렇게 하면 구성 요소 라이브러리가 열리고 페이지에서 사용할 수 있는 구성 요소가 나열됩니다.

1. 아래로 스크롤하여 **텍스트(v2)** 구성 요소를 페이지의 기본 편집 가능 영역으로 **끌어다 놓기**&#x200B;합니다.

   ![텍스트 구성 요소 끌어서 놓기](assets/author-content-publish/drag-drop-text-cmp.png)

1. **텍스트** 구성 요소를 클릭하여 강조 표시한 다음 **렌치** 아이콘 ![렌치 아이콘](assets/author-content-publish/wrench-icon.png)을 클릭하여 구성 요소의 대화 상자를 엽니다. 텍스트를 입력하고 대화 상자에 변경 사항을 저장합니다.

   ![서식 있는 텍스트 구성 요소](assets/author-content-publish/rich-text-populated-component.png)

   이제 **Text** 구성 요소에 페이지에 서식 있는 텍스트가 표시됩니다.

1. **Image(v2)** 구성 요소의 인스턴스를 페이지로 드래그하는 것을 제외하고 위의 단계를 반복합니다. **이미지** 구성 요소의 대화 상자를 엽니다.

1. 왼쪽 레일에서 **Assets** 아이콘 ![자산 아이콘](assets/author-content-publish/asset-icon.png)을 클릭하여 **자산 파인더**(으)로 전환합니다.
1. 구성 요소의 대화 상자로 이미지를 **끌어다 놓기**&#x200B;하고 **완료**&#x200B;를 클릭하여 변경 내용을 저장합니다.

   ![대화 상자에 자산 추가](assets/author-content-publish/add-asset-dialog.png)

1. **제목**, **탐색**, **검색**&#x200B;과 같이 페이지에 수정된 구성 요소가 있는지 확인하십시오. 이러한 영역은 페이지 템플릿의 일부로 구성되며 개별 페이지에서 수정할 수 없습니다. 이 내용은 다음 장에서 더 자세히 알아봅니다.

다른 구성 요소 중 일부를 자유롭게 실험해 보십시오. 각 [핵심 구성 요소에 대한 설명서는 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko-KR)에서 확인할 수 있습니다. [페이지 작성에 대한 자세한 비디오 시리즈는 여기에서 찾을 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publish 업데이트 {#publish-updates}

AEM 환경은 **작성자 서비스**&#x200B;와 **Publish 서비스** 간에 분할됩니다. 이 장에서는 **작성자 서비스**&#x200B;의 사이트를 몇 가지 수정했습니다. 사이트 방문자가 변경 내용을 보려면 **Publish 서비스**&#x200B;에 게시해야 합니다.

![높은 수준 다이어그램](assets/author-content-publish/author-publish-high-level-flow.png)

*작성자에서 Publish으로 높은 수준의 콘텐츠 흐름*

**1.** 콘텐츠 작성자가 사이트 콘텐츠를 업데이트합니다. 업데이트는 미리보기, 검토 및 승인하여 실시간으로 푸시할 수 있습니다.

**2.** 콘텐츠가 게시되었습니다. 게시는 온디맨드로 수행하거나 미래 날짜로 예약할 수 있습니다.

**3.** 사이트 방문자에게 Publish 서비스에 반영된 변경 내용이 표시됩니다.

### Publish 변경 사항

다음으로 변경 사항을 게시하겠습니다.

1. AEM 시작 화면에서 **사이트**(으)로 이동하여 **WKND 사이트**&#x200B;를 선택합니다.
1. 메뉴 표시줄에서 **게시 관리**&#x200B;를 클릭합니다.

   ![게시 관리](assets/author-content-publish/click-manage-publiciation.png)

   완전히 새로운 사이트이므로 모든 페이지를 게시하려고 하며 게시 관리 마법사를 사용하여 게시해야 하는 항목을 정확하게 정의할 수 있습니다.

1. **옵션**&#x200B;에서 기본 설정을 **Publish**(으)로 유지하고 **지금** 일정을 예약합니다. **다음**&#x200B;을 클릭합니다.
1. **범위**&#x200B;에서 **WKND 사이트**&#x200B;를 선택하고 **하위 설정 포함**&#x200B;을 클릭합니다. 대화 상자에서 **하위 항목 포함**&#x200B;을 선택합니다. 전체 사이트가 게시되도록 나머지 상자의 선택을 취소합니다.

   ![게시 범위 업데이트](assets/author-content-publish/update-scope-publish.png)

1. **게시된 참조** 단추를 클릭합니다. 대화 상자에서 모든 항목이 선택되어 있는지 확인합니다. 여기에는 **표준 사이트 템플릿** 및 사이트 템플릿에서 생성한 여러 구성이 포함됩니다. 업데이트하려면 **완료**&#x200B;를 클릭하세요.

   ![Publish 참조](assets/author-content-publish/publish-references.png)

1. 마지막으로 **WKND Site** 옆의 확인란을 선택하고 오른쪽 상단의 **다음**&#x200B;을 클릭합니다.
1. **워크플로** 단계에서 **워크플로 제목**&#x200B;을 입력하십시오. 모든 텍스트가 될 수 있으며 나중에 감사 추적의 일부로 유용할 수 있습니다. &quot;초기 게시&quot;를 입력하고 **Publish**&#x200B;을(를) 클릭합니다.

![워크플로 단계 초기 게시](assets/author-content-publish/workflow-step-publish.png)

## 게시된 콘텐츠 보기 {#publish}

그런 다음 Publish 서비스로 이동하여 변경 사항을 확인합니다.

1. Publish 서비스의 URL을 쉽게 가져올 수 있는 방법은 작성자 URL을 복사하고 `author` 단어를 `publish`(으)로 바꾸는 것입니다. 예:

   * **작성자 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **Publish URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 최종 URL이 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`처럼 보이도록 Publish URL에 `/content/wknd.html`을(를) 추가합니다.

   >[!NOTE]
   >
   > [사이트 생성](create-site.md) 중에 고유한 이름을 제공한 경우 `wknd.html`을(를) 사이트 이름과 일치하도록 변경하십시오.

1. Publish URL로 이동하면 AEM 작성 기능 없이 사이트가 표시됩니다.

   ![게시된 사이트](assets/author-content-publish/publish-url-update.png)

1. **탐색** 메뉴를 사용하여 **문서** > **Hello World**&#x200B;를 클릭하여 이전에 만든 Hello World 페이지로 이동합니다.
1. **AEM 작성자 서비스**(으)로 돌아가서 페이지 편집기에서 콘텐츠를 추가로 변경합니다.
1. **페이지 속성** 아이콘 > **Publish 페이지**&#x200B;를 클릭하여 페이지 편집기 내에서 직접 이러한 변경 내용을 Publish

   ![직접 게시](assets/author-content-publish/page-editor-publish.png)

1. 변경 사항을 보려면 **AEM Publish 서비스**(으)로 돌아가십시오. 업데이트를 즉시 **확인하지**&#x200B;못할 수 있습니다. 이는 **AEM Publish 서비스**&#x200B;에 Apache 웹 서버 및 CDN을 통한 [캐싱](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)이 포함되어 있기 때문입니다. 기본적으로 HTML 콘텐츠는 ~5분 동안 캐시됩니다.

1. 테스트/디버깅 목적으로 캐시를 무시하려면 `?nocache=true`과(와) 같은 쿼리 매개 변수를 추가하면 됩니다. URL은 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`과(와) 같습니다. 캐싱 전략 및 사용 가능한 구성에 대한 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)를 참조하세요.

1. Cloud Manager에서 Publish 서비스 URL을 찾을 수도 있습니다. **Cloud Manager 프로그램** > **환경** > **환경**&#x200B;으로 이동합니다.

   ![Publish 서비스 보기](assets/author-content-publish/view-environment-segments.png)

   **환경 세그먼트**&#x200B;에서 **작성자** 및 **Publish** 서비스에 대한 링크를 찾을 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. AEM 사이트에 대한 변경 사항을 방금 작성 및 게시했습니다!

### 다음 단계 {#next-steps}

실제 구현에서는 일반적으로 사이트 생성 전에 mockup 및 UI 디자인이 있는 사이트를 계획합니다. Adobe XD UI 키트를 사용하여 [Adobe XD을 사용한 UI 계획](./ui-planning-adobe-xd.md)에서 Adobe Experience Manager Sites 구현을 디자인하고 가속화하는 방법에 대해 알아봅니다.

AEM Sites 기능을 계속 살펴보시겠습니까? 언제든지 [페이지 템플릿](./page-templates.md)의 챕터로 이동하여 페이지 템플릿과 페이지 간의 관계를 이해할 수 있습니다.


