---
title: 작성 및 게시 소개 | AEM 빠른 사이트 생성
description: Adobe Experience Manager, AEM의 페이지 편집기를 사용하여 웹 사이트의 콘텐츠를 업데이트합니다. 작성을 용이하게 하기 위해 구성 요소를 사용하는 방법을 알아봅니다. AEM 작성자와 게시 환경의 차이점을 이해하고 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 3%

---

# 작성 및 게시 소개 {#author-content-publish}

사용자가 웹 사이트에 대한 콘텐츠를 업데이트하는 방법을 이해하는 것이 중요합니다. 이 장에서는 의 특성을 채택합니다. **콘텐츠 작성자** 그리고 이전 장에서 생성한 사이트에 대한 몇 가지 에디토리얼 업데이트를 수행합니다. 챕터 끝에 라이브 사이트가 업데이트되는 방식을 이해하는 변경 사항을 게시합니다.

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 다음에 설명된 단계를 가정합니다. [사이트 만들기](./create-site.md) 챕터가 완료되었습니다.

## 목표 {#objective}

1. 의 개념 이해 **페이지** 및 **구성 요소** AEM Sites.
1. 웹 사이트의 콘텐츠를 업데이트하는 방법을 알아봅니다.
1. 라이브 사이트에 변경 사항을 게시하는 방법을 알아봅니다.

## 새 페이지 만들기 {#create-page}

웹 사이트는 일반적으로 여러 페이지로 나뉘어 다중 페이지 경험을 형성합니다. AEM은 동일한 방식으로 콘텐츠를 구조화합니다. 그런 다음 사이트에 대한 새 페이지를 만듭니다.

1. AEM에 로그인 **작성자** 이전 장에서 사용된 서비스.
1. AEM 시작 화면에서 **사이트** > **WKND 사이트** > **영어** > **기사**
1. 오른쪽 상단에서 를 클릭합니다. **만들기** > **페이지**.

   ![페이지 만들기](assets/author-content-publish/create-page-button.png)

   이렇게 하면 다음 항목이 표시됩니다. **페이지 만들기** 마법사.

1. 다음을 선택합니다. **문서 페이지** 템플릿 및 클릭 **다음**.

   AEM의 페이지는 페이지 템플릿을 기반으로 만들어집니다. 페이지 템플릿은에서 자세히 살펴봅니다. [페이지 템플릿](page-templates.md) 챕터.

1. 아래 **속성** a 입력 **제목** &quot;Hello World&quot;
1. 설정 **이름** 대상 `hello-world` 및 클릭 **만들기**.

   ![초기 페이지 속성](assets/author-content-publish/initial-page-properties.png)

1. 대화 상자 팝업에서 **열기** 을 클릭하여 새로 만든 페이지를 엽니다.

## 구성 요소 작성 {#author-component}

AEM 구성 요소는 웹 페이지의 작은 모듈식 빌딩 블록으로 생각할 수 있습니다. UI를 논리적 청크 또는 구성 요소로 분류하면 관리가 훨씬 쉬워집니다. 구성 요소를 다시 사용하려면 구성 요소를 구성할 수 있어야 합니다. 이 작업은 작성자 대화 상자를 통해 수행됩니다.

