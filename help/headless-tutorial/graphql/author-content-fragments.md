---
title: 컨텐츠 조각 작성 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. 컨텐츠 조각 모델을 기반으로 새 컨텐츠 조각을 만들고 편집합니다. 컨텐츠 조각 변형을 만드는 방법을 알아봅니다.
sub-product: 자산
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: '"컨텐츠 조각, GraphQL API"'
topic: '"헤드리스, 콘텐츠 관리"'
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 0%

---


# 컨텐츠 조각 작성 {#authoring-content-fragments}

이 장에서는 [새로 정의된 콘텐츠 조각 모델](./content-fragment-models.md)을 기반으로 새 컨텐츠 조각을 만들고 편집합니다. 컨텐츠 조각을 변형하여 만드는 방법도 알아봅니다.

## 전제 조건 {#prerequisites}

이 자습서는 다중 부분 자습서이며 [컨텐츠 조각 모델 정의](./content-fragment-models.md)에 설명된 단계가 완료되었다고 가정합니다.

## 목표 {#objectives}

* 컨텐츠 조각 모델을 기반으로 컨텐츠 조각 작성
* 컨텐츠 조각 변형 만들기

## 컨텐츠 조각 작성 개요 {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

위의 비디오에서는 컨텐츠 조각 작성에 대한 고급 개요를 제공합니다.

## 컨텐츠 조각 {#create-content-fragment} 만들기

이전 장의 [컨텐츠 조각 모델 정의](./content-fragment-models.md)에서 **기고자** 모델이 생성되었습니다. 이 모델을 사용하여 새 컨텐츠 조각을 작성합니다.

1. **AEM 시작** 메뉴에서 **자산** > **파일**&#x200B;으로 이동합니다.
1. 폴더를 클릭하여 **WKND 사이트** > **영어** > **기여자**&#x200B;로 이동합니다. 이 폴더에는 WKND 브랜드의 기여자를 위한 헤드 샷 목록이 포함되어 있습니다.

1. 오른쪽 상단에서 **만들기**&#x200B;를 클릭하고 **컨텐츠 조각**&#x200B;을 선택합니다.

   ![새 조각 만들기를 클릭합니다.](assets/author-content-fragments/create-content-fragment-menu.png)

1. **기고자** 모델을 선택하고 **다음**&#x200B;을 클릭합니다.

   ![콘텐츠 작가 모델 선택](assets/author-content-fragments/select-contributor-model.png)

   이전 장에서 만든 동일한 **기고자** 모델입니다.

1. 제목의 **스테이시 로셀**&#x200B;을 입력하고 **만들기**&#x200B;를 클릭합니다.
1. **성공** 대화 상자에서 **열기**&#x200B;를 클릭하여 새로 만든 조각을 엽니다.

   ![새 컨텐츠 조각을 만들었습니다.](assets/author-content-fragments/new-content-fragment.png)

   이제 모델에서 정의한 필드를 컨텐츠 조각의 이 인스턴스를 작성할 수 있습니다.

