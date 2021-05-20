---
title: AEM 컨텐츠 조각에 대한 번역 지원
description: 컨텐츠 조각을 Adobe Experience Manager으로 현지화하고 변환하는 방법을 알아봅니다. 컨텐츠 조각과 연관된 혼합 미디어 자산도 추출 및 변환할 수 있습니다.
feature: 컨텐츠 조각, 다중 사이트 관리자
topic: 로컬라이제이션
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# AEM 컨텐츠 조각에 대한 번역 지원 {#translation-support-content-fragments}

컨텐츠 조각을 Adobe Experience Manager으로 현지화하고 변환하는 방법을 알아봅니다. 컨텐츠 조각과 연관된 혼합 미디어 자산도 추출 및 변환할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## 컨텐츠 조각 번역 사용 사례 {#content-fragment-translation-use-cases}

컨텐츠 조각은 AEM 추출이 외부 번역 서비스로 전송되도록 인식되는 컨텐츠 유형입니다. 몇 가지 사용 사례가 즉시 지원됩니다.

1. 언어 복사 및 번역을 위해 자산 콘솔에서 직접 컨텐츠 조각을 선택할 수 있습니다
2. 사이트 페이지에서 참조되는 컨텐츠 조각은 해당 언어 폴더에 복사되며 언어 사본 을 위해 사이트 페이지를 선택하면 번역용으로 추출됩니다
3. 컨텐츠 조각 내에 포함된 인라인 미디어 자산은 추출 및 변환할 수 있습니다.
4. 컨텐츠 조각과 연관된 자산 컬렉션은 추출 및 변환할 수 있습니다

## 번역 규칙 편집기 {#translation-rules-editor}

Experience Manager 번역 동작은 **번역 규칙 편집기**&#x200B;를 사용하여 업데이트할 수 있습니다. 번역을 업데이트하려면 **도구** > **일반** > **번역 구성** http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)으로 이동합니다.[

기본 구성은 리소스 유형이 `core/wcm/components/contentfragment/v1/contentfragment`인 `fragmentPath`에서 컨텐츠 조각을 참조합니다. `v1/contentfragment`에서 상속되는 모든 구성 요소는 기본 구성으로 인식됩니다.

![번역 규칙 편집기](assets/translation-configuration.png)