AEM은 다음 집합 제공 [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko) 사용할 준비가 된 프로덕션 상태입니다. 다음 **핵심 구성 요소** 과 같은 기본 요소의 범위 [텍스트](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 및 [이미지](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 와 같은 보다 복잡한 UI 요소 [회전판](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

그런 다음 AEM 페이지 편집기를 사용하여 몇 가지 구성 요소를 작성합니다.

1. 다음 위치로 이동 **헬로 월드** 이전 연습에서 페이지가 생성되었습니다.
1. 다음 위치에 있는지 확인합니다. **편집** 모드 를 클릭하고 왼쪽 레일에서 **구성 요소** 아이콘.

   ![페이지 편집기 사이드 레일](assets/author-content-publish/page-editor-siderail.png)

   이렇게 하면 구성 요소 라이브러리가 열리고 페이지에서 사용할 수 있는 구성 요소가 나열됩니다.

1. 아래로 스크롤하고 **드래그 앤 드롭** a **텍스트(v2)** 구성 요소를 페이지의 기본 편집 가능 영역에 추가합니다.

   ![텍스트 구성 요소 드래그 앤 드롭](assets/author-content-publish/drag-drop-text-cmp.png)

1. 다음을 클릭합니다. **텍스트** 강조 표시할 구성 요소를 선택한 다음 **렌치** 아이콘 ![렌치 아이콘](assets/author-content-publish/wrench-icon.png) 구성 요소의 대화 상자를 엽니다. 텍스트를 입력하고 대화 상자에 변경 사항을 저장합니다.

   ![리치 텍스트 구성 요소](assets/author-content-publish/rich-text-populated-component.png)

   다음 **텍스트** 이제 구성 요소에 페이지에 서식 있는 텍스트가 표시됩니다.

1. 위의 단계를 반복합니다. 단, 인스턴스의 **이미지(v2)** 구성 요소를 페이지에 추가합니다. 를 엽니다. **이미지** 구성 요소의 대화 상자

1. 왼쪽 레일에서 을(를) 로 전환합니다. **자산 파인더** 을(를) 클릭하여 **에셋** 아이콘 ![에셋 아이콘](assets/author-content-publish/asset-icon.png).
1. **드래그 앤 드롭** 이미지를 구성 요소의 대화 상자에 입력하고 **완료** 변경 내용을 저장합니다.

   ![대화 상자에 에셋 추가](assets/author-content-publish/add-asset-dialog.png)

1. 다음과 같은 구성 요소가 페이지에 있는지 확인합니다. **제목**, **탐색**, **검색** 수정되었습니다. 이러한 영역은 페이지 템플릿의 일부로 구성되며 개별 페이지에서 수정할 수 없습니다. 이 내용은 다음 장에서 더 자세히 알아봅니다.

다른 구성 요소 중 일부를 자유롭게 실험해 보십시오. 각각에 대한 설명서 [핵심 구성 요소는 여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko). 에 대한 자세한 비디오 시리즈 [페이지 작성은 여기에서 확인할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 업데이트 게시 {#publish-updates}

AEM 환경은 **Author 서비스** 및 a **서비스 게시**. 이 장에서는 의 사이트를 몇 가지 수정했습니다. **Author 서비스**. 사이트 방문자가 변경 사항을 볼 수 있으려면에 게시해야 합니다. **서비스 게시**.

![높은 수준 다이어그램](assets/author-content-publish/author-publish-high-level-flow.png)

*작성자에서 게시로의 높은 수준의 콘텐츠 흐름*

**1.** 콘텐츠 작성자는 사이트 콘텐츠를 업데이트합니다. 업데이트는 미리보기, 검토 및 승인하여 실시간으로 푸시할 수 있습니다.

**2.** 콘텐츠가 게시되었습니다. 게시는 온디맨드로 수행하거나 미래 날짜로 예약할 수 있습니다.

**3.** 사이트 방문자는 Publish 서비스에 반영된 변경 사항을 보게 됩니다.

### 변경 사항 게시

다음으로 변경 사항을 게시하겠습니다.

1. AEM 시작 화면에서 다음으로 이동합니다. **사이트** 및 선택 **WKND 사이트**.
1. 다음을 클릭합니다. **게시 관리** 메뉴 표시줄에서 을 클릭합니다.

   ![게시 관리](assets/author-content-publish/click-manage-publiciation.png)

   완전히 새로운 사이트이므로 모든 페이지를 게시하려고 하며 게시 관리 마법사를 사용하여 게시해야 하는 항목을 정확하게 정의할 수 있습니다.

1. 아래 **옵션** 기본 설정을 다음으로 유지 **게시** 다음에 대한 일정을 예약하십시오. **지금**. **다음**&#x200B;을 클릭합니다.
1. 아래 **범위**&#x200B;를 선택하고 **WKND 사이트** 및 클릭 **하위 설정 포함**. 대화 상자에서 다음을 확인합니다 **하위 항목 포함**. 전체 사이트가 게시되도록 나머지 상자의 선택을 취소합니다.

   ![게시 범위 업데이트](assets/author-content-publish/update-scope-publish.png)

1. 다음을 클릭합니다. **게시된 참조** 단추를 클릭합니다. 대화 상자에서 모든 항목이 선택되어 있는지 확인합니다. 여기에는 다음이 포함됩니다. **표준 사이트 템플릿** 및 사이트 템플릿에서 생성한 여러 구성. 클릭 **완료** 업데이트하기.

   ![참조 게시](assets/author-content-publish/publish-references.png)

1. 마지막으로 옆에 있는 상자를 선택합니다. **WKND 사이트** 및 클릭 **다음** 오른쪽 상단에 있습니다.
1. 다음에서 **워크플로** 단계, 입력 **워크플로우 제목**. 모든 텍스트가 될 수 있으며 나중에 감사 추적의 일부로 유용할 수 있습니다. &quot;Initial publish&quot;를 입력하고 **게시**.

![워크플로우 단계 초기 게시](assets/author-content-publish/workflow-step-publish.png)

## 게시된 콘텐츠 보기 {#publish}

그런 다음 Publish 서비스로 이동하여 변경 사항을 확인합니다.

1. Publish 서비스의 URL을 가져오는 쉬운 방법은 작성자 URL을 복사하고 `author` 단어 포함 `publish`. 예:

   * **작성자 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **게시 URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 추가 `/content/wknd.html` 최종 URL이 다음과 같이 표시되도록 게시 URL에 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 변경 `wknd.html` 를 사용하여 사이트 이름과 일치시키십시오. [사이트 생성](create-site.md).

1. 게시 URL로 이동하면 AEM 작성 기능 없이 사이트가 표시됩니다.

   ![게시된 사이트](assets/author-content-publish/publish-url-update.png)

1. 사용 **탐색** 메뉴 클릭 **기사** > **헬로 월드** 을 클릭하여 이전에 만든 Hello World 페이지로 이동합니다.
1. (으)로 돌아가기 **AEM 작성자 서비스** 페이지 편집기에서 컨텐츠를 몇 가지 더 변경할 수 있습니다.
1. 다음 아이콘을 클릭하여 페이지 편집기 내에서 직접 이러한 변경 사항을 게시합니다 **페이지 속성** 아이콘 > **페이지 게시**

   ![직접 게시](assets/author-content-publish/page-editor-publish.png)

1. (으)로 돌아가기 **AEM 게시 서비스** 변경 사항을 보려면 다음을 수행하십시오. 다음과 같을 가능성이 높음 **아님** 즉시 업데이트를 확인합니다. 이유는 다음과 같습니다. **AEM 게시 서비스** 포함 [Apache 웹 서버 및 CDN을 통한 캐싱](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). 기본적으로 HTML 콘텐츠는 ~5분 동안 캐시됩니다.

1. 테스트/디버깅 목적으로 캐시를 무시하려면 다음과 같은 쿼리 매개 변수를 추가하기만 하면 됩니다. `?nocache=true`. URL은 다음과 같이 표시됩니다 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 사용 가능한 캐싱 전략 및 구성에 대한 자세한 내용 [은(는) 여기에서 찾을 수 있음](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Cloud Manager에서 Publish 서비스 URL을 찾을 수도 있습니다. 다음 위치로 이동 **Cloud Manager 프로그램** > **환경** > **환경**.

   ![게시 서비스 보기](assets/author-content-publish/view-environment-segments.png)

   아래 **환경 세그먼트** 다음 링크를 찾을 수 있습니다. **작성자** 및 **게시** 서비스.

## 축하합니다! {#congratulations}

축하합니다. AEM 사이트에 대한 변경 사항을 방금 작성 및 게시했습니다!

### 다음 단계 {#next-steps}

실제 구현에서는 일반적으로 사이트 생성 전에 mockup 및 UI 디자인이 있는 사이트를 계획합니다. Adobe XD UI Kit를 사용하여 Adobe Experience Manager Sites 구현을 디자인하고 가속화하는 방법에 대해 알아봅니다 [Adobe XD을 통한 UI 계획](./ui-planning-adobe-xd.md).

AEM Sites 기능을 계속 살펴보시겠습니까? 언제든지 의 챕터로 이동합니다. [페이지 템플릿](./page-templates.md) 페이지 템플릿과 페이지 간의 관계를 이해할 수 있습니다.


