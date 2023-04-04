---
title: 2장 - 이벤트 컨텐츠 조각 모델 정의 - 컨텐츠 서비스
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: AEM 헤드리스 자습서의 2장에서는 이벤트 생성을 위한 정규화된 데이터 구조 및 작성 인터페이스를 정의하는 데 사용되는 컨텐츠 조각 모델을 활성화 및 정의하는 방법에 대해 설명합니다.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 10%

---

# 2장 - 컨텐츠 조각 모델 사용

AEM 컨텐츠 조각 모델은 AEM 작성자가 원시 컨텐츠 만들기를 템플릿 설정하는 데 사용할 수 있는 컨텐츠 스키마를 정의합니다. 이 방법은 스캐폴딩 또는 양식 기반 작성과 유사합니다. 컨텐츠 조각을 사용하는 주요 개념은 작성된 컨텐츠가 프레젠테이션에 영향을 받지 않으며, 이것은 AEM, 단일 페이지 애플리케이션 또는 모바일 앱이 사용자에게 컨텐츠가 표시되는 방식을 제어하는 데 사용되는 다중 채널 사용을 위해 고안된 것입니다.

컨텐츠 조각의 주요 관심사는 다음을 확인하는 것입니다.

1. 작성자로부터 올바른 컨텐츠가 수집됩니다
2. 컨텐츠는 구조화되고 잘 이해된 형식으로 노출되어 애플리케이션을 소모할 수 있습니다.

이 장에서는 &quot;이벤트&quot;를 모델링 및 생성하기 위한 정규화된 데이터 구조 및 작성 인터페이스를 정의하는 데 사용되는 컨텐츠 조각 모델 활성화 및 정의에 대해 설명합니다.

## 컨텐츠 조각 모델 활성화

컨텐츠 조각 모델 **반드시** 를 통해 활성화 **[AEM [!UICONTROL 구성 브라우저]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

컨텐츠 조각 모델이 **not** 구성에 대해 활성화되고 **[!UICONTROL 만들기] > [!UICONTROL 컨텐츠 조각]** 관련 AEM 구성에 대해서는 단추가 표시되지 않습니다.

>[!NOTE]
>
>AEM 구성은 일련의 [컨텍스트 인식 테넌트 구성](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 저장 `/conf`. 일반적으로 AEM 구성은 AEM Sites에서 관리되는 특정 웹 사이트나 하위 컨텐츠 세트(자산, 페이지 등)를 담당하는 사업부와 상호 연결됩니다 AEM에서 확인하십시오.
>
>구성이 컨텐츠 계층에 영향을 주려면 를 통해 구성을 참조해야 합니다. `cq:conf` 해당 컨텐츠 계층 구조에 대한 속성입니다. (이것은 [!DNL WKND Mobile] 의 구성 **5단계** 아래에 표시됩니다.
>
>이 `global` 구성을 사용하면 구성이 모든 컨텐츠에 적용되며 `cq:conf` 을 설정할 필요가 없습니다.
>
>자세한 내용은 [[!UICONTROL 구성 브라우저] 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 추가 정보.

1. 적절한 권한이 있는 사용자로 AEM Author에 로그인하여 관련 구성을 수정합니다.
   * 이 자습서의 경우 **관리** 사용자는 사용할 수 있습니다.
1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 구성 브라우저]**
1. 탭하기 **폴더 아이콘** 다음 **[!DNL WKND Mobile]** 을(를) 선택하고 을(를) 누릅니다. **[!UICONTROL 편집] 버튼** 왼쪽 상단에 있습니다.
1. 선택 **[!UICONTROL 컨텐츠 조각 모델]**, 탭 **[!UICONTROL 저장 및 닫기]** 오른쪽 위에 있습니다.

   이렇게 하면 가 있는 자산 폴더 컨텐츠 트리의 컨텐츠 조각 모델을 사용할 수 있습니다 [!DNL WKND Mobile] 구성이 적용되었습니다.

   >[!NOTE]
   >
   >이 구성 변경은 [!UICONTROL AEM 구성] 웹 UI. 이 구성을 취소하려면 다음을 수행하십시오.
   >    
   >    1. 열기 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 다음으로 이동 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 삭제 `models` 노드

   >    
   >이 구성으로 생성된 모든 기존 컨텐츠 조각 모델은 삭제되고 해당 정의는 아래에 저장됩니다 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 적용 **[!DNL WKND Mobile]** 구성 **[!DNL WKND Mobile]자산 폴더** 컨텐츠 조각 모델의 컨텐츠 조각을 해당 자산 폴더 계층 구조 내에서 만들 수 있도록 허용하려면 다음을 수행하십시오.

   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일]**
   1. 을(를) 선택합니다 **[!UICONTROL WKND Mobile] 폴더**
   1. 탭하기 **[!UICONTROL 속성]** 열기 [!UICONTROL 폴더 속성]
   1. in [!UICONTROL 폴더 속성]를 눌러 **[!UICONTROL Cloud Services]** 탭
   1. 확인 **[!UICONTROL 클라우드 구성]** 필드가 **/conf/wknd-mobile**
   1. 탭 **[!UICONTROL 저장 및 닫기]** 오른쪽 상단에서 변경 사항을 유지합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __컨텐츠 조각 모델__ 다음에서 이동됨 __도구 > 자산__ to __도구 > 일반__.

## 만들 컨텐츠 조각 모델 이해

컨텐츠 조각 모델을 정의하기 전에 필요한 모든 데이터 포인트를 캡처하도록 구동할 경험을 검토하겠습니다. 이를 위해 모바일 애플리케이션 디자인을 검토하고 디자인 요소를 콘텐츠 수집 대상에 매핑하겠습니다.

다음과 같이 이벤트를 정의하는 데이터 포인트를 분류할 수 있습니다.

![컨텐츠 조각 모델 만들기](assets/chapter-2/design-to-model-mapping.png)

매핑을 사용하여 최종적으로 이벤트 데이터를 수집하고 표시하는 데 사용되는 컨텐츠 조각을 정의할 수 있습니다.

## 컨텐츠 조각 모델 만들기

1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 컨텐츠 조각 모델]**.
1. 탭하기 **[!DNL WKND Mobile]** 열 폴더입니다.
1. 탭 **[!UICONTROL 만들기]** 컨텐츠 조각 모델 생성 마법사를 엽니다.
1. Enter 키 **[!DNL Event]** 로서의 **[!UICONTROL 모델 제목]** *(설명은 선택 사항입니다)* 탭 **[!UICONTROL 만들기]** 저장

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 컨텐츠 조각 모델의 구조 정의

1. 다음으로 이동 **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 컨텐츠 조각 모델] >[!DNL WKND]**.
1. 을(를) 선택합니다 **[!DNL Event]** 컨텐츠 조각 모델 및 탭 **[!UICONTROL 편집]** 를 클릭합니다.
1. 에서 **[!UICONTROL 데이터 유형] 탭** 오른쪽에서 **[!UICONTROL 한 줄 텍스트 입력]** 을(를) 왼쪽 드롭다운 영역에 추가하여 **[!DNL Question]** 필드.
1. 새로운 **[!UICONTROL 한 줄 텍스트 입력]** 이(가) 왼쪽에 있고 **[!UICONTROL 속성] 탭** 이 오른쪽에 선택되어 있습니다. 다음과 같이 속성 필드를 채웁니다.

   * [!UICONTROL 렌더링 형식] : `textfield`
   * [!UICONTROL 필드 레이블] : `Event Title`
   * [!UICONTROL 속성 이름] : `eventTitle`
   * [!UICONTROL 최대 길이] : 25년
   * [!UICONTROL 필수] : `Yes`

