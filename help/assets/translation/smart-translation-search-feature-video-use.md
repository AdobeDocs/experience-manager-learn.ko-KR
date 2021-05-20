---
title: AEM Assets에서 스마트 번역 검색 사용
description: Smart Translation Search를 사용하면 Assets와 Pages에서 AEM 컨텐츠 간에 자동으로 교차 언어 검색 및 검색을 수행할 수 있으므로 50개 이상의 언어를 지원하며 수동 컨텐츠 번역도 필요하지 않습니다.
version: 6.3, 6.4, 6.5
feature: 검색
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# AEM Assets{#using-smart-translation-search-with-aem-assets}에서 스마트 번역 검색 사용

Smart Translation Search를 사용하면 Assets와 Pages에서 AEM 컨텐츠 간에 자동으로 교차 언어 검색 및 검색을 수행할 수 있으므로 50개 이상의 언어를 지원하며 수동 컨텐츠 번역도 필요하지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Smart Translation Search를 사용하면 영어 이외의 용어를 사용하여 AEM에서 컨텐츠를 검색할 수 있으며, 영어 용어가 동일한 AEM의 자산과 일치합니다.

스마트 번역 검색은 영어로 된 자산에 적용되는 AEM 스마트 태그를 완벽하게 보완합니다.

이 비디오에서는 [AEM 스마트 번역 검색](smart-translation-search-technical-video-setup.md)이 설정되었다고 가정합니다.

## 스마트 번역 검색 작동 방식 {#how-smart-translation-search-works}

![스마트 번역 검색 흐름 다이어그램](assets/smart-translation-search-flow.png)

1. AEM 사용자는 전체 텍스트 검색을 수행하여 현지화된 검색어(예: &#39;남자&#39;, &#39;음색&#39;) 스페인어.
2. Apache Oak Machine Translation OSGi 번들에서 제공되는 스마트 번역 검색은 등록된 언어 팩을 사용하여 제공된 검색어를 변환할 수 있는지 여부를 평가하고 참여합니다.
3. 단계#2 번역된 모든 용어가 수집되고 검색어가 검색어로 포함되도록 쿼리가 내부적으로 강화됩니다. 관련 일치 항목을 찾는 AEM 검색 인덱스에 대해 정상적으로 평가되는 경우 이 증강된 검색어 세트입니다.
4. 원래 용어(&#39;hombre&#39;) 또는 번역어(&#39;man&#39;)와 일치하는 검색 결과가 수집되어 검색 결과로 반환됩니다.

## 추가 리소스{#additional-resources}

* [AEM Assets을 사용하여 스마트 번역 검색 설정](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)