1. **전체 이름**&#x200B;에 대해 다음을 입력합니다.**스테이시 로셀**
1. **이력**&#x200B;에 간단한 약력을 입력합니다. 영감이 필요하십니까? 이 [텍스트 파일](assets/author-content-fragments/stacey-roswells-bio.txt)을 다시 사용할 수 있습니다.
1. **그림 참조**&#x200B;에 대해 **폴더** 아이콘을 클릭하고 **WKND 사이트** > **영어** > **기여자** > **stai-roswels.jpg**. 이렇게 하면 다음 경로로 평가됩니다.`/content/dam/wknd/en/contributors/stacey-roswells.jpg`.
1. **직업**&#x200B;의 경우 **포토그래퍼**&#x200B;를 선택합니다.

   ![제작된 조각](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다.

## 컨텐츠 조각 변형 만들기

모든 컨텐츠 조각은 **기본** 변형으로 시작합니다. **기본** 변형은 조각의 *기본* 컨텐츠로 간주할 수 있으며 GraphQL API를 통해 컨텐츠가 노출될 때 자동으로 사용됩니다. 컨텐츠 조각을 변형하여 만들 수도 있습니다. 이 기능은 구현을 디자인하는 데 추가적인 유연성을 제공합니다.

특정 채널을 타깃팅하는 데 변형을 사용할 수 있습니다. 예를 들어 텍스트 양이 더 적은 **mobile** 변형을 만들거나 채널별 이미지를 참조할 수 있습니다. 변형이 사용되는 방법은 구현에 실제로 적용됩니다. 다른 모든 기능과 마찬가지로 사용하기 전에 신중하게 계획해야 합니다.

그런 다음 새로운 변형을 만들어 이용 가능한 기능에 대한 아이디어를 얻습니다.

1. **스테이시 로셀** 컨텐츠 조각을 다시 엽니다.
1. 왼쪽 사이드 레일에서 **변형 만들기**&#x200B;를 클릭합니다.
1. **New Variation** 양식에 **Summary**&#x200B;의 제목을 입력합니다.

   ![새로운 변형 - 요약](assets/author-content-fragments/new-variation-summary.png)

1. **이력** 여러 줄 필드를 클릭하고 **확장** 단추를 클릭하여 여러 줄 필드에 대한 전체 화면 보기를 입력합니다.

   ![전체 화면 보기 입력](assets/author-content-fragments/enter-full-screen-view.png)

1. 오른쪽 위 메뉴에서 **텍스트 요약**&#x200B;을 클릭합니다.

1. **50**&#x200B;단어 중 **Target**&#x200B;을 입력하고 **시작**&#x200B;을 클릭합니다.

   ![요약 미리 보기](assets/author-content-fragments/summarize-text-preview.png)

   요약 미리 보기가 열립니다. AEM 컴퓨터 언어 프로세서는 대상 단어 수에 따라 텍스트를 요약하려고 시도합니다. 제거할 다른 문장을 선택할 수도 있습니다.

1. 요약 작업이 만족스러우면 **요약**&#x200B;을 클릭합니다. 여러 줄 텍스트 필드를 클릭하고 **확장** 단추를 전환하여 기본 보기로 돌아갑니다.

1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다.

## 추가 컨텐츠 조각 만들기

[컨텐츠 조각 만들기](#create-content-fragment)에 설명된 단계를 반복하여 추가 **콘텐츠 작가**&#x200B;를 만듭니다. 여러 조각을 쿼리하는 방법의 예로 다음 장에서 사용됩니다.

1. **기여자** 폴더에서 오른쪽 상단의 **만들기**&#x200B;를 클릭하고 **컨텐츠 조각**&#x200B;을 선택합니다.
1. **기고자** 모델을 선택하고 **다음**&#x200B;을 클릭합니다.
1. 제목으로 **Jacob Wester**&#x200B;를 입력하고 **만들기**&#x200B;를 클릭합니다.
1. **성공** 대화 상자에서 **열기**&#x200B;를 클릭하여 새로 만든 조각을 엽니다.
1. **전체 이름**&#x200B;에 대해 다음을 입력합니다.**Jacob Wester**.
1. **이력**&#x200B;에 간단한 약력을 입력합니다. 영감이 필요하십니까? 이 [텍스트 파일](assets/author-content-fragments/jacob-wester.txt)을 다시 사용할 수 있습니다.
1. **그림 참조**&#x200B;에 대해 **폴더** 아이콘을 클릭하고 **WKND 사이트** > **영어** > **기여자** > **jacob_wester.jpg**&#x200B;로 이동합니다. 이렇게 하면 다음 경로로 평가됩니다.`/content/dam/wknd/en/contributors/jacob_wester.jpg`.
1. **작업**&#x200B;에서 **작성자**&#x200B;를 선택합니다.
1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다. 원하는 경우 변형을 만들 필요가 없습니다.

   ![추가 컨텐츠 조각](assets/author-content-fragments/additional-content-fragment.png)

   이제 2개의 **Contributors** 조각을 가져야 합니다.

## 축하합니다!{#congratulations}

축하합니다. 여러 개의 컨텐츠 조각을 제작하고 변형을 만들었습니다.

## 다음 단계 {#next-steps}

다음 장, [Explore GraphQL API](explore-graphql-api.md)에서 내장 GraphicsQL 도구를 사용하여 AEM GraphQL API를 탐색합니다. AEM에서 컨텐츠 조각 모델을 기반으로 GraphQL 스키마를 자동으로 생성하는 방법을 알아봅니다. GraphQL 구문을 사용하여 기본 쿼리를 생성해 봅니다.