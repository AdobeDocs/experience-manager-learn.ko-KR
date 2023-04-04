---
title: 3장 - 이벤트 컨텐츠 조각 작성 - 컨텐츠 서비스
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: AEM Headless 자습서의 3장에서는 2장에서 작성한 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각 만들기 및 작성을 다룹니다.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 2%

---

# 3장 - 이벤트 컨텐츠 조각 작성

AEM Headless 자습서의 3장에서는 [제2장](./chapter-2.md).

## 이벤트 컨텐츠 조각 작성

다음 포함 [!DNL Event] 만들어진 컨텐츠 조각 모델 및 WKND용 AEM 구성이 `/content/dam/wknd-mobile` 자산 폴더(를 통해) `cq:conf` property), [!DNL Event] 컨텐츠 조각을 만들 수 있습니다.

자산 유형인 컨텐츠 조각은 다른 자산과 마찬가지로 AEM Assets에서 구성 및 관리해야 합니다.

* 번역이 필요하거나 필요할 수 있는 경우 자산 폴더 구조에서 로케일 폴더를 사용합니다
* 컨텐츠 조각을 논리적으로 구성하면 쉽게 찾아 관리할 수 있습니다

이 단계에서 새 항목을 만듭니다 [!DNL Event] 대상 `Punkrock Fest` 에서 `/content/dam/wknd-mobile/en/events` assets 폴더.

1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] >[!DNL English]** 자산 폴더 및 만들기 **[!DNL Events]**.
1. 내 **[!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 유형의 새 컨텐츠 조각 만들기 **[!DNL Event]** 제목 **[!DNL Punkrock Fest]**.
1. 새로 만든 작성 [!DNL Event] 컨텐츠 조각.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **더 파충류 하우스**
   * [!DNL Venue City] : **New York**

   탭 **[!UICONTROL 저장]** 맨 위 작업 표시줄에서 변경 사항을 저장합니다.

1. 사용 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 눌러 아래 패키지를 AEM Author에 설치합니다. 이 패키지에는 많은 이벤트 컨텐츠 조각이 들어 있습니다.

   [파일 가져오기: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 컨텐츠 조각의 JCR 구조 검토

*이 섹션은 정보 전용이며 컨텐츠 조각 모델에서 만들어진 컨텐츠 조각의 기본 JCR 구조를 결합하기 위한 것입니다.*

1. 열기 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** AEM 작성자.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) 이 노드는 [!DNL Punkrock Fest] [!DNL Event] JCR의 컨텐츠 조각.
1. 를 확장합니다. [데이터](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드 아래에 있어야 합니다.
에서 검토 **속성 창** 그것은 속성을 가지고 있습니다 `cq:model` 이 점은 [!DNL Event] 컨텐츠 조각 모델 정의.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 아래 `data` 노드 선택 [마스터](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드 및 속성을 검토합니다. 이 노드는 [!DNL Event] 컨텐츠 조각 모델 . JCR 속성 이름은 컨텐츠 조각 모델 속성 이름의 에 해당하며 값은 &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] 컨텐츠 조각.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 다음 단계

설치하는 것이 좋습니다 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통해 AEM 작성자의 컨텐츠 패키지 [AEM [!UICONTROL 패키지 관리자]](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이전 장과 이 장에 설명된 구성 및 컨텐츠가 들어 있습니다.

* [4장 - AEM Content Services 템플릿 정의](./chapter-4.md)
