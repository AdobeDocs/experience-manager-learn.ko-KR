---
title: AEM 컨텐츠 조각과 번역 사용
description: AEM 6.3에서는 컨텐츠 조각을 변환하는 기능을 도입했습니다. 컨텐츠 조각과 연관된 혼합 미디어 자산 및 자산 컬렉션도 추출하여 변환할 수 있습니다.
sub-product: 사이트, 자산, 컨텐츠 서비스
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# AEM 컨텐츠 조각과 번역 사용{#using-translation-with-aem-content-fragments}

AEM 6.3에서는 컨텐츠 조각을 변환하는 기능을 도입했습니다. 컨텐츠 조각과 연관된 혼합 미디어 자산 및 자산 컬렉션도 추출하여 변환할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## 컨텐츠 조각 번역 사용 사례 {#content-fragment-translation-use-cases}

컨텐츠 조각은 외부 번역 서비스로 전송되도록 AEM에서 추출하는 인식된 컨텐츠 유형입니다. 몇 가지 사용 사례가 기본적으로 지원됩니다.

1. 언어 복사 및 번역을 위해 자산 콘솔에서 직접 컨텐츠 조각을 선택할 수 있습니다
2. 사이트 페이지에서 참조되는 컨텐츠 조각은 언어 복사를 위해 사이트 페이지를 선택하면 해당 언어 폴더에 복사되고 번역용으로 추출됩니다
3. 컨텐츠 조각 내에 임베드된 인라인 미디어 에셋은 추출하여 변환할 수 있습니다.
4. 콘텐츠 조각과 연결된 자산 컬렉션은 추출하여 변환할 수 있습니다

## 번역 구성 옵션 {#translation-config-options}

기본적으로 컨텐츠 조각을 변환하는 여러 옵션을 지원합니다. 기본적으로 인라인 미디어 자산 및 관련 자산 컬렉션은 번역되지 않습니다. 번역 구성을 업데이트하려면 [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html)로 이동합니다.

컨텐츠 조각 자산을 번역하기 위한 네 가지 옵션이 있습니다.

1. **번역 안 함(기본값)**
2. **인라인 미디어 자산만**
3. **연결된 자산 컬렉션만**
4. **인라인 미디어 자산 및 연결된 컬렉션**

![번역 구성](assets/classic-ui-dialog.png)
