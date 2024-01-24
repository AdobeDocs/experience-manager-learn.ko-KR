---
title: 콘텐츠 조각 작성 - AEM Headless 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. 콘텐츠 조각 모델을 기반으로 새 콘텐츠 조각을 만들고 편집합니다. 콘텐츠 조각의 변형을 만드는 방법을 알아봅니다.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
duration: 214
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '853'
ht-degree: 3%

---

# 컨텐츠 조각 작성 {#authoring-content-fragments}

이 장에서는 다음을 기반으로 새 콘텐츠 조각을 만들고 편집합니다. [새로 정의된 콘텐츠 조각 모델](./content-fragment-models.md). 콘텐츠 조각의 변형을 만드는 방법도 알아봅니다.

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 다음에 설명된 단계를 가정합니다. [콘텐츠 조각 모델 정의](./content-fragment-models.md) 완료되었습니다.

## 목표 {#objectives}

* 콘텐츠 조각 모델을 기반으로 콘텐츠 조각 작성
* 콘텐츠 조각 변형 만들기

## 자산 폴더 만들기

컨텐츠 조각은 AEM Assets의 폴더에 저장됩니다. 이전 장에서 만든 모델에서 콘텐츠 조각을 만들려면 해당 조각을 저장할 폴더를 만들어야 합니다. 특정 모델에서 조각을 만들 수 있도록 폴더에 구성이 필요합니다.

1. AEM 시작 화면에서 다음으로 이동합니다. **에셋** > **파일**.

   ![에셋 파일로 이동](assets/author-content-fragments/navigate-assets-files.png)

1. 누르기 **만들기** 오른쪽 상단 모서리에서 을 누릅니다. **폴더**. 그 결과로 표시되는 대화 상자에서 다음을 입력합니다.

   * 제목*: **내 프로젝트**
   * 이름: **내 프로젝트**

   ![폴더 만들기 대화 상자](assets/author-content-fragments/create-folder-dialog.png)

1. 다음 항목 선택 **내 폴더** 폴더 및 탭 **속성**.

   ![폴더 속성 열기](assets/author-content-fragments/open-folder-properties.png)

1. 탭 **Cloud Service** 탭. 클라우드 구성 탭에서 경로 파인더를 사용하여 **내 프로젝트** 구성. 값은 다음과 같아야 합니다. `/conf/my-project`.

   ![클라우드 구성 설정](assets/author-content-fragments/set-cloud-config-my-project.png)

   이 속성을 설정하면 이전 장에서 만든 모델을 사용하여 콘텐츠 조각을 만들 수 있습니다.

1. 탭 **정책** 탭, **허용된 컨텐츠 조각 모델** 필드 경로 파인더를 사용하여 **개인** 및 **팀** 모델이 이전에 생성되었습니다.

   ![허용된 컨텐츠 조각 모델](assets/author-content-fragments/allowed-content-fragment-models.png)

   이러한 정책은 하위 폴더에 자동으로 상속되며 재정의할 수 있습니다. 태그로 모델을 허용하거나 다른 프로젝트 구성의 모델을 활성화할 수도 있습니다. 이 메커니즘은 콘텐츠 계층 구조를 관리하는 강력한 방법을 제공합니다.

1. 누르기 **저장 및 닫기** 을 클릭하여 폴더 속성에 대한 변경 사항을 저장합니다.

1. 내부 탐색 **내 프로젝트** 폴더를 삭제합니다.

