---
title: 5장 - 컨텐츠 서비스 페이지 작성 - 컨텐츠 서비스
description: AEM Headless 자습서의 5장에서는 4장에 정의된 템플릿에서 페이지 만들기를 다룹니다. 이러한 페이지는 JSON HTTP 끝점으로 작동합니다.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# 5장 - 컨텐츠 서비스 페이지 작성

AEM Headless 자습서의 5장에서는 4장에 정의된 템플릿에서 페이지를 만드는 방법을 다룹니다. 이 장에서 만드는 페이지는 모바일 앱의 JSON HTTP 끝점으로 작동합니다.

>[!NOTE]
>
> `/content/wknd-mobile/en/api`의 페이지 콘텐츠 아키텍처가 미리 빌드되었습니다. `en` 및 `api`의 기본 페이지는 건축적 및 조직적 용도로 제공되지만 반드시 필요한 것은 아닙니다. API 컨텐츠가 로컬라이즈될 수 있는 경우 API 페이지를 AEM Sites 페이지처럼 현지화할 수 있으므로 일반적인 언어 복사 및 다중 사이트 관리자 페이지 구성 우수 사례를 따르는 것이 좋습니다.

## 이벤트 API 페이지 만들기

1. **[!UICONTROL AEM] > [!UICONTROL 사이트] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;으로 이동합니다.
1. **API 페이지** 레이블을 누른 다음 상단 작업  **** 막대의 [만들기] 단추를 누르고 API 페이지 아래에 새 이벤트 API 페이지를 만듭니다.
   1. 상단 작업 막대에서 **만들기**&#x200B;를 누릅니다.
   1. **이벤트 API** 템플릿 선택
   1. **이름** 필드에 **events**&#x200B;를 입력합니다.
   1. **제목** 필드에 **이벤트 API**&#x200B;를 입력합니다.
   1. 상단 작업 표시줄의 **만들기**&#x200B;를 눌러 페이지를 만듭니다
   1. AEM Sites 관리자로 돌아가려면 **Done**&#x200B;을 누릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 이벤트 API 페이지 작성

>[!NOTE]
>
> 이 프로젝트는 작성 환경에 몇 가지 기본 스타일을 제공하기 위해 CSS를 제공합니다.

1. **Events API** 페이지를 편집합니다. **AEM > Sites > WKND Mobile > English > API** 페이지로 이동하여 **Events API** 페이지를 선택하고 맨 위 작업 모음에서 **Edit**&#x200B;을 탭합니다.
1. 자산 파인더에서 이미지 구성 요소 자리 표시자로 드래그하여 놓아 앱에 표시할 **로고 이미지**&#x200B;를 추가합니다.
   * `/content/dam/wknd-mobile/images/wknd-logo.png`에 있는 제공된 로고를 사용하십시오.

1. 이벤트 위에 표시하려면 **태그 줄**&#x200B;을 추가합니다.
   1. **텍스트** 구성 요소 편집
   1. 텍스트를 입력합니다.
      * `The WKND is here.`

1. 표시할 **events**&#x200B;를 선택합니다.
   1. **속성** 탭에서 다음 구성을 설정합니다.
      * 모델:**Event**
      * 상위 경로:**/content/dam/wknd-mobile/en/events**
      * 태그:**&lt;비워 두십시오>**
   1. **Elements** 탭에서 다음 구성을 설정합니다.
      * 나열된 요소를 제거하면 이벤트 컨텐츠 조각의 모든 요소가 노출됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## API 페이지의 JSON 출력 검토

JSON 출력 및 형식은 `.model.json` 선택기로 페이지를 요청하여 검토할 수 있습니다.

이 JSON 구조(또는 스키마)는 이 API의 소비자가 잘 이해해야 합니다. 이는 API 소비자가 구조의 어떤 측면(예: 이벤트 API의 로고(이미지) 및 태그 라이브(텍스트) 및 유동적인 태그(예: 컨텐츠 조각 목록 구성 요소 아래에 나열된 이벤트).

게시된 API에서 이 계약을 중단하면 많은 시간이 소모되는 앱에서 잘못된 동작이 발생할 수 있습니다.

1. 새 브라우저 탭에서 AEM Content Services의 JSON Exporter를 호출하는 `.model.json` 선택기를 사용하여 이벤트 API 페이지를 요청하고, 페이지 및 구성 요소를 표준화되고 잘 정의된 JSON 구조로 정리합니다.

   이러한 페이지에서 생성된 JSON 구조는 앱에서 정렬해야 하는 구조입니다.

1. **이벤트 API** 페이지를 **JSON**&#x200B;으로 요청합니다.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   결과는 다음과 유사하게 나타납니다.

![AEM Content Services JSON 출력](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 이 JSON은 `.tidy` 선택기를 사용하여 사람이 읽을 수 있도록 **정리**(서식이 지정된) 방식으로 출력할 수 있습니다.
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 다음 단계

원할 경우, [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 통해 AEM 작성자에 설치합니다. 이 패키지에는 자습서의 이전 장과 이 장에 나와 있는 구성 및 컨텐츠가 포함되어 있습니다.

* [6장 - AEM 게시에서 JSON으로 컨텐츠 노출](./chapter-6.md)
