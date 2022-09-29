---
title: 4장 - 컨텐츠 서비스 템플릿 정의 - 컨텐츠 서비스
description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services에서 최종적으로 노출하는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 4장 - 컨텐츠 서비스 템플릿 정의

AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 Content Services가 활성화된 AEM 구성 요소의 구성을 통해 고객에게 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.

## AEM Content Services에서 템플릿의 역할 이해

AEM 편집 가능한 템플릿은 이벤트 컨텐츠를 JSON으로 표시하기 위해 액세스하는 HTTP 종료 지점을 정의하는 데 사용됩니다.

일반적으로 AEM 편집 가능한 템플릿은 웹 페이지를 정의하는 데 사용되지만 단순히 규칙을 사용합니다. 편집 가능한 템플릿은 작성하는 데 사용할 수 있습니다 **임의** 컨텐츠 세트; 컨텐츠 액세스 방법: JavaScript(AEM SPA Editor) 또는 모바일 앱에서 JSON이 브라우저의 HTML으로 사용되므로 페이지가 요청되는 방식의 함수입니다.

AEM Content Services에서 편집 가능한 템플릿은 JSON 데이터가 표시되는 방식을 정의하는 데 사용됩니다.

대상 [!DNL WKND Mobile] 앱에서는 단일 API 엔드포인트를 구동하는 데 사용되는 단일 편집 가능한 템플릿을 만듭니다. 이 예는 AEM Headless의 개념을 설명하는 간단한 방법이지만, 서로 다른 컨텐츠 세트를 노출하는 각 페이지(또는 엔드포인트)를 여러 개 만들어 보다 복잡하고 더 잘 구성된 API를 만들 수 있습니다.

## API 엔드포인트 이해

API 종단점을 구성하는 방법을 이해하고 API에 어떤 콘텐츠를 노출해야 하는지 이해하기 위해 [!DNL WKND Mobile] 앱, 디자인을 다시 살펴보겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

알 수 있듯이 모바일 앱에 제공할 세 개의 논리 컨텐츠 세트가 있습니다.

1. 다음 **로고**
2. 다음 **태그 라인**
3. 목록 **이벤트**

이렇게 하려면 이러한 요구 사항을 AEM 구성 요소(및 이 경우 AEM WCM 코어 구성 요소)에 매핑하여 필수 컨텐츠를 JSON으로 노출할 수 있습니다.

1. 다음 **로고** 는 **이미지 구성 요소**
2. 다음 **태그 라인** 는 **텍스트 구성 요소**
3. 목록 **이벤트** 는 **컨텐츠 조각 목록 구성 요소** 그러면 은 이벤트 컨텐츠 조각 세트를 참조합니다.

>[!NOTE]
>
>AEM 컨텐츠 서비스의 페이지 및 구성 요소 JSON 내보내기를 지원하려면 페이지 및 구성 요소 가 있어야 합니다 **AEM WCM 핵심 구성 요소에서 파생됨**.
>
>[AEM WCM 코어 구성 요소](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 내보낸 페이지 및 구성 요소의 정규화된 JSON 스키마를 지원하는 내장 기능이 있습니다. 이 자습서(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)에 사용된 모든 WKND 모바일 구성 요소는 AEM WCM 코어 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**.

1. 만들기 **[!DNL Events API]** 템플릿:

   1. 탭 **[!UICONTROL 만들기]** 상단 작업 모음에서
   1. 을(를) 선택합니다 **[!DNL WKND Mobile - Empty Page]** 템플릿
   1. 탭 **[!UICONTROL 다음]** 상단 작업 모음에서
   1. Enter 키 **[!DNL Events API]** 에서 [!UICONTROL 템플릿 제목] 필드
   1. 탭 **[!UICONTROL 만들기]** 상단 작업 모음에서
   1. 탭 **[!UICONTROL 열기]** 편집할 새 템플릿 열기

1. 먼저, 컨텐츠를 편집하여 컨텐츠를 모델링해야 하는 3개의 식별된 AEM 구성 요소를 허용합니다 [!UICONTROL 정책] 루트 [!UICONTROL 레이아웃 컨테이너]. 다음을 확인합니다. **[!UICONTROL 구조]** 모드가 활성 상태인 경우 **[!DNL Layout Container \[Root\]]**&#x200B;를 누르고 를 누릅니다 **[!UICONTROL 정책]** 버튼을 클릭합니다.
1. 아래 **[!UICONTROL 속성] > [!UICONTROL 허용된 구성 요소]** 검색 대상 **[!DNL WKND Mobile]**. 에서 다음 구성 요소를 허용합니다 [!DNL WKND Mobile] 구성 요소 그룹에서 사용 [!DNL Events] API 페이지.

   * **[!DNL WKND Mobile > Image]**

      * 앱의 로고
   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 카테고리 목록



1. 탭하기 **[!UICONTROL 완료]** 완료되면 오른쪽 위 모서리에서 을(를) 확인 표시합니다.
1. **새로 고침** 새로 보기 위한 브라우저 창 [!UICONTROL 허용된 구성 요소] 왼쪽 레일에 나열합니다.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소로 드래그합니다.
   1. **[!DNL Image]** 로고
   2. **[!DNL Text]** 태그 라인
   3. **[!DNL Content Fragment List]** 이벤트
1. **위의 각 구성 요소에 대해**&#x200B;를 선택하고 키를 누릅니다 **잠금 해제** 버튼을 클릭합니다.
1. 그러나 **레이아웃 컨테이너** is **잠김** 다른 구성 요소가 추가되지 않거나 이 세 구성 요소가 제거되지 않도록 합니다.
1. 탭 **[!UICONTROL 페이지 정보] > [!UICONTROL 관리자로 보기]** 로 돌아가기 [!DNL WKND Mobile] 템플릿 목록. 새로 만든 항목을 선택합니다 **[!DNL Events API]** 템플릿 및 탭 **[!UICONTROL 활성화]** 를 클릭합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐츠를 표시하는 데 사용되는 구성 요소가 템플릿 자체에 추가되고 잠깁니다. API 자체를 변경하면 JSON 구조에 대한 가정을 손상시키고 앱 사용을 중단시킬 수 있으므로 작성자가 미리 정의된 구성 요소를 편집할 수 있지만 임의로 구성 요소를 추가 또는 제거할 수는 없습니다. 모든 API는 안정적이어야 합니다.

## 다음 단계

선택적으로 를 설치합니다 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통해 AEM 작성자의 컨텐츠 패키지 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이전 장과 이 장에 설명된 구성 및 컨텐츠가 들어 있습니다.

* [5장 - 컨텐츠 서비스 페이지 작성](./chapter-5.md)
