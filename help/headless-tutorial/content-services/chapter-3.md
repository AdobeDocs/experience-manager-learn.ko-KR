---
title: 3장 - 이벤트 콘텐츠 조각 작성 - 콘텐츠 서비스
description: AEM Headless 자습서 3장에서는 2장에서 만든 콘텐츠 조각 모델에서 이벤트 콘텐츠 조각을 만들고 작성하는 방법에 대해 설명합니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 205
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 3장 - 이벤트 콘텐츠 조각 작성

AEM Headless 자습서의 3장에서는에서 만든 콘텐츠 조각 모델에서 이벤트 콘텐츠 조각을 만들고 작성하는 작업을 다룹니다 [챕터 2](./chapter-2.md).

## 이벤트 컨텐츠 조각 작성

포함 [!DNL Event] 콘텐츠 조각 모델이 만들어지고 WKND에 적용되는 AEM 구성 `/content/dam/wknd-mobile` 에셋 폴더(를 통해) `cq:conf` 속성), a [!DNL Event] 콘텐츠 조각을 만들 수 있습니다.

에셋의 한 유형인 콘텐츠 조각은 다른 에셋과 마찬가지로 AEM Assets에서 구성 및 관리되어야 합니다.

* 번역이 필요한 경우(또는 필요한 경우) 에셋 폴더 구조에서 로케일 폴더를 사용합니다
* 컨텐츠 조각을 찾아 관리하기 쉽도록 논리적으로 구성합니다.

이 단계에서 새 을(를) 만듭니다. [!DNL Event] 대상 `Punkrock Fest` 다음에서 `/content/dam/wknd-mobile/en/events` 에셋 폴더입니다.

1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 에셋] > [!UICONTROL 파일] > [!DNL WKND Mobile] >[!DNL English]** 자산 폴더 만들기 **[!DNL Events]**.
1. 다음 범위 내 **[!UICONTROL 에셋] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 유형의 새 콘텐츠 조각 만들기 **[!DNL Event]** 을 제목으로 **[!DNL Punkrock Fest]**.
1. 새로 만든 항목 작성 [!DNL Event] 컨텐츠 조각.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **더 파충류 하우스**
   * [!DNL Venue City] : **뉴욕**

   누르기 **[!UICONTROL 저장]** 을 클릭하여 변경 내용을 저장합니다.

1. 사용 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp), AEM 작성자에 아래 패키지를 설치합니다. 이 패키지에는 여러 이벤트 콘텐츠 조각이 포함되어 있습니다.

   [파일 가져오기: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 콘텐츠 조각의 JCR 구조 검토

*이 섹션은 정보 제공용으로만 제공되며 콘텐츠 조각 모델에서 만든 콘텐츠 조각의 기본 JCR 구조를 사회화하기 위한 것입니다.*

1. 열기 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** AEM Author에서.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) 를 나타내는 노드입니다. [!DNL Punkrock Fest] [!DNL Event] JCR의 콘텐츠 조각.
1. 확장 [데이터](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드.
다음에서 검토: **속성 창** 속성이 있음 `cq:model` 이(가) 다음을 가리킵니다. [!DNL Event] 콘텐츠 조각 모델 정의.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 아래 `data` 노드 선택 [마스터](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드에 추가하고 속성을 검토합니다. 이 노드에는 작성 중에 수집된 컨텐츠가 포함됩니다. [!DNL Event] 컨텐츠 조각 모델. JCR 속성 이름은 콘텐츠 조각 모델 속성 이름에 해당하고 값은 작성된 &quot; 값에 해당합니다.[!DNL Punkrock Fest]&quot; [!DNL Event] 컨텐츠 조각.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 다음 단계

다음을 설치하는 것이 좋습니다. [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통한 AEM Author의 콘텐츠 패키지 [AEM [!UICONTROL 패키지 관리자]](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이 장과 이전 장에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [4장 - AEM Content Services 템플릿 정의](./chapter-4.md)
