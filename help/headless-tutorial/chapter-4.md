---
title: 4장 - 컨텐츠 서비스 템플릿 정의
seo-title: AEM 헤드리스 시작하기 - 4장 - 컨텐츠 서비스 템플릿 정의
description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 궁극적으로 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.
seo-description: AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 AEM Content Services가 궁극적으로 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%

---


# 4장 - 컨텐츠 서비스 템플릿 정의

AEM Headless 자습서의 4장에서는 AEM Content Services 컨텍스트에서 AEM 편집 가능 템플릿의 역할을 다룹니다. 편집 가능한 템플릿은 컨텐츠 서비스가 활성화된 AEM 구성 요소의 구성을 통해 클라이언트에 노출되는 JSON 컨텐츠 구조를 정의하는 데 사용됩니다.

## AEM 컨텐츠 서비스에서 템플릿의 역할 이해

AEM 편집 가능 템플릿은 이벤트 컨텐츠를 JSON으로 노출하기 위해 액세스할 HTTP 끝점을 정의하는 데 사용됩니다.

일반적으로 AEM 편집 가능한 템플릿은 웹 페이지를 정의하는 데 사용되지만, 이는 단순히 규칙입니다. 편집 가능한 템플릿은 **모든** 컨텐츠 세트를 구성하는 데 사용할 수 있습니다.컨텐츠의 액세스 방법:브라우저에서 HTML로, JavaScript(AEM SPA Editor) 또는 모바일 앱에서 JSON이 사용되므로 페이지가 요청되는 방식에 따라 달라집니다.

AEM Content Services에서 편집 가능한 템플릿은 JSON 데이터가 표시되는 방식을 정의하는 데 사용됩니다.

앱의 경우 [!DNL WKND Mobile] 단일 API 종단점을 구동하는 데 사용할 단일 편집 가능 템플릿을 만듭니다. 이 예제는 AEM 헤드리스의 개념을 간단히 설명하지만, 서로 다른 컨텐츠 세트를 노출하는 여러 페이지(또는 엔드포인트)를 만들어 보다 복잡하고 체계적으로 구성된 API를 만들 수 있습니다.

## API 종단점 이해

API 종단점을 구성하는 방법을 이해하고 앱에 노출되어야 하는 콘텐츠를 이해하려면 디자인을 다시 [!DNL WKND Mobile] 방문하겠습니다.

![이벤트 API 페이지 분해](./assets/chapter-4/design-to-component-mapping.png)

보시는 바와 같이 모바일 앱에 제공할 3개의 논리적 컨텐츠 세트가 있습니다.

1. The **Logo**
2. 태그 **라인**
3. 이벤트 **목록**

이를 위해, 이러한 요구 사항을 AEM 구성 요소(및 당사의 경우 AEM WCM 핵심 구성 요소)에 매핑하여 필요한 컨텐츠를 JSON으로 제공할 수 있습니다.

1. 로고 **가** 이미지 구성 요소를 통해 **표시됩니다.**
2. 태그 **줄** 이 **텍스트 구성 요소를 통해 표시됩니다.**
3. 이벤트 목록 **은** 컨텐츠 조각 목록 구성 요소를 **** 통해 표시되며, 이 구성 요소는 이벤트 컨텐츠 조각 집합을 참조합니다.

