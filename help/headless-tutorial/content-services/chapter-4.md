---
title: 4장 - 콘텐츠 서비스 템플릿 정의 - 콘텐츠 서비스
description: AEM Headless 자습서 4장에서는 AEM Content Services의 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 최종적으로 노출하는 JSON 콘텐츠 구조를 정의하는 데 사용됩니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 4장 - Content Services 템플릿 정의

AEM Headless 자습서 4장에서는 AEM Content Services의 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 AEM 구성 요소를 통해 클라이언트에게 노출하는 JSON 콘텐츠 구조를 정의하는 데 사용됩니다.

## AEM Content Services에서 템플릿의 역할 이해

AEM 편집 가능한 템플릿은 이벤트 컨텐츠를 JSON으로 표시하기 위해 액세스하는 HTTP 끝점을 정의하는 데 사용됩니다.

일반적으로 AEM의 편집 가능한 템플릿 을 사용하여 웹 페이지를 정의하지만 이 사용은 단순히 규칙입니다. 편집 가능한 템플릿은 **모든** 콘텐츠 세트를 작성하는 데 사용할 수 있습니다. 해당 콘텐츠에 액세스하는 방법: 브라우저의 HTML으로, JavaScript(AEM SPA 편집기) 또는 모바일 앱에서 사용하는 JSON은 해당 페이지 요청 방식의 함수입니다.

AEM Content Services에서 편집 가능한 템플릿을 사용하여 JSON 데이터가 노출되는 방법을 정의합니다.

[!DNL WKND Mobile] 앱의 경우 단일 API 끝점을 구동하는 데 사용되는 편집 가능한 단일 템플릿을 만듭니다. 이 예는 AEM Headless의 개념을 간단하게 설명할 수 있지만, 서로 다른 콘텐츠 세트를 노출하는 여러 페이지(또는 엔드포인트)를 만들어 보다 복잡하고 잘 구성된 API를 만들 수 있습니다.

## API 끝점 이해

API 끝점을 구성하는 방법을 이해하고 [!DNL WKND Mobile] 앱에 노출되어야 하는 콘텐츠를 이해하려면 디자인을 다시 방문하겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

알 수 있듯이 모바일 앱에 제공할 논리적 콘텐츠 세트가 세 개 있습니다.

1. **로고**
2. **태그 줄**
3. **이벤트** 목록

이를 위해 필요한 컨텐츠를 JSON으로 노출하기 위해 이러한 요구 사항을 AEM 구성 요소(및 이 경우 AEM WCM 핵심 구성 요소)에 매핑할 수 있습니다.

1. **로고**&#x200B;은(는) **이미지 구성 요소**&#x200B;를 통해 표시됩니다.
2. **태그 줄**&#x200B;은(는) **텍스트 구성 요소**&#x200B;를 통해 표시됩니다
3. **이벤트** 목록은 이벤트 콘텐츠 조각 집합을 참조하는 **콘텐츠 조각 목록 구성 요소**&#x200B;를 통해 표시됩니다.

>[!NOTE]
>
>페이지 및 구성 요소에 대한 AEM Content Service의 JSON 내보내기를 지원하려면 페이지 및 구성 요소가 **AEM WCM 핵심 구성 요소에서 파생되어야 합니다**.
>
>[AEM의 WCM 코어 구성 요소](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components)에는 내보낸 페이지 및 구성 요소의 정규화된 JSON 스키마를 지원하는 기본 기능이 있습니다. 이 자습서에서 사용되는 모든 WKND Mobile 구성 요소(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)는 AEM의 WCM 핵심 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**(으)로 이동합니다.

1. **[!DNL Events API]** 템플릿 만들기:

   1. 상단 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 탭합니다.
   1. **[!DNL WKND Mobile - Empty Page]** 템플릿 선택
   1. 상단 작업 표시줄에서 **[!UICONTROL 다음]**&#x200B;을 누릅니다.
   1. [!UICONTROL 템플릿 제목] 필드에 **[!DNL Events API]** 입력
   1. 상단 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 탭합니다.
   1. 편집할 새 템플릿을 열려면 **[!UICONTROL 열기]**&#x200B;를 탭합니다.

1. 먼저, 루트 [!UICONTROL 레이아웃 컨테이너]의 [!UICONTROL 정책]을(를) 편집하여 콘텐츠를 모델링하는 데 필요한 식별된 AEM 구성 요소 3개를 허용합니다. **[!UICONTROL 구조]** 모드가 활성화되어 있는지 확인하고 **[!DNL Layout Container \[Root\]]**&#x200B;을(를) 선택한 다음 **[!UICONTROL 정책]** 단추를 탭합니다.
1. **[!UICONTROL 속성] > [!UICONTROL 허용된 구성 요소]**&#x200B;에서 **[!DNL WKND Mobile]**&#x200B;을(를) 검색합니다. [!DNL Events] API 페이지에서 사용할 수 있도록 [!DNL WKND Mobile] 구성 요소 그룹의 다음 구성 요소를 허용합니다.

   * **[!DNL WKND Mobile > Image]**

      * 앱의 로고

   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트

   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 범주 목록

1. 완료되면 오른쪽 상단의 **[!UICONTROL 완료]** 확인 표시를 탭합니다.
1. 왼쪽 레일에서 새로 [!UICONTROL 허용된 구성 요소] 목록을 보려면 브라우저 창을 **새로 고침**&#x200B;하십시오.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소를 드래그합니다.
   1. 로고의 **[!DNL Image]**
   2. 태그 줄의 **[!DNL Text]**
   3. 이벤트의 **[!DNL Content Fragment List]**
1. **위의 각 구성 요소에 대해**&#x200B;을(를) 선택하고 **잠금 해제** 단추를 누르십시오.
1. 그러나 다른 구성 요소가 추가되거나 이 세 구성 요소가 제거되지 않도록 하려면 **레이아웃 컨테이너**&#x200B;이 **잠김**&#x200B;인지 확인하십시오.
1. **[!UICONTROL 페이지 정보] > [!UICONTROL 관리자로 보기]**&#x200B;를 눌러 [!DNL WKND Mobile] 템플릿 목록으로 돌아갑니다. 새로 만든 **[!DNL Events API]** 템플릿을 선택하고 맨 위의 작업 표시줄에서 **[!UICONTROL 사용]**&#x200B;을 탭합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 콘텐츠를 표시하는 데 사용된 구성 요소가 템플릿 자체에 추가되고 잠깁니다. 이는 작성자가 사전 정의된 구성 요소를 편집할 수 있지만 API 자체를 변경하면 JSON 구조에 대한 가정이 깨지고 사용 중인 앱이 깨질 수 있으므로 구성 요소를 임의로 추가하거나 제거할 수 없기 때문입니다. 모든 API는 안정적이어야 합니다.

## 다음 단계

필요한 경우 [AEM의 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM 작성자에 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 콘텐츠 패키지를 설치하십시오. 이 패키지에는 자습서의 이 장과 이전 장에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [5장 - Content Services 페이지 작성](./chapter-5.md)
