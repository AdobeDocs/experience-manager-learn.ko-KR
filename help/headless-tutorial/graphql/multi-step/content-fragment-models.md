---
title: 컨텐츠 조각 모델 정의 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. AEM에서 컨텐츠 조각 모델을 사용하여 컨텐츠를 모델링하고 스키마를 구축하는 방법을 알아봅니다. 기존 모델을 검토하고 새 모델을 만듭니다. 스키마를 정의하는 데 사용할 수 있는 다양한 데이터 유형에 대해 알아봅니다.
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 1%

---

# 컨텐츠 조각 모델 정의 {#content-fragment-models}

이 장에서는 컨텐츠를 모델링하고 스키마를 구축하는 방법을 알아봅니다 **컨텐츠 조각 모델**. 기존 모델을 검토하고 새 모델을 만듭니다. 또한 모델의 일부로 스키마를 정의하는 데 사용할 수 있는 다양한 데이터 유형에 대해서도 학습합니다.

이 장에서는 **기여자**: WKND 브랜드의 일부로 매거진과 모험 컨텐츠를 작성하는 사용자들을 위한 데이터 모델입니다.

## 전제 조건 {#prerequisites}

이 내용은 여러 부분으로 구성된 자습서이며 [빠른 설정](../quick-setup/local-sdk.md) 을(를) 완료했습니다.

## 목표 {#objectives}

* 새 컨텐츠 조각 모델을 만듭니다.
* 모델 작성을 위해 사용 가능한 데이터 유형과 유효성 검사 옵션을 식별합니다.
* 컨텐츠 조각 모델이 정의하는 방법을 이해합니다 **둘 다** 컨텐츠 조각에 대한 데이터 스키마 및 작성 템플릿입니다.

## 컨텐츠 조각 모델 개요 {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

위의 비디오에서는 컨텐츠 조각 모델 작업에 대한 높은 수준의 개요를 제공합니다.

>[!CAUTION]
>
> 위의 비디오에서는 **기여자** 이름을 가진 모델 `Contributors`. 자체 환경에서 단계를 수행할 때 제목에 단일 양식이 사용되는지 확인하십시오. `Contributor` 없는 **s**. 컨텐츠 조각 모델 이름을 지정하면 자습서에서 나중에 수행될 GraphQL API 호출을 구동합니다.

## Inspect the Adventure Content Fragment Model

이전 장에서는 여러 모험 컨텐츠 조각이 편집되어 외부 애플리케이션에 표시되었습니다. Adventure 컨텐츠 조각 모델 을 검사하여 이러한 조각의 기본 데이터 스키마를 이해하겠습니다.

1. 에서 **AEM 시작** 메뉴 탐색 **도구** > **자산** > **컨텐츠 조각 모델**.

   ![컨텐츠 조각 모델로 이동합니다](assets/content-fragment-models/content-fragment-model-navigation.png)