>[!NOTE]
>
>AEM Content Service의 페이지 및 구성 요소 JSON 내보내기를 지원하기 위해 페이지 및 구성 요소는 AEM WCM 핵심 구성 요소에서 **파생되어야 합니다**.
>
>[AEM WCM 핵심 구성](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 요소에는 내보낸 페이지 및 구성 요소의 표준화된 JSON 스키마를 지원하는 내장 기능이 있습니다. 이 자습서(페이지, 이미지, 텍스트 및 컨텐츠 조각 목록)에 사용된 모든 WKND 모바일 구성 요소는 AEM WCM 핵심 구성 요소에서 파생됩니다.

## 이벤트 API 템플릿 정의

1. 도구 **[!UICONTROL >]일반[!UICONTROL >]템플릿[!DNL WKND Mobile]**>으로이동합니다.

1. 템플릿을 **[!DNL Events API]** 만듭니다.

   1. 상단 **[!UICONTROL 작업]** 표시줄에서 만들기를 누릅니다.
   1. 템플릿 **[!DNL WKND Mobile - Empty Page]** 선택
   1. 상단 **[!UICONTROL 작업]** 표시줄에서 다음을 누릅니다.
   1. 템플릿 제목 **[!DNL Events API]** 필드  입력
   1. 상단 **[!UICONTROL 작업]** 표시줄에서 만들기를 누릅니다.
   1. 열기를 **[!UICONTROL 탭하여]** 편집할 새 템플릿 열기

1. 먼저 루트 레이아웃 컨테이너의 [!UICONTROL 정책을 편집하여 컨텐츠를 모델링해야 하는 세 개의 식별된 AEM 구성 요소] 를 [!UICONTROL 허용합니다]. 구조 **** 모드가 활성화되어 있는지 확인하고 **[!DNL Layout Container \[Root\]]**&#x200B;을 선택한 다음 **[!UICONTROL 정책]** 단추를누릅니다.
1. 속성 **>[!UICONTROL 허용된 구성 요소]** 에서 **[!DNL WKND Mobile]**&#x200B;검색할 수있습니다. 구성 요소 그룹에서 다음 구성 요소를 [!DNL WKND Mobile] 허용하여 [!DNL Events] API 페이지에서 사용할 수 있습니다.

   * **[!DNL WKND Mobile > Image]**

      * 앱용 로고
   * **[!DNL WKND Mobile > Text]**

      * 앱의 소개 텍스트
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 앱에 표시할 수 있는 이벤트 카테고리 목록



1. 완료되면 오른쪽 **[!UICONTROL 위]** 모서리의 완료 확인란을 누릅니다.
1. **왼쪽 레일에 새로** 허용된 구성 요소  목록을 보려면 브라우저 창을 새로 고칩니다.
1. 왼쪽 레일의 구성 요소 파인더에서 다음 AEM 구성 요소를 드래그합니다.
   1. **[!DNL Image]** for the logo
   2. **[!DNL Text]** for the Tag Line
   3. **[!DNL Content Fragment List]** for events
1. **위의 각 구성 요소에 대해**&#x200B;해당 구성 요소를 선택하고 잠금 **해제** 단추를 누릅니다.
1. 그러나 다른 구성 요소가 **추가되지** 않도록 하려면 **레이아웃 컨테이너** 가잠겨있는지, 이러한 세 구성 요소가 제거되지 않도록 하십시오.
1. 템플릿 목록 **[!UICONTROL 으로]돌아가려면[!UICONTROL 페이지 정보]** > [!DNL WKND Mobile] 관리의보기를누릅니다. 새로 만든 **[!DNL Events API]** 템플릿을 선택하고 상단 작업 **[!UICONTROL 막대에서]** 활성화를 누릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐트를 표시하는 데 사용되는 구성 요소가 템플릿 자체에 추가되고 잠겼습니다. 이는 작성자가 미리 정의된 구성 요소를 편집할 수 있지만, API 자체를 변경하면 JSON 구조에 대한 가정을 깨고 앱 사용을 중단할 수 있으므로 임의로 구성 요소를 추가하거나 제거할 수는 없습니다. 모든 API는 안정되어야 합니다.

## 다음 단계

원할 경우, [AEM Package Manager를 통해 AEM Author에](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip [컨텐츠 패키지를 설치합니다](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이전 장과 이 장에 나와 있는 구성 및 컨텐츠가 포함되어 있습니다.

* [5장 - 컨텐츠 서비스 페이지 작성](./chapter-5.md)
