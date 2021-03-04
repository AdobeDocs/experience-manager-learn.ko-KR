---
title: 3장 - 이벤트 컨텐츠 조각 작성 - 컨텐츠 서비스
seo-title: AEM 컨텐츠 서비스 시작하기 - 3장 - 이벤트 컨텐츠 조각 작성
description: AEM 헤드리스 자습서의 3장에서는 2장에서 만든 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법을 다룹니다.
seo-description: AEM 헤드리스 자습서의 3장에서는 2장에서 만든 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법을 다룹니다.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 2%

---


# 3장 - 이벤트 컨텐츠 조각 작성

AEM 제목 없는 자습서의 3장에서는 [2장](./chapter-2.md)에서 만든 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법에 대해 설명합니다.

## 이벤트 컨텐츠 조각 작성

[!DNL Event] 컨텐츠 조각 모델을 만들고 WKND에 대한 AEM 구성을 `/content/dam/wknd-mobile` 자산 폴더(`cq:conf` 속성을 통해)에 적용하면 [!DNL Event] 컨텐츠 조각을 만들 수 있습니다.

자산 유형인 컨텐츠 조각은 다른 자산과 마찬가지로 AEM Assets에서 구성하고 관리해야 합니다.

* 번역해야 하는 경우 에셋 폴더 구조의 로케일 폴더를 사용하십시오.
* 컨텐츠 조각을 논리적으로 구성하여 손쉽게 찾고 관리할 수 있습니다.

이 단계에서 `/content/dam/wknd-mobile/en/events` 자산 폴더에 `Punkrock Fest`에 대한 새 [!DNL Event]을 만듭니다.

1. **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;으로 이동하여 자산 폴더 **[!DNL Events]**&#x200B;을 만듭니다.
1. **[!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 내에 **[!DNL Punkrock Fest]** 제목의 새 컨텐트 조각을 만듭니다.**[!DNL Event]**
1. 새로 만든 [!DNL Event] 컨텐츠 조각을 작성합니다.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **더 파충류 하우스**
   * [!DNL Venue City] : **New York**

   위쪽 작업 표시줄의 **[!UICONTROL 저장]**&#x200B;을 눌러 변경 내용을 저장합니다.

1. [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 AEM 작성자에 아래 패키지를 설치합니다. 이 패키지에는 다수의 이벤트 컨텐츠 조각이 들어 있습니다.

   [파일 가져오기:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 컨텐츠 조각의 JCR 구조 검토

*이 섹션은 정보 전용이며 컨텐츠 조각 모델에서 생성된 컨텐츠 조각의 기본 JCR 구조를 연계하기 위한 것입니다.*

1. AEM 작성자에서 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**&#x200B;을(를) 엽니다.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 JCR의 [!DNL Punkrock Fest] [!DNL Event] 컨텐츠 조각을 나타내는 노드인 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)로 이동합니다.
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 확장합니다.
**속성 창**&#x200B;에서 [!DNL Event] 컨텐츠 조각 모델 정의를 가리키는 속성 `cq:model`이 있는지 검토합니다.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data` 노드 아래에서 [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 선택하고 속성을 검토합니다. 이 노드는 [!DNL Event] 컨텐츠 조각 모델을 작성하는 동안 수집된 컨텐츠를 포함합니다. JCR 속성 이름은 컨텐츠 조각 모델 속성 이름&#39;에 해당하고 값은 &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] 컨텐츠 조각의 작성 값에 해당합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 다음 단계

[AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM Author에 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 설치하는 것이 좋습니다. 이 패키지에는 자습서의 이전 장과 본 장에 나와 있는 구성 및 컨텐츠가 들어 있습니다.

* [4장 - AEM 컨텐츠 서비스 템플릿 정의](./chapter-4.md)