1. 로 이동합니다. **WKND 사이트** 폴더 위로 마우스를 가져갑니다. **모험** 컨텐츠 조각 모델 을 클릭하고 **편집** 아이콘(연필)을 클릭하여 모델을 엽니다.

   ![Adventure 컨텐츠 조각 모델 열기](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. 이렇게 하면 **컨텐츠 조각 모델 편집기**. 필드에 따라 Adventure 모델이 서로 다른 것을 정의합니다 **데이터 유형** 좋아요 **한 줄 텍스트**, **여러 줄 텍스트**, **열거형**, 및 **컨텐츠 참조**.

1. 편집기의 오른쪽 열에는 사용 가능한 항목이 나열됩니다 **데이터 유형** 컨텐츠 조각 작성에 사용되는 양식 필드를 정의하는 방법입니다.

1. 을(를) 선택합니다 **제목** 기본 패널의 필드. 오른쪽 열에서 **속성** 탭:

   ![어드벤처 제목 속성](assets/content-fragment-models/adventure-title-properties-tab.png)

   관찰하십시오 **속성 이름** 필드가 `adventureTitle`. AEM에 지속되는 속성의 이름을 정의합니다. 다음 **속성 이름** 또한 은 **key** 데이터 스키마의 일부로 이 속성의 이름입니다. 이 **key** GraphQL API를 통해 컨텐츠 조각 데이터가 노출될 때 사용됩니다.

   >[!CAUTION]
   >
   > 수정 **속성 이름** 필드 **after** 컨텐츠 조각은 모델에서 파생되며 다운스트림 효과가 있습니다. 기존 조각의 필드 값은 더 이상 참조되지 않으며 GraphQL에 의해 노출된 데이터 스키마가 변경되어 기존 애플리케이션에 영향을 줍니다.

1. 에서 아래로 스크롤합니다. **속성** 탭 및 보기 **유효성 검사 유형** 드롭다운.

   ![유효성 검사 옵션 사용 가능](assets/content-fragment-models/validation-options-available.png)

   에 대한 기본 양식 유효성 검사를 사용할 수 있습니다 **이메일** 및 **URL**. 또한 **사용자 지정** 정규 표현식을 사용한 유효성 검사.

1. 클릭 **취소** 컨텐츠 조각 모델 편집기를 닫으려면 를 클릭합니다.

## 기여자 모델 만들기

다음으로, **기여자**: WKND 브랜드의 일부로 매거진과 모험 컨텐츠를 작성하는 사용자들을 위한 데이터 모델입니다.

1. 클릭 **만들기** 오른쪽 상단 모서리에서 **모델 만들기** 마법사
1. 대상 **모델 제목** 다음을 입력합니다. **기여자** 을(를) 클릭합니다. **만들기**

   ![컨텐츠 조각 모델 마법사](assets/content-fragment-models/content-fragment-model-wizard.png)

   클릭 **열기** 를 클릭하여 새로 생성된 모델을 엽니다.

1. 끌어다 놓기 **한 줄 텍스트** 요소를 기본 패널에 추가합니다. 에 다음 속성을 입력합니다. **속성** 탭:

   * **필드 레이블**: **전체 이름**
   * **속성 이름**: `fullName`
   * 확인 **필수 여부**

   ![전체 이름 속성 필드](assets/content-fragment-models/full-name-property-field.png)

1. 을(를) 클릭합니다. **데이터 유형** 탭 및 드래그하여 놓기 **여러 줄 텍스트** 아래 필드 **전체 이름** 필드. 다음 속성을 입력합니다.

   * **필드 레이블**: **전기**
   * **속성 이름**: `biographyText`
   * **기본 유형**: **리치 텍스트**

1. 을(를) 클릭합니다. **데이터 유형** 탭 및 드래그하여 놓기 **컨텐츠 참조** 필드. 다음 속성을 입력합니다.

   * **필드 레이블**: **그림 참조**
   * **속성 이름**: `pictureReference`
   * **루트 경로**: `/content/dam/wknd`

   구성 시 **루트 경로** 을 클릭하여 **폴더** 아이콘을 클릭하여 경로를 선택하는 모달을 표시합니다. 이렇게 하면 작성자가 경로를 채우는 데 사용할 수 있는 폴더가 제한됩니다.

   ![루트 경로 구성됨](assets/content-fragment-models/root-path-configure.png)

1. 에 유효성 검사 추가 **그림 참조** 따라서 **이미지** 를 사용하여 필드를 채울 수 있습니다.

   ![이미지로 제한](assets/content-fragment-models/picture-reference-content-types.png)

1. 을(를) 클릭합니다. **데이터 유형** 탭 및 드래그하여 놓기 **열거형**  아래의 데이터 유형 **그림 참조** 필드. 다음 속성을 입력합니다.

   * **필드 레이블**: **직업**
   * **속성 이름**: `occupation`

1. 몇 개 추가 **옵션** 사용 **옵션 추가** 버튼을 클릭합니다. 에 동일한 값 사용 **옵션 레이블** 및 **옵션 값**:

   **아티스트**, **영향력 있는 사용자**, **사진사**, **여행자**, **작성기**, **YouTuber**

   ![작업 옵션 값](assets/content-fragment-models/occupation-options-values.png)

1. 마지막 **기여자** 모델은 다음과 같습니다.

   ![최종 기여자 모델](assets/content-fragment-models/final-contributor-model.png)

1. 클릭 **저장** 변경 사항을 저장하려면 을 클릭합니다.

## 기여자 모델 활성화

컨텐츠 조각 모델은 **활성화됨** 컨텐츠 작성자가 사용하기 전에 가능한 작업 **비활성화** 컨텐츠 조각 모델 을 사용할 수 없으므로 작성자가 사용할 수 없습니다. 다음 사항을 수정하는 것을 기억하십시오 **속성 이름** 모델의 필드 중 하나는 기본 데이터 스키마를 변경하며 기존 조각 및 외부 애플리케이션에 상당한 다운스트림 효과를 가질 수 있습니다. 에 사용되는 명명 규칙을 신중히 계획하는 것이 좋습니다 **속성 이름** 컨텐츠 조각 모델 을 활성화하기 전에 필드를 작성합니다.

1. 다음을 확인합니다. **기여자** 모델이 현재 **활성화됨** state.

   ![기여자 모델 활성화](assets/content-fragment-models/enable-contributor-model.png)

   카드 위로 마우스를 이동하고 을 클릭하여 컨텐츠 조각 모델의 상태를 전환할 수 있습니다 **비활성화** / **활성화** 아이콘.

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 컨텐츠 조각 모델을 만들었습니다!

## 다음 단계 {#next-steps}

다음 장에서 [컨텐츠 조각 모델 작성](author-content-fragments.md)를 지정하는 경우 컨텐츠 조각 모델을 기반으로 새 컨텐츠 조각을 만들고 편집합니다. 컨텐츠 조각의 변형을 만드는 방법도 알아봅니다.
