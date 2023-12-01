---
title: 4장 - 콘텐츠 서비스 템플릿 정의 - 콘텐츠 서비스
description: AEM Headless 자습서 4장에서는 AEM Content Services의 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 최종적으로 노출하는 JSON 콘텐츠 구조를 정의하는 데 사용됩니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 4장 - Content Services 템플릿 정의

AEM Headless 자습서 4장에서는 AEM Content Services의 컨텍스트에서 AEM 편집 가능한 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 AEM 구성 요소를 통해 클라이언트에게 노출하는 JSON 콘텐츠 구조를 정의하는 데 사용됩니다.

## AEM Content Services에서 템플릿의 역할 이해

AEM 편집 가능한 템플릿은 이벤트 컨텐츠를 JSON으로 표시하기 위해 액세스하는 HTTP 끝점을 정의하는 데 사용됩니다.

일반적으로 AEM 편집 가능한 템플릿은 웹 페이지를 정의하는 데 사용되지만, 이 사용은 단순히 규칙입니다. 편집 가능한 템플릿을 사용하여 다음을 작성할 수 있습니다. **임의** 컨텐츠 세트; 컨텐츠 액세스 방법: 브라우저의 HTML으로, JavaScript(AEM SPA 편집기) 또는 모바일 앱에서 사용하는 JSON은 해당 페이지가 요청된 방식의 함수입니다.

AEM Content Services에서 편집 가능한 템플릿을 사용하여 JSON 데이터가 노출되는 방법을 정의합니다.

의 경우 [!DNL WKND Mobile] 앱에서는 단일 API 엔드포인트를 구동하는 데 사용되는 편집 가능한 단일 템플릿을 만듭니다. 이 예는 AEM Headless의 개념을 간단하게 설명할 수 있지만, 서로 다른 콘텐츠 세트를 노출하는 여러 페이지(또는 엔드포인트)를 만들어 보다 복잡하고 잘 구성된 API를 만들 수 있습니다.

## API 끝점 이해

API 엔드포인트를 구성하는 방법을 이해하고 당사에 노출되어야 하는 콘텐츠를 파악합니다 [!DNL WKND Mobile] 앱, 디자인을 다시 방문하겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

알 수 있듯이 모바일 앱에 제공할 논리적 콘텐츠 세트가 세 개 있습니다.

1. 다음 **로고**
2. 다음 **태그 라인**
3. 의 목록 **이벤트**

이를 위해 필요한 컨텐츠를 JSON으로 노출하기 위해 이러한 요구 사항을 AEM 구성 요소(및 이 경우 AEM WCM 핵심 구성 요소)에 매핑할 수 있습니다.

1. 다음 **로고** 은(는) 을 통해 표시됩니다. **이미지 구성 요소**
2. 다음 **태그 라인** 은(는) 을 통해 표시됩니다. **텍스트 구성 요소**
3. 의 목록 **이벤트** 은(는) 을 통해 표시됩니다. **콘텐츠 조각 목록 구성 요소** 결과적으로 는 이벤트 콘텐츠 조각 집합을 참조합니다.

>[!NOTE]
>
>페이지 및 구성 요소에 대한 AEM Content Service의 JSON 내보내기를 지원하려면 페이지 및 구성 요소를 다음과 같이 해야 합니다. **AEM WCM 코어 구성 요소에서 파생**.
>
>[AEM WCM 코어 구성 요소](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 에는 내보낸 페이지 및 구성 요소의 표준화된 JSON 스키마를 지원하는 기능이 내장되어 있습니다. 이 자습서에서 사용되는 모든 WKND Mobile 구성 요소(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)는 AEM WCM 핵심 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**.

1. 만들기 **[!DNL Events API]** 템플릿:

   1. 누르기 **[!UICONTROL 만들기]** 맨 위의 작업 표시줄에서
   1. 다음 항목 선택 **[!DNL WKND Mobile - Empty Page]** 템플릿
   1. 누르기 **[!UICONTROL 다음]** 맨 위의 작업 표시줄에서
   1. 입력 **[!DNL Events API]** 다음에서 [!UICONTROL 템플릿 제목] 필드
   1. 누르기 **[!UICONTROL 만들기]** 맨 위의 작업 표시줄에서
   1. 누르기 **[!UICONTROL 열기]** 편집할 새 템플릿 열기

1. 먼저, 를 편집하여 콘텐츠를 모델링하는 데 필요한 세 개의 식별된 AEM 구성 요소를 허용합니다. [!UICONTROL 정책] 루트 중 [!UICONTROL 레이아웃 컨테이너]. 다음을 확인합니다. **[!UICONTROL 구조]** 모드가 활성 상태입니다. **[!DNL Layout Container \[Root\]]**&#x200B;을 누르고 **[!UICONTROL 정책]** 단추를 클릭합니다.
1. 아래 **[!UICONTROL 속성] > [!UICONTROL 허용된 구성 요소]** 검색 대상 **[!DNL WKND Mobile]**. 에서 다음 구성 요소 허용 [!DNL WKND Mobile] 에서 사용할 수 있도록 구성 요소 그룹 [!DNL Events] API 페이지.

   * **[!DNL WKND Mobile > Image]**

      * 앱의 로고

   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트

   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 범주 목록

1. 탭 **[!UICONTROL 완료]** 완료되면 오른쪽 상단의 확인 표시를 합니다.
1. **새로 고침** 새로 볼 브라우저 창 [!UICONTROL 허용된 구성 요소] 왼쪽 레일에 나열합니다.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소를 드래그합니다.
   1. **[!DNL Image]** 로고용
   2. **[!DNL Text]** 태그 라인의 경우
   3. **[!DNL Content Fragment List]** 이벤트용
1. **위의 각 구성 요소에 대해**&#x200B;을(를) 선택하고 **잠금 해제** 단추를 클릭합니다.
1. 단, **레이아웃 컨테이너** 은(는) **잠김** 다른 구성 요소가 추가되거나 이 세 구성 요소가 제거되지 않도록 하려는 경우
1. 누르기 **[!UICONTROL 페이지 정보] > [!UICONTROL 관리자 화면에서 보기]** (으)로 돌아가기 [!DNL WKND Mobile] 템플릿 목록. 새로 만든 항목 선택 **[!DNL Events API]** 템플릿 및 탭 **[!UICONTROL 사용]** 맨 위의 작업 표시줄에 표시됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 콘텐츠를 표시하는 데 사용된 구성 요소가 템플릿 자체에 추가되고 잠깁니다. 이는 작성자가 사전 정의된 구성 요소를 편집할 수 있지만 API 자체를 변경하면 JSON 구조에 대한 가정이 깨지고 사용 중인 앱이 깨질 수 있으므로 구성 요소를 임의로 추가하거나 제거할 수 없기 때문입니다. 모든 API는 안정적이어야 합니다.

## 다음 단계

필요한 경우 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통한 AEM Author의 콘텐츠 패키지 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이 장과 이전 장에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [5장 - Content Services 페이지 작성](./chapter-5.md)
