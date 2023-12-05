---
title: 2장 - 이벤트 콘텐츠 조각 모델 정의 - 콘텐츠 서비스
description: AEM Headless 자습서의 2장에서는 정규화된 데이터 구조 및 이벤트 생성을 위한 작성 인터페이스를 정의하는 데 사용되는 콘텐츠 조각 모델 활성화 및 정의를 다룹니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 472
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 1%

---

# 2장 - 콘텐츠 조각 모델 사용

AEM 콘텐츠 조각 모델은 AEM 작성자가 원시 콘텐츠 생성을 템플릿화하는 데 사용할 수 있는 콘텐츠 스키마를 정의합니다. 이 접근 방식은 스캐폴딩 또는 양식 기반 작성과 유사합니다. 컨텐츠 조각의 주요 개념은 작성된 컨텐츠가 표시 유형에 관계없이 존재한다는 것입니다. 즉, 사용 중인 애플리케이션(AEM, 단일 페이지 애플리케이션 또는 모바일 앱)이 사용자에게 컨텐츠가 표시되는 방식을 제어하는 곳에서 다중 채널 사용을 목적으로 합니다.

콘텐츠 조각의 주요 관심사는 다음을 확인하는 것입니다.

1. 작성자로부터 올바른 콘텐츠가 수집됩니다
2. 콘텐츠는 소비하는 애플리케이션에 체계적이고 잘 알려진 형식으로 노출될 수 있습니다.

이 장에서는 &quot;이벤트&quot;를 모델링하고 만들기 위한 정규화된 데이터 구조 및 작성 인터페이스를 정의하는 데 사용되는 콘텐츠 조각 모델을 활성화하고 정의하는 작업을 다룹니다.

## 콘텐츠 조각 모델 활성화

