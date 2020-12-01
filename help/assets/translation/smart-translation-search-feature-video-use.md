---
title: AEM Assets에서 고급 번역 검색 사용
seo-title: AEM Assets에서 고급 번역 검색 사용
description: Smart Translation Search를 사용하면 AEM 컨텐츠에서 에셋과 페이지 모두를 자동으로 언어 간 검색 및 검색할 수 있으므로 50개 이상의 언어를 지원하고 수동 컨텐츠 번역 필요성을 줄일 수 있습니다.
seo-description: Smart Translation Search를 사용하면 AEM 컨텐츠에서 에셋과 페이지 모두를 자동으로 언어 간 검색 및 검색할 수 있으므로 50개 이상의 언어를 지원하고 수동 컨텐츠 번역 필요성을 줄일 수 있습니다.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# AEM Assets에서 고급 변환 검색 사용{#using-smart-translation-search-with-aem-assets}

Smart Translation Search를 사용하면 AEM 컨텐츠에서 에셋과 페이지 모두를 자동으로 언어 간 검색 및 검색할 수 있으므로 50개 이상의 언어를 지원하고 수동 컨텐츠 번역 필요성을 줄일 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Smart Translation Search를 사용하면 AEM에서 영어 용어에 상응하는 에셋을 일치시키기 위해 영어 이외의 용어를 사용하여 AEM에서 컨텐츠를 검색할 수 있습니다.

고급 번역 검색은 영어로 된 자산에 적용되는 AEM 스마트 태그를 완벽하게 보완합니다.

이 비디오에서는 [AEM Smart Translation Search](smart-translation-search-technical-video-setup.md)가 설정되었다고 가정합니다.

## 고급 번역 검색 작동 방식 {#how-smart-translation-search-works}

![고급 번역 검색 흐름 다이어그램](assets/smart-translation-search-flow.png)

1. AEM 사용자는 지역화된 검색어를 제공하는 전체 텍스트 검색을 수행합니다(예). 스페인어 용어 남자, 음색).
2. Apache Oak Machine Translation OSGi 번들로 제공되는 Smart Translation Search는 등록된 언어 팩을 사용하여 제공된 검색어를 번역할 수 있는지 여부를 평가하고 이에 참여합니다.
3. 단계 2에서 번역된 모든 용어가 수집되고 검색어로 포함하도록 쿼리가 내부적으로 향상되었습니다. 관련 일치 항목을 찾는 AEM 검색 색인에 대해 정상적으로 평가한 경우 이 증강 검색 용어 집합.
4. 원래 용어(&#39;홈&#39;) 또는 번역어(&#39;남자&#39;)와 일치하는 검색 결과가 수집되어 사용자를 검색 결과로 반환합니다.

## 추가 리소스{#additional-resources}

* [AEM Assets을 사용하여 고급 번역 검색 설정](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)