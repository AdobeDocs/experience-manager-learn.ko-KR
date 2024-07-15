---
title: 5장 - 콘텐츠 서비스 페이지 작성 - 콘텐츠 서비스
description: AEM Headless 자습서 5장에서는 4장에 정의된 템플릿으로 페이지 만들기에 대해 다룹니다. 이러한 페이지는 JSON HTTP 끝점으로 작동합니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# 5장 - Content Services 페이지 작성

AEM Headless 자습서 5장에서는 4장에 정의된 템플릿으로 페이지 만들기에 대해 다룹니다. 이 장에서 만든 페이지는 모바일 앱에 대한 JSON HTTP 끝점 역할을 합니다.

>[!NOTE]
>
> `/content/wknd-mobile/en/api`의 페이지 콘텐츠 아키텍처가 미리 빌드되었습니다. `en` 및 `api`의 기본 페이지는 아키텍처 및 조직 용도로 사용되지만 반드시 필요한 것은 아닙니다. API 콘텐츠가 현지화될 수 있는 경우, API 페이지는 AEM Sites 페이지처럼 현지화될 수 있으므로 일반적인 언어 사본 및 다중 사이트 관리자 페이지 구성 모범 사례를 따르는 것이 좋습니다.

## 이벤트 API 페이지 생성

1. **[!UICONTROL AEM] > [!UICONTROL 사이트] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**(으)로 이동합니다.
1. **API 페이지의 레이블을 탭한 다음**, 맨 위의 작업 표시줄에서 **만들기** 단추를 탭하고 API 페이지 아래에 새 이벤트 API 페이지를 만듭니다.
   1. 상단 작업 표시줄에서 **만들기**&#x200B;를 탭합니다.
   1. **이벤트 API** 템플릿 선택
   1. **이름** 필드에 **이벤트**&#x200B;를 입력하십시오.
   1. **제목** 필드에 **이벤트 API**&#x200B;를 입력합니다.
   1. 상단 작업 표시줄에서 **만들기**&#x200B;를 탭하여 페이지를 만듭니다.
   1. AEM Sites 관리자로 돌아가려면 **완료**&#x200B;를 탭하세요.

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 이벤트 API 페이지 작성

>[!NOTE]
>
> 프로젝트는 작성자 경험을 위한 몇 가지 기본 스타일을 제공하기 위해 CSS를 제공합니다.

1. **AEM > 사이트 > WKND Mobile > 영어 > API**(으)로 이동하고, **이벤트 API** 페이지를 선택한 다음 상단 작업 표시줄에서 **편집**&#x200B;을 탭하여 **이벤트 API** 페이지를 편집합니다.
1. 자산 파인더에서 이미지 구성 요소 자리 표시자로 드래그 앤 드롭하여 앱에 표시할 **로고 이미지**&#x200B;를 추가하십시오.
   * `/content/dam/wknd-mobile/images/wknd-logo.png`에 제공된 로고를 사용하십시오.

1. **태그 줄**&#x200B;을 추가하여 이벤트 위에 표시합니다.
   1. **텍스트** 구성 요소 편집
   1. 텍스트를 입력합니다.
      * `The WKND is here.`

1. 표시할 **이벤트** 선택:
   1. **속성** 탭에서 다음 구성을 설정합니다.
      * 모델: **이벤트**
      * 상위 경로: **/content/dam/wknd-mobile/en/events**
      * 태그: **&lt;비워 둠>**
   1. **요소** 탭에서 다음 구성을 설정합니다.
      * 나열된 요소를 제거하여 이벤트 콘텐츠 조각의 모든 요소가 노출되도록 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## API 페이지의 JSON 출력 검토

`.model.json` 선택기를 사용하여 페이지를 요청하여 JSON 출력 및 해당 형식을 검토할 수 있습니다.

이 JSON 구조(또는 스키마)는 이 API의 소비자가 잘 이해해야 합니다. 구조의 어떤 측면이 수정되는지(즉, API 소비자가 이해하는 것이 중요합니다. Event API의 로고(이미지) 및 태그 라이브(텍스트) 및 유동적(예: 콘텐츠 조각 목록 구성 요소 아래에 나열된 이벤트.

게시된 API에 대한 이 계약을 위반하면 를 사용하는 앱에서 잘못된 동작이 발생할 수 있습니다.

1. 새 브라우저 탭에서 AEM Content Services의 JSON 내보내기를 호출하고 페이지 및 구성 요소를 정규화된 JSON 구조로 직렬화하는 `.model.json` 선택기를 사용하여 이벤트 API 페이지를 요청합니다.

   이러한 페이지에서 생성된 JSON 구조는 앱을 사용하는 구조가 일치해야 합니다.

1. **이벤트 API** 페이지를 **JSON**(으)로 요청합니다.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   결과는 다음과 비슷하게 표시됩니다.

![AEM Content Services JSON 출력](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 이 JSON은 사용자가 읽을 수 있도록 `.tidy` 선택기를 사용하여 **깔끔한**(형식이 지정된) 방식으로 출력할 수 있습니다.
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## 다음 단계

필요한 경우 [AEM의 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM 작성자에 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 콘텐츠 패키지를 설치하십시오. 이 패키지에는 자습서의 이 장과 이전 장에 설명된 구성과 콘텐츠가 포함되어 있습니다.

* [6장 - AEM Publish에서 JSON으로 콘텐츠 노출](./chapter-6.md)