컨텐츠 조각 모델 **필수** 을 통해 활성화됨 **[AEM [!UICONTROL 구성 브라우저]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

콘텐츠 조각 모델이 다음과 같은 경우 **아님** 구성이 활성화되었습니다. **[!UICONTROL 만들기] > [!UICONTROL 컨텐츠 조각]** 관련 AEM 구성에 대한 단추가 표시되지 않습니다.

>[!NOTE]
>
>AEM 구성은 다음을 나타냅니다. [컨텍스트 인식 테넌트 구성](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 아래에 저장됨 `/conf`. 일반적으로 AEM 구성은 AEM Sites에서 관리되는 특정 웹 사이트 또는 하위 콘텐츠 세트(자산, 페이지 등)를 담당하는 사업부와 상호 연관됩니다 AEM.
>
>구성이 콘텐츠 계층 구조에 영향을 주려면 다음을 통해 구성을 참조해야 합니다. `cq:conf` 속성 아래에 그룹화됩니다. (이 작업은 다음에 대해 수행됩니다. [!DNL WKND Mobile] 에서 구성 **5단계** 아래).
>
>다음의 경우 `global` 구성이 사용되고 구성이 모든 콘텐츠에 적용되며 `cq:conf` 를 설정할 필요가 없습니다.
>
>다음을 참조하십시오. [[!UICONTROL 구성 브라우저] 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 추가 정보.

1. 관련 구성을 수정할 수 있는 적절한 권한이 있는 사용자로 AEM Author에 로그인합니다.
   * 이 자습서의 경우 **admin** 사용자를 사용할 수 있습니다.
1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 구성 브라우저]**
1. 탭 **폴더 아이콘** 다음에 **[!DNL WKND Mobile]** 을(를) 선택한 다음 **[!UICONTROL 편집] 단추** 왼쪽 상단에서.
1. 선택 **[!UICONTROL 컨텐츠 조각 모델]**, 및 탭 **[!UICONTROL 저장 및 닫기]** 오른쪽 상단에서

   이를 통해 다음과 같은 기능이 있는 에셋 폴더 콘텐츠 트리에서 콘텐츠 조각 모델을 사용할 수 있습니다. [!DNL WKND Mobile] 구성이 적용되었습니다.

   >[!NOTE]
   >
   >이 구성 변경은 다음 위치에서 되돌릴 수 없습니다. [!UICONTROL AEM 구성] 웹 UI. 이 구성을 실행 취소하려면 다음 작업을 수행하십시오.
   >    
   >    1. 열기 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 다음으로 이동 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 삭제 `models` 노드
   >    
   >이 구성으로 생성된 기존 콘텐츠 조각 모델은 삭제되고 해당 정의는 아래에 저장됩니다. `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 적용 **[!DNL WKND Mobile]** 에 대한 구성 **[!DNL WKND Mobile]에셋 폴더** 콘텐츠 조각 모델의 콘텐츠 조각을 해당 에셋 폴더 계층 구조 내에서 만들 수 있도록 하려면 다음을 수행합니다.

   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 에셋] > [!UICONTROL 파일]**
   1. 다음 항목 선택 **[!UICONTROL WKND 모바일] 폴더**
   1. 탭 **[!UICONTROL 속성]** 맨 위 작업 표시줄의 단추를 클릭하여 열기 [!UICONTROL 폴더 속성]
   1. 위치 [!UICONTROL 폴더 속성]을 누릅니다. **[!UICONTROL Cloud Service]** 탭
   1. 확인 **[!UICONTROL 클라우드 구성]** 필드가 로 설정됨 **/conf/wknd-mobile**
   1. 누르기 **[!UICONTROL 저장 및 닫기]** 변경 사항을 유지하려면 오른쪽 상단에서

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __컨텐츠 조각 모델__ 다음에서 이동함: __도구 > 에셋__ 끝 __도구 > 일반__.

## 생성할 콘텐츠 조각 모델 이해

콘텐츠 조각 모델을 정의하기 전에 필요한 모든 데이터 포인트를 캡처하고 있는지 확인하기 위해 앞으로 안내할 경험을 검토해 보겠습니다. 이를 위해 모바일 애플리케이션 디자인을 검토하고 디자인 요소를 수집 콘텐츠에 매핑합니다.

다음과 같이 이벤트를 정의하는 데이터 포인트를 나눌 수 있습니다.

![콘텐츠 조각 모델 만들기](assets/chapter-2/design-to-model-mapping.png)

매핑으로 무장하면 이벤트 데이터를 수집하고 궁극적으로 노출하는 데 사용되는 콘텐츠 조각을 정의할 수 있습니다.

## 콘텐츠 조각 모델 만들기

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 콘텐츠 조각 모델]**&#x200B;로 이동합니다.
1. 탭 **[!DNL WKND Mobile]** 열 폴더입니다.
1. 누르기 **[!UICONTROL 만들기]** 콘텐츠 조각 모델 만들기 마법사를 엽니다.
1. 입력 **[!DNL Event]** (으)로 **[!UICONTROL 모델 제목]** *(설명은 선택 사항입니다.)* 및 탭 **[!UICONTROL 만들기]** 저장.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 콘텐츠 조각 모델의 구조 정의

1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 컨텐츠 조각 모델] >[!DNL WKND]**.
1. 다음 항목 선택 **[!DNL Event]** 콘텐츠 조각 모델 및 탭 **[!UICONTROL 편집]** 맨 위의 작업 표시줄에 표시됩니다.
1. 다음에서 **[!UICONTROL 데이터 유형] 탭** 오른쪽에서 **[!UICONTROL 한 줄 텍스트 입력]** 을(를) 왼쪽 드롭 영역에 추가하여 **[!DNL Question]** 필드.
1. 새로 만들기 **[!UICONTROL 한 줄 텍스트 입력]** 왼쪽에서 을(를) 선택하고 **[!UICONTROL 속성] 탭** 오른쪽에서 을 선택합니다. 다음과 같이 속성 필드를 채웁니다.

   * [!UICONTROL 렌더링 형식] : `textfield`
   * [!UICONTROL 필드 레이블] : `Event Title`
   * [!UICONTROL 속성 이름] : `eventTitle`
   * [!UICONTROL 최대 길이] : 25
   * [!UICONTROL 필수] : `Yes`

나머지 이벤트 콘텐츠 조각 모델을 생성하려면 아래에 정의된 입력 정의를 사용하여 이 단계를 반복합니다.

>[!NOTE]
>
> 다음 **속성 이름** android 애플리케이션이 이러한 이름을 키로 지정하도록 프로그래밍되므로 필드는 정확히 일치해야 합니다.

### 이벤트 설명

* [!UICONTROL 데이터 유형] : `Multi-line text`
* [!UICONTROL 필드 레이블] : `Event Description`
* [!UICONTROL 속성 이름] : `eventDescription`
* [!UICONTROL 기본 유형] : `Rich text`

### 이벤트 날짜 및 시간

* [!UICONTROL 데이터 유형] : `Date and time`
* [!UICONTROL 필드 레이블] : `Event Date and Time`
* [!UICONTROL 속성 이름] : `eventDateAndTime`
* [!UICONTROL 필수] : `Yes`

### 이벤트 유형

* [!UICONTROL 데이터 유형] : `Enumeration`
* [!UICONTROL 필드 레이블] : `Event Type`
* [!UICONTROL 속성 이름] : `eventType`
* [!UICONTROL 옵션] : `Art,Music,Performance,Photography`

### 티켓 가격

* [!UICONTROL 데이터 유형] : `Number`
* [!UICONTROL 렌더링 형식] : `numberfield`
* [!UICONTROL 필드 레이블] : `Ticket Price`
* [!UICONTROL 속성 이름] : `eventPrice`
* [!UICONTROL 유형] : `Integer`
* [!UICONTROL 필수] : `Yes`

### 이벤트 이미지

* [!UICONTROL 데이터 유형] : `Content Reference`
* [!UICONTROL 렌더링 형식] : `contentreference`
* [!UICONTROL 필드 레이블] : `Event Image`
* [!UICONTROL 속성 이름] : `eventImage`
* [!UICONTROL 루트 경로] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 필수] : `Yes`

### 장소 이름

* [!UICONTROL 데이터 유형] : `Single-line text`
* [!UICONTROL 렌더링 형식] : `textfield`
* [!UICONTROL 필드 레이블] : `Venue Name`
* [!UICONTROL 속성 이름] : `venueName`
* [!UICONTROL 최대 길이] : 20
* [!UICONTROL 필수] : `Yes`

### 장소 도시

* [!UICONTROL 데이터 유형] : `Enumeration`
* [!UICONTROL 필드 레이블] : `Venue City`
* [!UICONTROL 속성 이름] : `venueCity`
* [!UICONTROL 옵션] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>다음 **[!UICONTROL 속성 이름]** 다음을 나타냅니다. **모두** 이 값이 저장되는 JCR 속성 이름과 JSON 파일의 키. 콘텐츠 조각 모델의 수명에 걸쳐 변경되지 않는 의미 체계 이름이어야 합니다.

콘텐츠 조각 모델 생성을 완료한 후 다음과 같은 정의가 나와야 합니다.


![이벤트 컨텐츠 조각 모델](assets/chapter-2/event-content-fragment-model.png)

## 다음 단계

필요한 경우 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통한 AEM Author의 콘텐츠 패키지 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이 부분에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [3장 - 이벤트 콘텐츠 조각 작성](./chapter-3.md)
