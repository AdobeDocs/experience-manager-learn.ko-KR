---
title: 3장 - 이벤트 컨텐츠 조각 작성 - 컨텐츠 서비스
seo-title: AEM 컨텐츠 서비스 시작하기 - 3장 - 이벤트 컨텐츠 조각 작성
description: AEM Headless 자습서의 3장에서는 2장에서 작성한 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각 만들기 및 작성을 다룹니다.
seo-description: AEM Headless 자습서의 3장에서는 2장에서 작성한 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각 만들기 및 작성을 다룹니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 2%

---


# 3장 - 이벤트 컨텐츠 조각 작성

AEM Headless 자습서의 3장에서는 [Chapter 2](./chapter-2.md)에서 작성한 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 것을 다룹니다.

## 이벤트 컨텐츠 조각 작성

[!DNL Event] 컨텐츠 조각 모델을 만들고 WKND용 AEM 구성이 `/content/dam/wknd-mobile` 자산 폴더( `cq:conf` 속성을 통해)에 적용되면 [!DNL Event] 컨텐츠 조각을 만들 수 있습니다.

자산 유형인 컨텐츠 조각은 다른 자산과 마찬가지로 AEM Assets에서 구성 및 관리해야 합니다.

* 번역이 필요하거나 필요할 수 있는 경우 자산 폴더 구조에서 로케일 폴더를 사용합니다
* 컨텐츠 조각을 논리적으로 구성하면 쉽게 찾아 관리할 수 있습니다

이 단계에서는 `/content/dam/wknd-mobile/en/events` assets 폴더에서 `Punkrock Fest`에 대한 새 [!DNL Event]을 만듭니다.

1. **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;로 이동하여 자산 폴더 **[!DNL Events]**&#x200B;를 만듭니다.
1. **[!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 내에서 제목이 **[!DNL Punkrock Fest]**&#x200B;인 새 컨텐츠 조각을 만듭니다.**[!DNL Event]**
1. 새로 만든 [!DNL Event] 컨텐츠 조각을 작성합니다.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **더 파충류 하우스**
   * [!DNL Venue City] : **New York**

   맨 위 작업 표시줄에서 **[!UICONTROL 저장]**&#x200B;을 탭하여 변경 내용을 저장합니다.

1. [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 아래 패키지를 AEM Author에 설치합니다. 이 패키지에는 많은 이벤트 컨텐츠 조각이 들어 있습니다.

   [파일 가져오기:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 컨텐츠 조각의 JCR 구조 검토

*이 섹션은 정보 전용이며 컨텐츠 조각 모델에서 만들어진 컨텐츠 조각의 기본 JCR 구조를 결합하기 위한 것입니다.*

1. AEM 작성자에서 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**&#x200B;를 엽니다.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 JCR의 [!DNL Punkrock Fest] [!DNL Event] 컨텐츠 조각을 나타내는 노드인 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) 로 이동합니다.
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 확장합니다.
**속성 창**&#x200B;에서 [!DNL Event] 컨텐츠 조각 모델 정의를 가리키는 속성 `cq:model`이 있는지 검토합니다.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data` 노드 아래에서 [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 선택하고 속성을 검토합니다. 이 노드는 [!DNL Event] 컨텐츠 조각 모델을 작성하는 동안 수집된 컨텐츠를 포함합니다. JCR 속성 이름은 컨텐츠 조각 모델 속성 이름의 에 해당하며 값은 &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] 컨텐츠 조각의 작성된 값에 해당합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 다음 단계

[AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM Author에 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 설치하는 것이 좋습니다. 이 패키지에는 자습서의 이전 장과 이 장에 설명된 구성 및 컨텐츠가 들어 있습니다.

* [4장 - AEM Content Services 템플릿 정의](./chapter-4.md)
