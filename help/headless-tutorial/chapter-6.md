---
title: AEM 헤드리스 시작하기 - 6장 - AEM 게시에서 JSON으로 컨텐츠 노출
description: AEM 헤드리스 자습서의 6장에서는 모바일 앱의 소비를 허용하기 위해 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 포함시키는 내용을 다룹니다.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# 6장 - 전달을 위해 AEM 게시에 컨텐츠 노출

AEM 헤드리스 자습서의 6장에서는 모바일 앱의 소비를 허용하기 위해 필요한 모든 패키지, 구성 및 컨텐츠를 AEM Publish에 포함시키는 내용을 다룹니다.

## AEM 컨텐츠 서비스용 컨텐츠 게시

AEM Content Services를 통해 이벤트를 유도하는 구성 및 컨텐츠는 AEM Publish에 게시되어야 모바일 앱에서 액세스할 수 있습니다.

AEM Content Services는 구성(컨텐츠 조각 모델, 편집 가능한 템플릿), 자산(컨텐츠 조각, 이미지) 및 페이지에서 만들어지므로 다음과 같은 AEM 컨텐츠 관리 기능이 자동으로 제공됩니다.

* 검토 및 처리를 위한 워크플로우
* AEM Publish의 AEM Content Services 엔드포인트에서 콘텐츠를 푸시하고 가져오는 데 대한 활성화/비활성화

1. 1장에 나열된 **[!DNL WKND Mobile]응용 프로그램 패키지가** Package Manager를 사용하여 [AEM](./chapter-1.md#wknd-mobile-application-packages)Publish **에** [!UICONTROL 설치되는지]확인합니다.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 편집 가능한 템플릿 **[!DNL WKND Mobile Events API]게시**
   1. AEM **[!UICONTROL >]도구[!UICONTROL >]일반> 템플릿으로이동합니다.[!DNL WKND Mobile]**
   1. 템플릿 **[!DNL Event API]** 선택
   1. 상단 **[!UICONTROL 작업]** 표시줄에서 게시를 누릅니다.
   1. 템플릿 **및** 모든 참조 **** (컨텐츠 정책, 컨텐츠 정책 매핑 및 템플릿) 게시

1. 컨텐츠 조각을 **[!DNL WKND Mobile Events]게시합니다**.

이는 이벤트 API가 컨텐츠 조각 목록 구성 요소를 사용하므로 컨텐츠 조각을 특별히 참조하지 않습니다.
1. **[!UICONTROL AEM][!UICONTROL >]자산[!UICONTROL >]파일[!DNL WKND Mobile]> > > > 11로 이동합니다. 모든 컨텐츠 조각[!DNL English]을[!DNL Events]**&#x200B;선택합니다.11을 **[!DNL Event]** **** **** **** **** **** *[!DNL Events]누릅니다.11을 누릅니다. 1을 누릅니다. 1을 누릅니다. 기본 조치를 누릅니다.1을 누릅니다. 기본 조치1. 기본 조치Bfragets를 누르면맨 윗부분의 작업이Bar를 누릅니다1를 누릅니다. 최상위 작업 표시줄의 게시*최상위 작업 모음에서 게시* 컨텐츠 조각 모델 및 참조 컨텐트와 함께 자동으로 게시됩니다.*

1. 페이지를 **[!DNL Events API]게시합니다**.
   1. AEM **[!UICONTROL >]사이트>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. 페이지 **[!DNL Events]** 선택
   1. 상단 작업 **[!UICONTROL 모음에서 게시]** 관리를 누릅니다.
   1. 기본 **게시** 작업을 그대로 **[!UICONTROL 두고 상단 작업]** 모음에서 다음을 누릅니다
   1. 페이지 **[!DNL Events]** 선택
   1. 상단 작업 막대 **[!DNL Publish]** 에서 탭

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM 게시 확인

1. 새 웹 브라우저에서 AEM 게시가 로그아웃되었는지 확인하고 다음 URL을 요청합니다(어떤 호스트:포트 AEM 게시가 실행 중인 호스트 `http://localhost:4503` 로 대체됨).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   이러한 요청은 해당 AEM 작성자 끝점을 검토할 때와 동일한 JSON 응답을 반환해야 합니다. 그렇지 않은 경우 모든 발행물이 성공했는지(복제 큐 확인) 확인하고, 패키지가 AEM 게시에 [!DNL WKND Mobile] 설치되어 있는지 확인하고 AEM 게시에 `ui.apps` `error.log` 대해 검토하십시오.

## 다음 단계

설치할 추가 패키지가 없습니다. 이 섹션에 요약된 컨텐츠와 구성이 AEM Publish에 게시되었는지 확인하십시오. 그렇지 않으면 이후 장이 작동하지 않습니다.

* [7장 - 모바일 앱에서 AEM 컨텐츠 서비스 사용](./chapter-7.md)
