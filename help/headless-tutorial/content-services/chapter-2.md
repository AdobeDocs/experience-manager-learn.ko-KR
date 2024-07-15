---
title: 2장 - 이벤트 콘텐츠 조각 모델 정의 - 콘텐츠 서비스
description: AEM Headless 자습서의 2장에서는 정규화된 데이터 구조 및 이벤트 생성을 위한 작성 인터페이스를 정의하는 데 사용되는 콘텐츠 조각 모델 활성화 및 정의를 다룹니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

콘텐츠 조각 모델 **은(는)**[ AEM의 [!UICONTROL 구성 브라우저]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**을(를) 통해 활성화해야 합니다**.

구성에 대해 콘텐츠 조각 모델이 **활성화되지 않음**&#x200B;인 경우 관련 AEM 구성에 대해 **[!UICONTROL 만들기] > [!UICONTROL 콘텐츠 조각]** 단추가 표시되지 않습니다.

>[!NOTE]
>
>AEM 구성은 `/conf`에 저장된 [컨텍스트 인식 테넌트 구성](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 집합을 나타냅니다. 일반적으로 AEM 구성은 AEM Sites에서 관리되는 특정 웹 사이트 또는 하위 콘텐츠 세트(자산, 페이지 등)를 담당하는 사업부와 상호 연관됩니다 AEM.
>
>구성이 콘텐츠 계층 구조에 영향을 미치려면 해당 콘텐츠 계층 구조의 `cq:conf` 속성을 통해 구성을 참조해야 합니다. (아래 **단계 5**&#x200B;의 [!DNL WKND Mobile] 구성에 대해 수행됩니다.)
>
>`global` 구성을 사용하면 구성이 모든 콘텐츠에 적용되며 `cq:conf`을(를) 설정할 필요가 없습니다.
>
>자세한 내용은 [[!UICONTROL 구성 브라우저] 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)를 참조하세요.

1. 관련 구성을 수정할 수 있는 적절한 권한이 있는 사용자로 AEM Author에 로그인합니다.
   * 이 자습서에서는 **admin** 사용자를 사용할 수 있습니다.
1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 구성 브라우저]**(으)로 이동
1. **[!DNL WKND Mobile]** 옆에 있는 **폴더 아이콘**&#x200B;을 눌러 선택한 다음 왼쪽 상단의 **[!UICONTROL 편집] 단추**&#x200B;를 누릅니다.
1. **[!UICONTROL 콘텐츠 조각 모델]**&#x200B;을 선택하고 오른쪽 상단에서 **[!UICONTROL 저장 및 닫기]**&#x200B;를 탭합니다.

   이렇게 하면 [!DNL WKND Mobile] 구성이 적용된 에셋 폴더 콘텐츠 트리에서 콘텐츠 조각 모델을 사용할 수 있습니다.

   >[!NOTE]
   >
   >이 구성 변경은 [!UICONTROL AEM 구성] 웹 UI에서 되돌릴 수 없습니다. 이 구성을 실행 취소하려면 다음 작업을 수행하십시오.
   >    
   >    1. [CRXDE Lite](http://localhost:4502/crx/de) 열기
   >    1. `/conf/wknd-mobile/settings/dam/cfm`(으)로 이동
   >    1. `models` 노드 삭제
   >    
   >이 구성으로 만들어진 기존 콘텐츠 조각 모델은 모두 삭제되며 해당 정의는 `/conf/wknd-mobile/settings/dam/cfm/models`에 저장됩니다.

1. **[!DNL WKND Mobile]Assets 폴더**&#x200B;에 **[!DNL WKND Mobile]** 구성을 적용하여 해당 Assets 폴더 계층 구조 내에서 콘텐츠 조각 모델의 콘텐츠 조각을 만들 수 있도록 합니다.

   1. **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 파일]**(으)로 이동
   1. **[!UICONTROL WKND Mobile] 폴더 선택**
   1. 상단 작업 표시줄에서 **[!UICONTROL 속성]** 버튼을 탭하여 [!UICONTROL 폴더 속성]을 엽니다.
   1. [!UICONTROL 폴더 속성]에서 **[!UICONTROL Cloud Service]** 탭을 탭하세요.
   1. **[!UICONTROL 클라우드 구성]** 필드가 **/conf/wknd-mobile**(으)로 설정되어 있는지 확인합니다.
   1. 변경 내용을 유지하려면 오른쪽 상단의 **[!UICONTROL 저장 및 닫기]**&#x200B;를 탭하세요.

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __콘텐츠 조각 모델__&#x200B;이(가) __도구 > Assets__&#x200B;에서 __도구 > 일반__(으)로 이동했습니다.

## 생성할 콘텐츠 조각 모델 이해

콘텐츠 조각 모델을 정의하기 전에 필요한 모든 데이터 포인트를 캡처하고 있는지 확인하기 위해 앞으로 안내할 경험을 검토해 보겠습니다. 이를 위해 모바일 애플리케이션 디자인을 검토하고 디자인 요소를 수집 콘텐츠에 매핑합니다.

다음과 같이 이벤트를 정의하는 데이터 포인트를 나눌 수 있습니다.

![콘텐츠 조각 모델을 만드는 중](assets/chapter-2/design-to-model-mapping.png)

매핑으로 무장하면 이벤트 데이터를 수집하고 궁극적으로 노출하는 데 사용되는 콘텐츠 조각을 정의할 수 있습니다.

## 콘텐츠 조각 모델 만들기

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 콘텐츠 조각 모델]**&#x200B;로 이동합니다.
1. **[!DNL WKND Mobile]** 폴더를 탭하여 엽니다.
1. 콘텐츠 조각 모델 만들기 마법사를 열려면 **[!UICONTROL 만들기]**&#x200B;를 탭하세요.
1. **[!DNL Event]**&#x200B;을(를) **[!UICONTROL 모델 제목]** *(설명은 선택 사항)*(으)로 입력하고 **[!UICONTROL 만들기]**&#x200B;를 탭하여 저장합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 콘텐츠 조각 모델의 구조 정의

1. **[!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 콘텐츠 조각 모델] >[!DNL WKND]**(으)로 이동합니다.
1. **[!DNL Event]** 콘텐츠 조각 모델을 선택하고 맨 위의 작업 표시줄에서 **[!UICONTROL 편집]**&#x200B;을 탭합니다.
1. 오른쪽의 **[!UICONTROL 데이터 형식] 탭**&#x200B;에서 **[!UICONTROL 한 줄 텍스트 입력]**&#x200B;을(를) 왼쪽 드롭 영역으로 드래그하여 **[!DNL Question]** 필드를 정의합니다.
1. 새 **[!UICONTROL 한 줄 텍스트 입력]**&#x200B;이 왼쪽에서 선택되어 있고 **[!UICONTROL 속성] 탭**&#x200B;이 오른쪽에서 선택되어 있는지 확인하십시오. 다음과 같이 속성 필드를 채웁니다.

   * [!UICONTROL 다른 이름으로 렌더링]: `textfield`
   * [!UICONTROL 필드 레이블] : `Event Title`
   * [!UICONTROL 속성 이름]: `eventTitle`
   * [!UICONTROL 최대 길이] : 25
   * [!UICONTROL 필수]: `Yes`

나머지 이벤트 콘텐츠 조각 모델을 생성하려면 아래에 정의된 입력 정의를 사용하여 이 단계를 반복합니다.

>[!NOTE]
>
> Android 응용 프로그램이 이러한 이름을 키로 지정하도록 프로그래밍되었으므로 **속성 이름** 필드는 정확히 일치해야 합니다.

### 이벤트 설명

* [!UICONTROL 데이터 형식]: `Multi-line text`
* [!UICONTROL 필드 레이블] : `Event Description`
* [!UICONTROL 속성 이름]: `eventDescription`
* [!UICONTROL 기본 유형]: `Rich text`

### 이벤트 날짜 및 시간

* [!UICONTROL 데이터 형식]: `Date and time`
* [!UICONTROL 필드 레이블] : `Event Date and Time`
* [!UICONTROL 속성 이름]: `eventDateAndTime`
* [!UICONTROL 필수]: `Yes`

### 이벤트 유형

* [!UICONTROL 데이터 형식]: `Enumeration`
* [!UICONTROL 필드 레이블] : `Event Type`
* [!UICONTROL 속성 이름]: `eventType`
* [!UICONTROL 옵션] : `Art,Music,Performance,Photography`

### 티켓 가격

* [!UICONTROL 데이터 형식]: `Number`
* [!UICONTROL 다른 이름으로 렌더링]: `numberfield`
* [!UICONTROL 필드 레이블] : `Ticket Price`
* [!UICONTROL 속성 이름]: `eventPrice`
* [!UICONTROL 유형]: `Integer`
* [!UICONTROL 필수]: `Yes`

### 이벤트 이미지

* [!UICONTROL 데이터 형식]: `Content Reference`
* [!UICONTROL 다른 이름으로 렌더링]: `contentreference`
* [!UICONTROL 필드 레이블] : `Event Image`
* [!UICONTROL 속성 이름]: `eventImage`
* [!UICONTROL 루트 경로]: `/content/dam/wknd-mobile/images`
* [!UICONTROL 필수]: `Yes`

### 장소 이름

* [!UICONTROL 데이터 형식]: `Single-line text`
* [!UICONTROL 다른 이름으로 렌더링]: `textfield`
* [!UICONTROL 필드 레이블] : `Venue Name`
* [!UICONTROL 속성 이름]: `venueName`
* [!UICONTROL 최대 길이] : 20
* [!UICONTROL 필수]: `Yes`

### 장소 도시

* [!UICONTROL 데이터 형식]: `Enumeration`
* [!UICONTROL 필드 레이블] : `Venue City`
* [!UICONTROL 속성 이름]: `venueCity`
* [!UICONTROL 옵션] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL 속성 이름]**&#x200B;은(는) 이 값이 저장된 JCR 속성 이름과 JSON 파일의 키 **모두**&#x200B;을(를) 나타냅니다. 콘텐츠 조각 모델의 수명에 걸쳐 변경되지 않는 의미 체계 이름이어야 합니다.

콘텐츠 조각 모델 생성을 완료한 후 다음과 같은 정의가 나와야 합니다.


![이벤트 콘텐츠 조각 모델](assets/chapter-2/event-content-fragment-model.png)

## 다음 단계

필요한 경우 [AEM의 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM 작성자에 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 콘텐츠 패키지를 설치하십시오. 이 패키지에는 자습서의 이 부분에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [3장 - 이벤트 콘텐츠 조각 작성](./chapter-3.md)
