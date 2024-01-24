---
title: AEM Assets에서 스마트 번역 검색 사용
description: 스마트 번역 검색을 사용하면 에셋 및 페이지 모두에서 AEM 콘텐츠 간에 언어 간 검색 및 검색을 자동으로 수행할 수 있으므로 50개 이상의 언어를 지원하고 수동 콘텐츠 번역의 필요성을 줄일 수 있습니다.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 195
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# AEM Assets에서 스마트 번역 검색 사용{#using-smart-translation-search-with-aem-assets}

스마트 번역 검색을 사용하면 에셋 및 페이지 모두에서 AEM 콘텐츠 간에 언어 간 검색 및 검색을 자동으로 수행할 수 있으므로 50개 이상의 언어를 지원하고 수동 콘텐츠 번역의 필요성을 줄일 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM Smart Translation Search를 사용하면 영어 이외의 용어를 사용하여 AEM의 컨텐츠를 검색하고 해당 영어 용어가 동등한 AEM의 에셋과 일치시킬 수 있습니다.

스마트 번역 검색은 영어로 된 자산에 적용되는 AEM 스마트 태그에 대한 완벽한 보완입니다.

이 비디오는 [AEM 스마트 번역 검색](smart-translation-search-technical-video-setup.md) 이(가) 설정되었습니다.

## 스마트 번역 검색 작동 방식 {#how-smart-translation-search-works}

![스마트 번역 검색 흐름 다이어그램](assets/smart-translation-search-flow.png)

1. AEM 사용자는 전체 텍스트 검색을 수행하여 지역화된 검색어(예: &#39;남자&#39;, &#39;홈브레&#39;)라는 스페인어 용어.
2. Apache Oak Machine Translation OSGi 번들에서 제공하는 스마트 번역 검색은 제공된 검색어가 등록된 언어 팩을 사용하여 번역될 수 있는지 평가하고 사용됩니다.
3. #2단계에서 번역된 모든 용어가 수집되고 쿼리가 내부적으로 증가하여 검색어로 포함됩니다. 관련 일치 항목을 찾는 AEM 검색 인덱스에 대해 정상적으로 평가되는 경우 이러한 증강된 검색어 세트.
4. 원래 용어(&#39;hombre&#39;) 또는 번역된 용어(&#39;man&#39;)와 일치하는 검색 결과가 수집되어 사용자에게 검색 결과로 반환됩니다.

## 추가 리소스{#additional-resources}

* [AEM Assets을 사용하여 스마트 번역 검색 설정](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
