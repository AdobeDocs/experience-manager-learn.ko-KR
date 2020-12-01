---
title: AEM 경험 조각으로 소셜 게시 설정
description: 경험 조각을 통해 마케터는 AEM에서 만든 경험을 소셜 미디어 플랫폼에 게시할 수 있습니다. 아래 비디오에서는 경험 조각을 Facebook 및 Pinterest에 게시하는 데 필요한 설정 및 구성에 대해 자세히 설명합니다.
sub-product: 사이트, 컨텐츠 서비스
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# 경험 조각 {#set-up-social-posting-with-experience-fragments}을(를) 사용하여 소셜 게시 설정

경험 조각을 통해 마케터는 AEM에서 만든 경험을 소셜 미디어 플랫폼에 게시할 수 있습니다. 아래 비디오에서는 경험 조각을 Facebook 및 Pinterest에 게시하는 데 필요한 설정 및 구성에 대해 자세히 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[경험 조각]  - Facebook 및 Pinterest에 소셜 게시물을 설정하는 설정 및 구성*

## Facebook 및 Pinterest에 게시하도록 경험 조각을 구성하는 검사 목록

1. HTTPS에서 AEM 작성자 인스턴스가 실행 중입니다.
2. Facebook 계정 + Facebook 개발자 앱
3. Pinterest 계정 + Pinterest 개발자 앱
4. [!UICONTROL AEM Cloud ] 서비스 구성 - Facebook
5. [!UICONTROL AEM Cloud ] 서비스 구성 - Pinterest
6. Facebook + Pinterest용 Cloud Services이 있는 AEM 경험 조각
7. Facebook 템플릿을 사용한 경험 조각 변형
8. Pinterest 템플릿을 사용한 경험 조각 변형

## 경험 조각 리디렉션 URI

이 URI는 Oauth 흐름의 일부로 Facebook 및 Pinterest 앱에 사용됩니다.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