아래에 정의된 입력 정의를 사용하여 이러한 단계를 반복하여 이벤트 컨텐츠 조각 모델의 나머지 부분을 만듭니다.

>[!NOTE]
>
> 다음 **속성 이름** Android 애플리케이션이 이러한 이름을 키로 프로그래밍되므로 필드가 정확히 일치해야 합니다.

### 이벤트 설명

* [!UICONTROL 데이터 형식] : `Multi-line text`
* [!UICONTROL 필드 레이블] : `Event Description`
* [!UICONTROL 속성 이름] : `eventDescription`
* [!UICONTROL 기본 유형] : `Rich text`

### 이벤트 날짜 및 시간

* [!UICONTROL 데이터 형식] : `Date and time`
* [!UICONTROL 필드 레이블] : `Event Date and Time`
* [!UICONTROL 속성 이름] : `eventDateAndTime`
* [!UICONTROL 필수] : `Yes`

### 이벤트 유형

* [!UICONTROL 데이터 형식] : `Enumeration`
* [!UICONTROL 필드 레이블] : `Event Type`
* [!UICONTROL 속성 이름] : `eventType`
* [!UICONTROL 옵션] : `Art,Music,Performance,Photography`

### 티켓 가격

* [!UICONTROL 데이터 형식] : `Number`
* [!UICONTROL 렌더링 형식] : `numberfield`
* [!UICONTROL 필드 레이블] : `Ticket Price`
* [!UICONTROL 속성 이름] : `eventPrice`
* [!UICONTROL 유형] : `Integer`
* [!UICONTROL 필수] : `Yes`

### 이벤트 이미지

* [!UICONTROL 데이터 형식] : `Content Reference`
* [!UICONTROL 렌더링 형식] : `contentreference`
* [!UICONTROL 필드 레이블] : `Event Image`
* [!UICONTROL 속성 이름] : `eventImage`
* [!UICONTROL 루트 경로] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 필수] : `Yes`

### 장소 이름

* [!UICONTROL 데이터 형식] : `Single-line text`
* [!UICONTROL 렌더링 형식] : `textfield`
* [!UICONTROL 필드 레이블] : `Venue Name`
* [!UICONTROL 속성 이름] : `venueName`
* [!UICONTROL 최대 길이] : 20년
* [!UICONTROL 필수] : `Yes`

### 장소 구/군/시

* [!UICONTROL 데이터 형식] : `Enumeration`
* [!UICONTROL 필드 레이블] : `Venue City`
* [!UICONTROL 속성 이름] : `venueCity`
* [!UICONTROL 옵션] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>다음 **[!UICONTROL 속성 이름]** 는 **둘 다** 이 값이 저장되는 JCR 속성 이름과 JSON 파일에 있는 키가 여기에 포함됩니다. 컨텐츠 조각 모델의 수명 동안 변경되지 않는 의미 있는 이름이어야 합니다.

컨텐츠 조각 모델 만들기를 완료한 후에는 다음과 같은 정의가 표시됩니다.


![이벤트 컨텐츠 조각 모델](assets/chapter-2/event-content-fragment-model.png)

## 다음 단계

선택적으로 를 설치합니다 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 를 통해 AEM 작성자의 컨텐츠 패키지 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp). 이 패키지에는 자습서의 이 부분에 설명된 구성 및 컨텐츠가 들어 있습니다.

* [3장 - 이벤트 컨텐츠 조각 작성](./chapter-3.md)
