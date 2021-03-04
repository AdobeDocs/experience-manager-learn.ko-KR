---
title: AEM Assets에서 고급 번역 검색 사용
description: Smart Translation Search를 사용하면 AEM 컨텐츠(Assets 및 Pages) 전체에서 자동으로 언어 간 검색을 수행할 수 있으므로 50개 이상의 언어를 지원하고 수동 컨텐츠 번역 필요성을 줄일 수 있습니다.
version: 6.3, 6.4, 6.5
feature: 검색
topic: 컨텐츠 관리
role: 비즈니스 전문가
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# AEM Assets{#using-smart-translation-search-with-aem-assets}에서 스마트 번역 검색 사용

Smart Translation Search를 사용하면 AEM 컨텐츠(Assets 및 Pages) 전체에서 자동으로 언어 간 검색을 수행할 수 있으므로 50개 이상의 언어를 지원하고 수동 컨텐츠 번역 필요성을 줄일 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Smart Translation Search를 사용하면 AEM에서 영어 용어와 동등한 영어 단어가 포함된 자산과 일치하도록 영어 이외의 용어를 사용하여 컨텐츠를 검색할 수 있습니다.

고급 번역 검색은 영어로 된 자산에 적용되는 AEM 스마트 태그를 완벽하게 보완합니다.

이 비디오는 [AEM 스마트 번역 검색](smart-translation-search-technical-video-setup.md)이(가) 설정되었다고 가정합니다.

## 고급 번역 검색 작동 방식 {#how-smart-translation-search-works}

![스마트 번역 검색 흐름 다이어그램](assets/smart-translation-search-flow.png)

1. AEM 사용자는 현지화된 검색어(예: 스페인어 용어 &#39;남자&#39;, &#39;홈&#39;).
2. Apache Oak Machine Translation OSGi 번들에서 제공되는 스마트 번역 검색은 등록된 언어 팩을 사용하여 제공된 검색어를 변환할 수 있는지 여부를 참여 및 평가합니다.
3. 단계 #2에서 번역된 모든 용어가 수집되고 검색어로 포함하도록 쿼리가 내부적으로 향상되었습니다. 관련 일치 항목을 찾는 AEM 검색 색인에 대해 정상적으로 평가되는 경우 이 증강 검색 용어 집합.
4. 원래 용어(&#39;hombre&#39;) 또는 번역어(&#39;man&#39;)와 일치하는 검색 결과가 수집되어 사용자를 검색 결과로 반환합니다.

## 추가 리소스{#additional-resources}

* [AEM Assets을 사용하여 고급 번역 검색 설정](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)