1. 다음 값으로 다른 폴더를 만듭니다.

   * 제목*: **영어**
   * 이름: **en**

   가장 좋은 방법은 다국어 지원을 위한 프로젝트를 설정하는 것입니다. 다음을 참조하십시오 [자세한 내용은 다음 문서 페이지를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## 콘텐츠 조각 만들기 {#create-content-fragment}

>[!TIP]
>
>로컬 AEM SDK 사용자의 경우: AEM Assets UI를 활용하여 아래에 요약된 콘텐츠 조각 UI 대신 콘텐츠 조각을 만들고 작성할 수 있습니다. 자세한 지침은 [AEM 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html).

다음 몇 가지 콘텐츠 조각은 **팀** 및 **개인** 모델.

1. AEM 시작 화면에서 다음을 누릅니다. **컨텐츠 조각** 콘텐츠 조각 UI를 엽니다.

   ![콘텐츠 조각 UI](assets/author-content-fragments/cf-fragment-ui.png)

1. 왼쪽 레일에서 을 확장합니다. **내 프로젝트** 및 탭 **영어**.
1. 누르기 **만들기** 을(를) 호출하려면 **새 콘텐츠 조각** 대화 상자를 열고 다음 값을 입력합니다.

   * 위치: `/content/dam/my-project/en`
   * 콘텐츠 조각 모델: **개인**
   * 제목: **존 도**
   * 이름: `john-doe`

   ![새 콘텐츠 조각](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. **만들기**&#x200B;를 탭합니다.
1. 위의 단계를 반복하여 을 나타내는 조각을 생성합니다. **앨리슨 스미스**:

   * 위치: `/content/dam/my-project/en`
   * 콘텐츠 조각 모델: **개인**
   * 제목: **앨리슨 스미스**
   * 이름: `alison-smith`

   누르기 **만들기** 개인 조각을 만듭니다.

1. 그런 다음 단계를 반복하여 **팀** 을 나타내는 조각 **팀 Alpha**:

   * 위치: `/content/dam/my-project/en`
   * 콘텐츠 조각 모델: **팀**
   * 제목: **팀 Alpha**
   * 이름: `team-alpha`

   누르기 **만들기** 팀 조각을 만듭니다.

1. 아래에 세 개의 콘텐츠 조각이 있어야 합니다. **내 프로젝트** > **영어**:

   ![새 콘텐츠 조각](assets/author-content-fragments/new-content-fragments.png)

## 개인 콘텐츠 조각 편집 {#edit-person-content-fragments}

다음으로 새로 생성된 조각을 데이터로 채웁니다.

1. 옆에 있는 확인란을 탭합니다. **존 도** 및 탭 **열기**.

   ![컨텐츠 조각 열기](assets/author-content-fragments/open-fragment-for-editing.png)

1. 콘텐츠 조각 편집기에는 콘텐츠 조각 모델을 기반으로 하는 양식이 포함되어 있습니다. 콘텐츠를 추가할 다양한 필드를 채웁니다. **존 도** 조각. 프로필 사진의 경우 자신의 이미지를 AEM Assets에 업로드하십시오.

   ![콘텐츠 조각 편집기](assets/author-content-fragments/content-fragment-editor-jd.png)

1. 누르기 **저장 및 닫기** John Doe 조각에 대한 변경 사항을 저장합니다.
1. 콘텐츠 조각 UI로 돌아가서 를 엽니다. **앨리슨 스미스** 편집할 파일입니다.
1. 위의 단계를 반복하여 다음을 채웁니다. **앨리슨 스미스** 콘텐츠가 있는 조각.

## 팀 컨텐츠 조각 편집 {#edit-team-content-fragment}

1. 를 엽니다. **팀 Alpha** 콘텐츠 조각 UI를 사용하는 콘텐츠 조각.
1. 다음에 대한 필드 작성 **제목**, **짧은 이름**, 및 **설명**.
1. 다음 항목 선택 **존 도** 및 **앨리슨 스미스** 콘텐츠 조각 을 사용하여 **팀원** 필드:

   ![팀원 설정](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >를 사용하여 콘텐츠 조각을 인라인으로 만들 수도 있습니다. **새 콘텐츠 조각** 단추를 클릭합니다.

1. 누르기 **저장 및 닫기** 팀 Alpha 조각의 변경 사항을 저장합니다.

## 콘텐츠 조각 게시

>[!TIP]
>
>로컬 AEM SDK 사용자의 경우: AEM Assets UI를 활용하여 아래에 요약된 콘텐츠 조각 UI 대신 콘텐츠 조각을 게시할 수 있습니다. 자세한 지침은 [AEM 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html#publishing-and-referencing-a-fragment).

검토 및 확인 후 작성된 항목 게시 `Content Fragments`

1. AEM 시작 화면에서 다음을 누릅니다. **컨텐츠 조각** 콘텐츠 조각 UI를 엽니다.

1. 왼쪽 레일에서 을 확장합니다. **내 프로젝트** 및 탭 **영어**.

1. 콘텐츠 조각 옆에 있는 확인란을 탭한 다음 을 탭합니다 **게시**.
   ![콘텐츠 조각 게시](assets/author-content-fragments/publish-content-fragment.png)

## 축하합니다! {#congratulations}

축하합니다. 여러 콘텐츠 조각을 작성하고 변형을 만들었습니다.

## 다음 단계 {#next-steps}

다음 장에서는 [GraphQL API 살펴보기](explore-graphql-api.md), 기본 제공 GraphiQL 도구를 사용하여 AEM GraphQL API를 살펴보게 됩니다. AEM이 콘텐츠 조각 모델을 기반으로 GraphQL 스키마를 자동으로 생성하는 방법을 알아봅니다. GraphQL 구문을 사용하여 기본 쿼리를 구성해 봅니다.

## 관련 설명서

* [콘텐츠 조각 관리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [변형 - 조각 콘텐츠 작성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)
