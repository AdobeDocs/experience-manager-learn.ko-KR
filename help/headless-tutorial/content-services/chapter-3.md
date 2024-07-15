---
title: 3장 - 이벤트 콘텐츠 조각 작성 - 콘텐츠 서비스
description: AEM Headless 자습서 3장에서는 2장에서 만든 콘텐츠 조각 모델에서 이벤트 콘텐츠 조각을 만들고 작성하는 방법에 대해 설명합니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 3장 - 이벤트 콘텐츠 조각 작성

AEM Headless 자습서의 3장에서는 [Chapter 2](./chapter-2.md)에서 만든 콘텐츠 조각 모델에서 이벤트 콘텐츠 조각을 만들고 작성하는 방법에 대해 설명합니다.

## 이벤트 컨텐츠 조각 작성

[!DNL Event] 콘텐츠 조각 모델을 만들고 WKND에 대한 AEM 구성을 `/content/dam/wknd-mobile` 에셋 폴더(`cq:conf` 속성을 통해)에 적용하면 [!DNL Event] 콘텐츠 조각을 만들 수 있습니다.

에셋의 한 유형인 콘텐츠 조각은 다른 에셋과 마찬가지로 AEM Assets에서 구성 및 관리되어야 합니다.

* 번역이 필요한 경우(또는 필요한 경우) Assets 폴더 구조에서 로케일 폴더를 사용합니다
* 컨텐츠 조각을 찾아 관리하기 쉽도록 논리적으로 구성합니다.

이 단계에서는 `/content/dam/wknd-mobile/en/events` 자산 폴더에 `Punkrock Fest`에 대한 새 [!DNL Event]을(를) 만듭니다.

1. **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 파일] > [!DNL WKND Mobile] >[!DNL English]**(으)로 이동하여 자산 폴더를 만듭니다&#x200B;**[!DNL Events]**.
1. **[!UICONTROL Assets] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 내에서 **[!DNL Punkrock Fest]**&#x200B;이라는 제목을 가진 **[!DNL Event]** 형식의 새 콘텐츠 조각을 만듭니다.
1. 새로 만든 [!DNL Event] 콘텐츠 조각을 작성합니다.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;설명 줄 몇 개를 입력하세요...>**
   * [!DNL Event Date] : **&lt;미래 날짜 선택>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **파충류 집**
   * [!DNL Venue City] : **뉴욕**

   변경 내용을 저장하려면 맨 위의 작업 표시줄에서 **[!UICONTROL 저장]**&#x200B;을 탭하세요.

1. [AEM의 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 AEM 작성자에 아래 패키지를 설치하십시오. 이 패키지에는 여러 이벤트 콘텐츠 조각이 포함되어 있습니다.

   [파일 가져오기: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 콘텐츠 조각의 JCR 구조 검토

*이 섹션은 정보 제공용으로만 제공되며 콘텐츠 조각 모델에서 만들어진 콘텐츠 조각의 기본 JCR 구조를 사회화하기 위한 것입니다.*

1. AEM 작성자에서 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**&#x200B;을(를) 엽니다.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 JCR의 [!DNL Punkrock Fest] [!DNL Event] 콘텐츠 조각을 나타내는 노드인 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)(으)로 이동합니다.
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 확장합니다.
**속성 창**&#x200B;에서 [!DNL Event] 콘텐츠 조각 모델 정의를 가리키는 속성 `cq:model`이 있는지 검토하십시오.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data` 노드 아래에서 [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 선택하고 속성을 검토합니다. 이 노드에는 [!DNL Event] 콘텐츠 조각 모델을 작성하는 동안 수집된 콘텐츠가 포함되어 있습니다. JCR 속성 이름은 콘텐츠 조각 모델 속성 이름에 해당하고 값은 &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] 콘텐츠 조각의 작성된 값에 해당합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 다음 단계

[AEM의 [!UICONTROL 패키지 관리자]](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM 작성자에 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 콘텐츠 패키지를 설치하는 것이 좋습니다. 이 패키지에는 자습서의 이 장과 이전 장에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [4장 - AEM Content Services 템플릿 정의](./chapter-4.md)
