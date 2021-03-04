---
title: 4장 - 컨텐츠 서비스 템플릿 정의 - 컨텐츠 서비스
seo-title: AEM 헤드리스 시작 - 4장 - 컨텐츠 서비스 템플릿 정의
description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 JSON 컨텐츠 구조를 정의하는 데 사용됩니다. AEM 컨텐츠 서비스는 궁극적으로 노출됩니다.
seo-description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 JSON 컨텐츠 구조를 정의하는 데 사용됩니다. AEM 컨텐츠 서비스는 궁극적으로 노출됩니다.
feature: '"컨텐츠 조각, API"'
topic: '"헤드리스, 콘텐츠 관리"'
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '845'
ht-degree: 0%

---


# 4장 - 컨텐츠 서비스 템플릿 정의

AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 Content Services가 활성화된 AEM 구성 요소의 구성을 통해 JSON 컨텐츠 구조 AEM Content Services가 고객에게 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.

## AEM 컨텐츠 서비스에서 템플릿의 역할 이해

AEM 편집 가능 템플릿은 이벤트 컨텐츠를 JSON으로 노출하기 위해 액세스할 HTTP 끝점을 정의하는 데 사용됩니다.

일반적으로 AEM 편집 가능 템플릿은 웹 페이지를 정의하는 데 사용되지만, 이는 단순히 규칙입니다. 편집 가능한 템플릿은 **모든** 컨텐츠 세트를 작성하는 데 사용할 수 있습니다.컨텐트에 액세스하는 방법:브라우저에서 HTML로, JavaScript(AEM Editor) 또는 모바일 앱이 사용하는 JSON은 페이지가 요청되는 방식에 대한 함수입니다.

AEM Content Services에서 편집 가능한 템플릿은 JSON 데이터가 표시되는 방식을 정의하는 데 사용됩니다.

[!DNL WKND Mobile] 앱의 경우 단일 API 끝점을 실행하는 데 사용할 단일 편집 가능 템플릿을 만듭니다. 이 예제는 AEM 헤드리스의 개념을 쉽게 설명하지만 서로 다른 컨텐츠 세트를 표시하는 여러 페이지(또는 끝점)를 만들어 보다 복잡하고 체계적으로 구성된 API를 만들 수 있습니다.

## API 끝점 이해

API 끝점을 구성하는 방법을 이해하고 [!DNL WKND Mobile] 앱에 어떤 콘텐츠가 노출되어야 하는지 이해하려면 디자인을 다시 방문하겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

보시다시피 모바일 앱에 제공할 3개의 논리 컨텐츠 세트가 있습니다.

1. **로고**
2. **태그 줄**
3. **이벤트** 목록

이 작업을 수행하려면 이러한 요구 사항을 AEM 구성 요소(그리고 AEM WCM 핵심 구성 요소)에 매핑하여 필요한 컨텐츠를 JSON으로 표시할 수 있습니다.

1. **로고**&#x200B;는 **이미지 구성 요소**&#x200B;를 통해 표시됩니다.
2. **태그 줄**&#x200B;이 **텍스트 구성 요소**&#x200B;를 통해 표시됩니다.
3. **이벤트** 목록은 이벤트 컨텐츠 조각 집합을 참조하는 **컨텐츠 조각 목록 구성 요소**&#x200B;를 통해 표시됩니다.

>[!NOTE]
>
>AEM Content Service의 페이지 및 구성 요소 JSON 내보내기를 지원하려면 페이지 및 구성 요소가 AEM WCM 핵심 구성 요소&#x200B;**에서**&#x200B;파생되어야 합니다.
>
>[AEM WCM 핵심 ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 구성 요소는 내보낸 페이지 및 구성 요소의 표준화된 JSON 스키마를 지원하기 위해 내장된 기능을 제공합니다. 이 자습서(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)에 사용된 모든 WKND 모바일 구성 요소는 AEM WCM 핵심 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**&#x200B;으로 이동합니다.

1. **[!DNL Events API]** 템플릿을 만듭니다.

   1. 위쪽 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 탭합니다.
   1. **[!DNL WKND Mobile - Empty Page]** 템플릿 선택
   1. 위쪽 작업 표시줄에서 **[!UICONTROL 다음]**&#x200B;을 탭합니다.
   1. [!UICONTROL 템플릿 제목] 필드에 **[!DNL Events API]**&#x200B;을 입력합니다.
   1. 위쪽 작업 표시줄에서 **[!UICONTROL 만들기]**&#x200B;를 탭합니다.
   1. **[!UICONTROL 열기]**&#x200B;를 눌러 편집할 새 템플릿을 엽니다.

1. 먼저 루트 [!UICONTROL 레이아웃 컨테이너]의 [!UICONTROL 정책]을 편집하여 컨텐츠를 모델링해야 하는 3개의 식별된 AEM 구성 요소를 허용합니다. **[!UICONTROL 구조]** 모드가 활성화되어 있는지 확인하고 **[!DNL Layout Container \[Root\]]**&#x200B;을 선택하고 **[!UICONTROL 정책]** 단추를 누릅니다.
1. **[!UICONTROL 속성] > [!UICONTROL 허용된 구성 요소]**&#x200B;에서 **[!DNL WKND Mobile]**&#x200B;를 검색합니다. [!DNL Events] API 페이지에서 사용할 수 있도록 [!DNL WKND Mobile] 구성 요소 그룹의 다음 구성 요소를 허용합니다.

   * **[!DNL WKND Mobile > Image]**

      * 앱용 로고
   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 카테고리 목록



1. 완료되면 오른쪽 위 모서리에서 **[!UICONTROL 완료]** 체크 표시를 누릅니다.
1. **왼쪽** 레일에 새로  [!UICONTROL 허용된 ] 구성 요소 목록을 보려면 브라우저 창을 새로 고칩니다.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소를 드래그합니다.
   1. **[!DNL Image]** 로고
   2. **[!DNL Text]** 를 선택합니다.
   3. **[!DNL Content Fragment List]** 이벤트
1. **위의 각 구성 요소에 대해** 해당 구성 요소를 선택하고 잠금 해제 버튼을  **** 누릅니다.
1. 그러나 다른 구성 요소가 추가되지 않도록 하려면 **레이아웃 컨테이너**&#x200B;이(가) **잠김**&#x200B;인지 확인하십시오. 또는 이 3개의 구성 요소가 제거되지 않습니다.
1. [!DNL WKND Mobile] 템플릿 목록으로 돌아가려면 **[!UICONTROL 페이지 정보] > [!UICONTROL 관리의 보기]**&#x200B;를 누릅니다. 새로 만든 **[!DNL Events API]** 템플릿을 선택하고 맨 위 작업 표시줄에서 **[!UICONTROL Enable]**&#x200B;을 탭합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐트를 표면화하는 데 사용되는 구성 요소가 템플릿 자체에 추가되고 잠깁니다. 이는 작성자가 미리 정의된 구성 요소를 편집할 수 있지만, API 자체를 변경하면 JSON 구조 주위의 가정을 깨고 앱 사용을 중단할 수 있으므로 구성 요소를 임의로 추가 또는 제거할 수는 없습니다. 모든 API는 안정적일 필요가 있습니다.

## 다음 단계

원할 경우, [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM Author에 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 설치하십시오. 이 패키지에는 자습서의 이전 장과 본 장에 나와 있는 구성 및 컨텐츠가 들어 있습니다.

* [5장 - 컨텐츠 서비스 페이지 작성](./chapter-5.md)
