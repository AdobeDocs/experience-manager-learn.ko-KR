---
title: 6장 - AEM 게시에서 콘텐츠를 JSON으로 노출 - 콘텐츠 서비스
description: AEM Headless 자습서 6장에서는 모바일 앱에서 사용할 수 있도록 AEM Publish에 필요한 모든 패키지, 구성 및 콘텐츠가 있는지 확인하는 방법에 대해 다룹니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 235
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 6장 - 게재를 위해 AEM 게시에서 컨텐츠 노출

AEM Headless 자습서 6장에서는 모바일 앱에서 사용할 수 있도록 AEM Publish에 필요한 모든 패키지, 구성 및 콘텐츠가 있는지 확인하는 방법에 대해 다룹니다.

## AEM Content Services용 콘텐츠 게시

AEM Content Services를 통해 이벤트를 유도하기 위해 만들어진 구성 및 콘텐츠는 모바일 앱이 액세스할 수 있도록 AEM Publish에 게시되어야 합니다.

AEM Content Services는 구성(콘텐츠 조각 모델, 편집 가능한 템플릿), 에셋(콘텐츠 조각, 이미지) 및 페이지에서 빌드되므로 이러한 모든 조각은 다음을 포함한 AEM 콘텐츠 관리 기능을 자동으로 사용할 수 있습니다.

* 검토 및 처리를 위한 워크플로
* AEM Publish의 AEM Content Services 끝점에서 콘텐츠를 푸시하고 가져오기 위한 활성화/비활성화

1. 다음을 확인합니다. **[!DNL WKND Mobile]응용 프로그램 패키지**, 나열된 위치 [1장](./chapter-1.md#wknd-mobile-application-packages), 설치 위치: **AEM 게시** 사용 [!UICONTROL 패키지 관리자].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 게시 **[!DNL WKND Mobile Events API]편집 가능한 템플릿**
   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**
   1. 다음 항목 선택 **[!DNL Event API]** 템플릿
   1. 누르기 **[!UICONTROL 게시]** 맨 위의 작업 표시줄에서
   1. 게시 **템플릿** 및 **모든 참조** (콘텐츠 정책, 콘텐츠 정책 매핑 및 템플릿)

1. 게시 **[!DNL WKND Mobile Events]컨텐츠 조각**.

   이벤트 API가 콘텐츠 조각 목록 구성 요소를 사용하므로 이 작업이 필요합니다. 이 구성 요소는 콘텐츠 조각을 구체적으로 참조하지 않습니다.

   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 에셋] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 모두 선택 **[!DNL Event]** 컨텐츠 조각
   1. 탭 **[!UICONTROL 게시 관리]** 맨 위의 작업 표시줄에서
   1. 기본값 유지 **게시** 그대로 작업, 탭 **[!UICONTROL 다음]** 맨 위의 작업 표시줄에서
   1. 선택 **모두** 컨텐츠 조각
   1. 누르기 **[!UICONTROL 게시]** 맨 위의 작업 표시줄에서
      * *다음 [!DNL Events] 콘텐츠 조각 모델 및 참조 이벤트 이미지는 콘텐츠 조각과 함께 자동으로 게시됩니다.*

1. 게시 **[!DNL Events API]페이지**.
   1. 다음으로 이동 **[!UICONTROL AEM] > [!UICONTROL 사이트] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 다음 항목 선택 **[!DNL Events]** 페이지
   1. 탭 **[!UICONTROL 게시 관리]** 맨 위의 작업 표시줄에서
   1. 기본값 유지 **게시** 그대로 작업, 탭 **[!UICONTROL 다음]** 맨 위의 작업 표시줄에서
   1. 다음 항목 선택 **[!DNL Events]** 페이지
   1. 누르기 **[!DNL Publish]** 맨 위의 작업 표시줄에서

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM 게시 확인

1. 새 웹 브라우저에서 AEM Publish에서 로그아웃되었는지 확인하고 다음 URL을 요청합니다(대체 `http://localhost:4503` 모든 호스트에 대해:port AEM Publish가 실행 중입니다.)

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   이러한 요청은 해당 AEM 작성자 끝점을 검토할 때와 동일한 JSON 응답을 반환해야 합니다. 그렇지 않으면 모든 게시가 성공했는지 확인합니다(복제 큐 확인). [!DNL WKND Mobile] `ui.apps` 패키지가 AEM Publish에 설치되고 `error.log` AEM 게시용.

## 다음 단계

설치할 추가 패키지가 없습니다. 이 섹션에 설명된 내용과 구성이 AEM Publish에 게시되어 있는지 확인하십시오. 그렇지 않으면 후속 챕터가 작동하지 않습니다.

* [7장 - 모바일 앱에서 AEM Content Services 사용](./chapter-7.md)
