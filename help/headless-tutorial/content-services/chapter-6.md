---
title: 6장 - AEM Publish에서 JSON으로 컨텐츠 노출 - 컨텐츠 서비스
description: AEM Headless 자습서의 6장에서는 모바일 앱의 소비를 허용하는 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 보장하는 내용을 다룹니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 6장 - 전달을 위해 AEM 게시에 있는 컨텐츠 노출

AEM Headless 자습서의 6장에서는 모바일 앱에서 이를 사용할 수 있도록 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 보장하는 내용을 다룹니다.

## AEM Content Services용 컨텐츠 게시

AEM Content Services를 통해 이벤트를 구동하기 위해 만들어진 구성 및 컨텐츠는 모바일 앱에서 액세스할 수 있도록 AEM Publish에 게시해야 합니다.

AEM Content Services는 구성(컨텐츠 조각 모델, 편집 가능한 템플릿), 자산(컨텐츠 조각, 이미지) 및 페이지에서 빌드되므로 이러한 모든 조각은 다음을 포함한 AEM 컨텐츠 관리 기능을 자동으로 사용할 수 있습니다.

* 검토 및 처리 워크플로우
* AEM Publish의 AEM Content Services 엔드포인트에서 컨텐츠를 푸시하고 가져오는 데 대한 활성화/비활성화

1. 다음을 확인합니다. **[!DNL WKND Mobile]애플리케이션 패키지**, 다음 목록에 추가됨 [제 1장](./chapter-1.md#wknd-mobile-application-packages)에 설치되어 있습니다. **AEM 게시** 사용 [!UICONTROL 패키지 관리자].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 게시 **[!DNL WKND Mobile Events API]편집 가능한 템플릿**
   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**
   1. 을(를) 선택합니다 **[!DNL Event API]** 템플릿
   1. 탭 **[!UICONTROL 게시]** 상단 작업 모음에서
   1. 게시 **템플릿** 및 **모든 참조** (컨텐츠 정책, 컨텐츠 정책 매핑 및 템플릿)

1. 게시 **[!DNL WKND Mobile Events]콘텐츠 조각**.

   이벤트 API는 컨텐츠 조각 목록 구성 요소를 사용하므로 특히 컨텐츠 조각을 참조하지 않습니다.

   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 모두 선택 **[!DNL Event]** 콘텐츠 조각
   1. 탭하기 **[!UICONTROL 게시 관리]** 상단 작업 모음에서
   1. 기본값 유지 **게시** 작업을 있는 그대로, 탭 **[!UICONTROL 다음]** 상단 작업 모음에서
   1. 선택 **모두** 콘텐츠 조각
   1. 탭 **[!UICONTROL 게시]** 상단 작업 모음에서
      * *다음 [!DNL Events] 컨텐츠 조각 모델 및 참조 이벤트 이미지는 컨텐츠 조각과 함께 자동으로 게시됩니다.*

1. 게시 **[!DNL Events API]페이지**.
   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 을(를) 선택합니다 **[!DNL Events]** 페이지
   1. 탭하기 **[!UICONTROL 게시 관리]** 상단 작업 모음에서
   1. 기본값 유지 **게시** 작업을 있는 그대로, 탭 **[!UICONTROL 다음]** 상단 작업 모음에서
   1. 을(를) 선택합니다 **[!DNL Events]** 페이지
   1. 탭 **[!DNL Publish]** 상단 작업 모음에서

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM 게시 확인

1. 새 웹 브라우저에서 AEM 게시에서 로그아웃했는지 확인하고 다음 URL(대체)을 요청합니다 `http://localhost:4503` 어떤 호스트든: port AEM Publish가 실행 중임).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   이러한 요청은 해당 AEM 작성자 종단점을 검토할 때와 동일한 JSON 응답을 반환해야 합니다. 그렇지 않은 경우 모든 게시에 성공했는지 확인합니다( 복제 큐 확인). [!DNL WKND Mobile] `ui.apps` 패키지가 AEM 게시에 설치되고 `error.log` AEM 게시용.

## 다음 단계

설치할 추가 패키지가 없습니다. 이 섹션에 요약된 컨텐츠 및 구성이 AEM Publish에 게시되었는지 확인합니다. 이 경우 후속 장이 작동하지 않습니다.

* [7장 - 모바일 앱에서 AEM Content Services 사용](./chapter-7.md)
