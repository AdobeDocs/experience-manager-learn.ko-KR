---
title: 6장 - AEM Publish에서 JSON으로 컨텐츠 노출 - 컨텐츠 서비스
description: AEM Headless 자습서의 6장에서는 모바일 앱의 소비를 허용하는 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 보장하는 내용을 다룹니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# 6장 - 전달을 위해 AEM 게시에 있는 컨텐츠 노출

AEM Headless 자습서의 6장에서는 모바일 앱에서 이를 사용할 수 있도록 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 보장하는 내용을 다룹니다.

## AEM Content Services용 컨텐츠 게시

AEM Content Services를 통해 이벤트를 구동하기 위해 만들어진 구성 및 컨텐츠는 모바일 앱에서 액세스할 수 있도록 AEM Publish에 게시해야 합니다.

AEM Content Services는 구성(컨텐츠 조각 모델, 편집 가능한 템플릿), 자산(컨텐츠 조각, 이미지) 및 페이지에서 빌드되므로 이러한 모든 조각은 다음을 포함한 AEM 컨텐츠 관리 기능을 자동으로 사용할 수 있습니다.

* 검토 및 처리 워크플로우
* AEM Publish의 AEM Content Services 엔드포인트에서 컨텐츠를 푸시하고 가져오는 데 대한 활성화/비활성화

1. [!UICONTROL 패키지 관리자]을 사용하여 [Chapter 1](./chapter-1.md#wknd-mobile-application-packages)에 나열된 **[!DNL WKND Mobile]응용 프로그램 패키지**&#x200B;가 **AEM 게시**&#x200B;에 설치되어 있는지 확인합니다.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. **[!DNL WKND Mobile Events API]편집 가능한 템플릿** 게시
   1. **[!UICONTROL AEM] > [!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**&#x200B;로 이동합니다.
   1. **[!DNL Event API]** 템플릿을 선택합니다
   1. 맨 위 작업 표시줄에서 **[!UICONTROL 게시]**&#x200B;를 누릅니다
   1. **템플릿** 및 **모든 참조**(컨텐츠 정책, 컨텐츠 정책 매핑 및 템플릿)를 게시합니다.

1. **[!DNL WKND Mobile Events]컨텐츠 조각**&#x200B;을(를) 게시합니다.

이벤트 API는 컨텐츠 조각 목록 구성 요소를 사용하므로 특히 컨텐츠 조각을 참조하지 않습니다.
1. **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**로 이동합니다.
1. 모든 **[!DNL Event]** 컨텐츠 조각을 선택합니다
1. 맨 위 작업 표시줄에서 **[!UICONTROL 게시 관리]**를 탭합니다
1. 기본 **게시** 작업을 그대로 두고 맨 위 작업 표시줄에서 **[!UICONTROL 다음]**을 탭합니다
1. **모든** 컨텐츠 조각을 선택합니다.
1. 맨 위 작업 표시줄에서 **[!UICONTROL 게시]**를 탭합니다
* *컨텐츠 조각 모델 및 참조 이벤트 이미지가 컨텐츠 조각과 함께 자동으로 게시됩니다.*[!DNL Events]

1. **[!DNL Events API]페이지**&#x200B;를 게시합니다.
   1. **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;로 이동합니다.
   1. **[!DNL Events]** 페이지를 선택합니다
   1. 맨 위 작업 표시줄에서 **[!UICONTROL 게시 관리]**&#x200B;를 탭합니다
   1. 기본 **게시** 작업을 그대로 둔 채, 맨 위의 작업 표시줄에서 **[!UICONTROL 다음]**&#x200B;을 탭합니다
   1. **[!DNL Events]** 페이지를 선택합니다
   1. 맨 위 작업 표시줄에서 **[!DNL Publish]** 을 누릅니다

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM 게시 확인

1. 새 웹 브라우저에서 AEM 게시에서 로그아웃했는지 확인하고 다음 URL을 요청합니다(AEM Publish가 실행 중인 모든 호스트에 대해 `http://localhost:4503` 대체).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   이러한 요청은 해당 AEM 작성자 종단점을 검토할 때와 동일한 JSON 응답을 반환해야 합니다. 그렇지 않은 경우 모든 게시가 성공했는지( 복제 큐 확인) 확인하고, [!DNL WKND Mobile] `ui.apps` 패키지가 AEM Publish에 설치되어 있는지 확인하고, AEM Publish용 `error.log` 를 검토하십시오.

## 다음 단계

설치할 추가 패키지가 없습니다. 이 섹션에 요약된 컨텐츠 및 구성이 AEM Publish에 게시되었는지 확인합니다. 이 경우 후속 장이 작동하지 않습니다.

* [7장 - 모바일 앱에서 AEM Content Services 사용](./chapter-7.md)
