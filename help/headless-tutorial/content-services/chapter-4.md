---
title: 4장 - 컨텐츠 서비스 템플릿 정의 - 컨텐츠 서비스
seo-title: AEM Headless 시작하기 - 4장 - 컨텐츠 서비스 템플릿 정의
description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services에서 최종적으로 노출할 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.
seo-description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services에서 최종적으로 노출할 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# 4장 - 컨텐츠 서비스 템플릿 정의

AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 Content Services가 활성화된 AEM 구성 요소의 구성을 통해 고객에게 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.

## AEM Content Services에서 템플릿의 역할 이해

AEM 편집 가능한 템플릿은 이벤트 컨텐츠를 JSON으로 표시하기 위해 액세스할 HTTP 엔드포인트를 정의하는 데 사용됩니다.

일반적으로 AEM 편집 가능한 템플릿은 웹 페이지를 정의하는 데 사용되지만 단순히 규칙을 사용합니다. 편집 가능한 템플릿은 **임의의** 컨텐츠 집합을 작성하는 데 사용할 수 있습니다.컨텐츠 액세스 방법:를 브라우저에서 HTML로, JavaScript(AEM SPA Editor) 또는 모바일 앱에서 JSON이 사용되면 페이지가 요청되는 방식의 함수입니다.

AEM Content Services에서 편집 가능한 템플릿은 JSON 데이터가 표시되는 방식을 정의하는 데 사용됩니다.

[!DNL WKND Mobile] 앱의 경우 단일 API 엔드포인트를 구동하는 데 사용할 단일 편집 가능한 템플릿을 만듭니다. 이 예는 AEM Headless의 개념을 설명하는 간단한 방법이지만, 서로 다른 컨텐츠 세트를 노출하는 각 페이지(또는 엔드포인트)를 여러 개 만들어 보다 복잡하고 더 잘 구성된 API를 만들 수 있습니다.

## API 엔드포인트 이해

API 엔드포인트를 작성하는 방법을 이해하고 [!DNL WKND Mobile] 앱에 어떤 콘텐츠가 노출되어야 하는지 이해하려면 디자인을 다시 방문하겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

알 수 있듯이 모바일 앱에 제공할 세 개의 논리 컨텐츠 세트가 있습니다.

1. **로고**
2. **태그 줄**
3. **Events** 목록

이렇게 하려면 이러한 요구 사항을 AEM 구성 요소(및 이 경우 AEM WCM 코어 구성 요소)에 매핑하여 필수 컨텐츠를 JSON으로 노출할 수 있습니다.

1. **로고**&#x200B;는 **이미지 구성 요소**&#x200B;를 통해 표시됩니다
2. **태그 라인**&#x200B;은 **텍스트 구성 요소**&#x200B;를 통해 표시됩니다
3. **Events** 목록은 **컨텐츠 조각 목록 구성 요소**&#x200B;를 통해 표시되며, 이 구성 요소는 이벤트 컨텐츠 조각 세트를 참조합니다.

>[!NOTE]
>
>AEM 컨텐츠 서비스의 페이지 및 구성 요소 JSON 내보내기를 지원하려면 페이지 및 구성 요소 가 AEM WCM 코어 구성 요소&#x200B;**에서**&#x200B;파생되어야 합니다.
>
>[내보낸 페이지 ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 및 구성 요소의 정규화된 JSON 스키마를 지원하기 위해 AEM WCM Core Components 내장 기능입니다. 이 자습서(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)에 사용된 모든 WKND 모바일 구성 요소는 AEM WCM 코어 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**&#x200B;으로 이동합니다.

1. **[!DNL Events API]** 템플릿을 만듭니다.

   1. 맨 위 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 누릅니다
   1. **[!DNL WKND Mobile - Empty Page]** 템플릿을 선택합니다
   1. 맨 위 작업 표시줄에서 **[!UICONTROL Next]**&#x200B;를 누릅니다
   1. [!UICONTROL 템플릿 제목] 필드에 **[!DNL Events API]**&#x200B;을 입력합니다
   1. 맨 위 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 누릅니다
   1. **[!UICONTROL 열기]**&#x200B;를 탭하여 편집할 새 템플릿을 엽니다

1. 먼저, Root [!UICONTROL 레이아웃 컨테이너]의 [!UICONTROL Policy]를 편집하여 컨텐츠를 모델링해야 하는 3개의 식별된 AEM 구성 요소를 허용합니다. **[!UICONTROL 구조]** 모드가 활성화되어 있는지 확인하고 **[!DNL Layout Container \[Root\]]**&#x200B;를 선택하고 **[!UICONTROL 정책]** 단추를 누릅니다.
1. **[!UICONTROL 속성] > [!UICONTROL 허용된 구성 요소]**&#x200B;에서 **[!DNL WKND Mobile]**&#x200B;를 검색합니다. [!DNL Events] API 페이지에서 사용할 수 있도록 [!DNL WKND Mobile] 구성 요소 그룹에서 다음 구성 요소를 허용합니다.

   * **[!DNL WKND Mobile > Image]**

      * 앱의 로고
   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 카테고리 목록



1. 완료되면 오른쪽 위 모서리에서 **[!UICONTROL 완료]** 확인 표시를 누릅니다.
1. **** 브라우저 창을 새로 고쳐  [!UICONTROL 왼쪽 ] 레일에서 새로 허용된 구성 요소 목록을 확인합니다.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소로 드래그합니다.
   1. **[!DNL Image]** 로고
   2. **[!DNL Text]** 태그 라인
   3. **[!DNL Content Fragment List]** 이벤트
1. **위의 각 구성 요소에 대해** 해당 구성 요소를 선택하고 잠금 해제  **** 단추를 누릅니다.
1. 그러나 다른 구성 요소가 추가되지 않도록 하려면 **레이아웃 컨테이너**&#x200B;가 **잠긴**&#x200B;인지 확인하거나 이 세 구성 요소가 제거되지 않도록 하십시오.
1. **[!UICONTROL 페이지 정보] > [!UICONTROL Admin]**&#x200B;에서 보기를 탭하여 [!DNL WKND Mobile] 템플릿 목록으로 돌아갑니다. 새로 만든 **[!DNL Events API]** 템플릿을 선택하고 맨 위 작업 표시줄에서 **[!UICONTROL 활성화]**&#x200B;를 누릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐츠를 표시하는 데 사용되는 구성 요소가 템플릿 자체에 추가되고 잠깁니다. API 자체를 변경하면 JSON 구조에 대한 가정을 손상시키고 앱 사용을 중단시킬 수 있으므로 작성자가 미리 정의된 구성 요소를 편집할 수 있지만 임의로 구성 요소를 추가 또는 제거할 수는 없습니다. 모든 API는 안정적이어야 합니다.

## 다음 단계

원할 경우 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM Author에 설치합니다. 이 패키지에는 자습서의 이전 장과 이 장에 설명된 구성 및 컨텐츠가 들어 있습니다.

* [5장 - 컨텐츠 서비스 페이지 작성](./chapter-5.md)
