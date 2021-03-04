---
title: 6장 - AEM 게시에서 JSON으로 컨텐츠 노출 - 컨텐츠 서비스
description: AEM 헤드리스 자습서의 6장에서는 모바일 앱의 소비를 허용하기 위해 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 포함시키는 내용을 다룹니다.
feature: '"컨텐츠 조각, API"'
topic: '"헤드리스, 콘텐츠 관리"'
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# 6장 - 전달을 위해 AEM 게시에서 컨텐츠 노출

AEM 헤드리스 자습서의 6장에서는 모바일 앱의 소비를 허용하기 위해 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 포함시키는 내용을 다룹니다.

## AEM 컨텐츠 서비스용 컨텐츠 게시

AEM Content Services를 통해 이벤트를 유도하기 위해 만들어진 구성 및 컨텐츠는 AEM Publish에 게시되어야 모바일 앱에서 액세스할 수 있습니다.

AEM 컨텐츠 서비스는 구성(컨텐츠 조각 모델, 편집 가능한 템플릿), 자산(컨텐츠 조각, 이미지) 및 페이지에서 만들어지므로 다음과 같은 모든 요소가 자동으로 AEM 컨텐츠 관리 기능을 사용할 수 있습니다.

* 검토 및 처리 워크플로우
* AEM Publish의 AEM Content Services 엔드포인트에서 콘텐츠를 푸시 및 가져오는 경우 활성화/비활성화

1. [!UICONTROL 패키지 관리자]을 사용하여 [장 1](./chapter-1.md#wknd-mobile-application-packages)에 나열된 **[!DNL WKND Mobile]응용 프로그램 패키지**&#x200B;이(가) **AEM 게시**&#x200B;에 설치되어 있는지 확인합니다.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. **[!DNL WKND Mobile Events API]편집 가능한 템플릿** 게시
   1. **[!UICONTROL AEM] > [!UICONTROL 도구] > [!UICONTROL 일반] > [!UICONTROL 템플릿] >[!DNL WKND Mobile]**&#x200B;으로 이동합니다.
   1. **[!DNL Event API]** 템플릿 선택
   1. 위쪽 작업 표시줄에서 **[!UICONTROL 게시]**&#x200B;를 탭합니다.
   1. **템플릿** 및 **모든 참조**(콘텐츠 정책, 콘텐츠 정책 매핑 및 템플릿)을 게시합니다.

1. **[!DNL WKND Mobile Events]컨텐츠 조각**&#x200B;을(를) 게시합니다.

이는 이벤트 API가 컨텐츠 조각을 특별히 참조하지 않는 컨텐츠 조각 목록 구성 요소를 사용하기 때문에 필요합니다.
1. **[!UICONTROL AEM] > [!UICONTROL 자산] > [!UICONTROL 파일] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**으로 이동합니다.
1. **[!DNL Event]** 컨텐츠 조각을 모두 선택합니다.
1. 위쪽 작업 표시줄에서 **[!UICONTROL 발행물 관리]**를 누릅니다.
1. 기본 **게시** 작업을 그대로 두고 맨 위 작업 표시줄에서 **[!UICONTROL 다음]**을 탭합니다.
1. **모든** 컨텐츠 조각을 선택합니다.
1. 위쪽 작업 표시줄에서 **[!UICONTROL 게시]**를 누릅니다.
* *컨텐트 조각 모델 및 참조 이벤트 이미지가 컨텐츠 조각과 함께 자동으로 게시됩니다.*[!DNL Events]

1. **[!DNL Events API]페이지**&#x200B;를 게시합니다.
   1. **[!UICONTROL AEM] > [!UICONTROL 사이트] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;로 이동합니다.
   1. **[!DNL Events]** 페이지 선택
   1. 위쪽 작업 표시줄에서 **[!UICONTROL 발행물 관리]**&#x200B;를 탭합니다.
   1. 기본 **게시** 동작을 그대로 둔 상태에서 상단 작업 표시줄에서 **[!UICONTROL 다음]**&#x200B;을 탭합니다.
   1. **[!DNL Events]** 페이지 선택
   1. 위쪽 작업 표시줄에서 **[!DNL Publish]**&#x200B;을 탭합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM 게시 확인

1. 새 웹 브라우저에서 AEM 게시에서 로그아웃되었는지 확인하고 다음 URL을 요청합니다(AEM 게시가 실행 중인 모든 호스트에 대해 `http://localhost:4503` 대신).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   이러한 요청은 해당 AEM 작성자 끝점을 검토할 때와 동일한 JSON 응답을 반환해야 합니다. 그렇지 않은 경우 모든 발행물이 성공했는지 확인(복제 큐 확인), [!DNL WKND Mobile] `ui.apps` 패키지가 AEM 게시에 설치되어 있는지 확인하고 AEM 게시용 `error.log` 게시를 검토하십시오.

## 다음 단계

설치할 추가 패키지가 없습니다. 이 섹션에 설명된 컨텐츠 및 구성이 AEM 게시로 게시되었는지 확인하십시오. 그렇지 않으면 후속 장이 작동하지 않습니다.

* [7장 - 모바일 앱에서 AEM 컨텐츠 서비스 사용](./chapter-7.md)
