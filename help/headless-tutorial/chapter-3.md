---
title: 3장 - 이벤트 컨텐츠 조각 작성
seo-title: AEM 컨텐츠 서비스 시작하기 - 3장 - 이벤트 컨텐츠 조각 작성
description: AEM 헤드리스 자습서의 3장에서는 2장에 만들어진 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법을 다룹니다.
seo-description: AEM 헤드리스 자습서의 3장에서는 2장에 만들어진 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법을 다룹니다.
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 2%

---


# 3장 - 이벤트 컨텐츠 조각 작성

AEM 헤드리스 자습서의 3장에서는 [2장](./chapter-2.md)에서 만들어진 컨텐츠 조각 모델에서 이벤트 컨텐츠 조각을 만들고 작성하는 방법을 다룹니다.

## 이벤트 컨텐츠 조각 작성

컨텐츠 [!DNL Event] 조각 모델을 생성하고 WKND에 대한 AEM 구성이 자산 폴더(속성을 통해) `/content/dam/wknd-mobile` 에 적용되면 컨텐츠 조각 `cq:conf` 을 [!DNL Event] 생성할 수 있습니다.

자산 유형인 컨텐츠 조각은 다른 자산과 마찬가지로 AEM Assets에서 구성하고 관리해야 합니다.

* 번역이 필요한 경우 에셋 폴더 구조의 로케일 폴더를 사용하십시오.
* 컨텐츠 조각을 논리적으로 구성하여 손쉽게 찾아 관리

이 단계에서 자산 폴더 [!DNL Event] 에 새 `Punkrock Fest` 를 만듭니다 `/content/dam/wknd-mobile/en/events` .

1. AEM **[!UICONTROL >]자산[!UICONTROL >]파일[!UICONTROL >][!DNL WKND Mobile]으로 이동하고[!DNL English]** **[!DNL Events]**&#x200B;자산 폴더를 만듭니다.
1. Within **[!UICONTROL Assets]>[!UICONTROL Files]> >[!DNL WKND Mobile]>[!DNL English]create a new Content Fragment of[!DNL Events]** Cuts of a title **[!DNL Event]** **[!DNL Punkrock Fest]**.
1. 새로 만든 컨텐츠 조각을 [!DNL Event] 작성합니다.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;몇 줄의 설명 입력..>**
   * [!DNL Event Date] : **&lt;미래의 날짜 선택>**
   * [!DNL Event Type] : **음악**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **파충류 하우스**
   * [!DNL Venue City] : **New York**

   상단 작업 **[!UICONTROL 막대에서 저장을]** 눌러 변경 사항을 저장합니다.

1. AEM [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 AEM Author에 아래 패키지를 설치합니다. 이 패키지에는 다수의 이벤트 콘텐츠 조각이 포함되어 있습니다.

   [파일 가져오기:GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 컨텐츠 조각 JCR 구조 검토

*이 섹션은 정보용으로만 제공되며 컨텐츠 조각 모델에서 생성된 컨텐츠 조각의 기본 JCR 구조를 위해 만들어졌습니다.*

1. AEM 작성자에서 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** 를 엽니다.
1. CRXDE Lite의 왼쪽 계층 메뉴에서 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)[!DNL Punkrock Fest] 로 이동합니다. 이 노드는 JCR의 컨텐츠 조각을 나타내는 [!DNL Event] 노드입니다.
1. 데이터 [노드를](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 확장합니다.
속성 창 **에서** 컨텐츠 조각 모델 정의를 가리키는 속성 `cq:model` 이 [!DNL Event] 있는지확인합니다.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 노드 아래에서 `data` 마스터 [](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 노드를 선택하고 속성을 검토합니다. 이 노드에는 컨텐츠 조각 모델을 작성하는 동안 수집된 컨텐츠가 [!DNL Event] 포함됩니다. JCR 속성 이름은 컨텐츠 조각 모델 속성 이름&#39;에 해당하고 값은 &quot;[!DNL Punkrock Fest]&quot; 컨텐츠 조각&quot;의 작성된 값에 [!DNL Event] 해당합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 다음 단계

AEM [패키지 관리자를 통해 AEM 작성자에](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip [컨텐츠 패키지를 설치하는 것이 좋습니다 ](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이전 장과 이 장에 나와 있는 구성 및 컨텐츠가 포함되어 있습니다.

* [4장 - AEM 컨텐츠 서비스 템플릿 정의](./chapter-4.md)
