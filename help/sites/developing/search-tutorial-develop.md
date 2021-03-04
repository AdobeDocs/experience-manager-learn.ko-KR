---
title: 간단한 검색 구현 안내서
description: 단순 검색 구현은 2017 Summit 랩 AEM Search Demified의 자료입니다. 이 페이지에는 이 랩의 자료가 포함되어 있습니다. 랩을 둘러보려면 이 페이지의 프레젠테이션 섹션에서 Lab 통합 문서를 확인하십시오.
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
feature: 검색
topic: 개발
role: 개발자
level: 중간, 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 2%

---


# 단순 검색 구현 안내서{#simple-search-implementation-guide}

단순 검색 구현은 **Adobe Summit lab AEM 검색 Search Demified**&#x200B;의 자료입니다. 이 페이지에는 이 랩의 자료가 포함되어 있습니다. 랩을 둘러보려면 이 페이지의 프레젠테이션 섹션에서 Lab 통합 문서를 확인하십시오.

![검색 아키텍처 개요](assets/l4080/simple-search-application.png)

## 프레젠테이션 자료 {#bookmarks}

* [랩 통합 문서](assets/l4080/l4080-lab-workbook.pdf)
* [프레젠테이션](assets/l4080/l4080-presentation.pdf)

## 책갈피 {#bookmarks-1}

### 도구 {#tools}

* [색인 관리자](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [쿼리 설명](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder 디버거](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak 색인 정의 생성기](https://oakutils.appspot.com/generate/index)

### 챕터 {#chapters}

*아래의 장 링크는 AEM 작성자 [에 초기 ](#initialpackages) 패키지가 설치된다고 가정합니다.`http://localhost:4502`*

* [제1장](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [제2장](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [제3장](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [제4장](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [5장](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [6장](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [7장](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [8장](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [9장](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## 패키지 {#packages}

### 초기 패키지 {#initial-packages}

* [태그](assets/l4080/summit-tags.zip)
* [간단한 검색 응용 프로그램 패키지](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 장 패키지 {#chapter-packages}

* [1장 솔루션](assets/l4080/l4080-chapter1.zip)
* [제2장 해결 방법](assets/l4080/l4080-chapter2.zip)
* [3장 솔루션](assets/l4080/l4080-chapter3.zip)
* [4장 솔루션](assets/l4080/l4080-chapter4.zip)
* [5장 설정](assets/l4080/l4080-chapter5-setup.zip)
* [5장 솔루션](assets/l4080/l4080-chapter5-solution.zip)
* [6장 솔루션](assets/l4080/l4080-chapter6.zip)
* [9장 솔루션](assets/l4080/l4080-chapter9.zip)

## 참조된 자료 {#reference-materials}

* [Github 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)
* [Sling 모델 내보내기](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://docs.adobe.com/docs/en/aem/6-2/develop/search/querybuilder-api.html)
* [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([설명서 페이지](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 수정 및 후속 작업 {#corrections-and-follow-up}

랩 토론에서의 수정 및 명확히, 참석자의 후속 질문에 대한 답변.

1. **다시 색인화를 중지하는 방법?**

   다시 색인화는 [AEM 웹 콘솔 > JMX](http://localhost:4502/system/console/jmx)을 통해 사용할 수 있는 IndexStats MBean을 통해 중지할 수 있습니다.

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 다시 색인화를 중단하려면 `abortAndPause()`을 실행합니다. 이렇게 하면 `resume()`이(가) 호출될 때까지 색인을 다시 색인화하도록 잠글 수 있습니다.
      * `resume()`을(를) 실행하면 인덱싱 프로세스가 다시 시작됩니다.
   * 설명서:[https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **오크 색인은 여러 세입자를 어떻게 지원할 수 있습니까?**

   Oak는 컨텐츠 트리를 통해 색인 배치를 지원하며 이러한 인덱스는 해당 하위 트리 내에만 색인화됩니다. 예를 들어 **`/content/site-a/oak:index/cqPageLucene`**&#x200B;은(는) **`/content/site-a`아래에만 컨텐츠를 인덱싱하도록 만들 수 있습니다.**

   동일한 접근 방식은 **`/oak:index`** 아래의 인덱스에서 **`includePaths`** 및 **`queryPaths`** 속성을 사용하는 것입니다. 예:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   이 접근 방식의 고려 사항은 다음과 같습니다.

   * 쿼리는 인덱스의 쿼리 경로 범위와 같은 경로 제한을 지정하거나 페이지의 하위 항목이 되어야 합니다.
   * 보다 광범위한 범위 색인(예: `/oak:index/cqPageLucene`)은 데이터를 인덱싱하여 중복 통합 및 디스크 사용 비용을 초래합니다.
   * 중복 구성 관리가 필요할 수 있습니다(예: 동일한 쿼리 세트를 충족해야 하는 경우 여러 임차인 색인에 동일한 indexRules 추가)
   * 이 방법은 사용자 지정 사이트 검색을 위한 AEM 게시 계층에서 가장 잘 제공되므로, AEM 작성자와 같이 서로 다른 임차인의 컨텐츠 트리 상단에서 쿼리를 실행하는 것이 일반적입니다(예: OmniSearch를 통해). 서로 다른 색인 정의는 경로 제한에 따라 다른 행동을 초래할 수 있습니다.


3. **사용 가능한 모든 Analytics 목록은 어디에 있습니까?**

   Oak는 AEM에서 사용할 수 있도록 lucene 제공 분석기 구성 요소 집합을 표시합니다.

   * [Apache Oak Analyzers 설명서](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [토케니저](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [필터](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **동일한 쿼리에서 페이지 및 자산을 검색하는 방법**

   AEM 6.3에서는 동일한 쿼리에서 여러 노드 유형을 쿼리하는 기능이 새로 도입되었습니다. 다음 QueryBuilder 쿼리 각 &quot;sub-query&quot;는 자체 인덱스로 확인할 수 있으므로 이 예에서는 `cq:Page` 하위 쿼리가 `/oak:index/cqPageLucene`으로 확인되고 `dam:Asset` 하위 쿼리는 `/oak:index/damAssetLucene`으로 확인됩니다.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   결과 쿼리 및 쿼리 계획은 다음과 같습니다.

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   [QueryBuilder 디버거](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) 및 [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)을 통해 쿼리 및 결과를 탐색할 수 있습니다.

5. **동일한 쿼리에서 여러 경로를 검색하는 방법**

   AEM 6.3의 새로운 기능은 동일한 쿼리에서 여러 경로를 쿼리하는 기능입니다. 다음 QueryBuilder 쿼리 각 &quot;하위 쿼리&quot;는 자체 색인으로 확인될 수 있습니다.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   다음 쿼리 및 쿼리 계획의 결과

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   [QueryBuilder 디버거](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) 및 [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)을 통해 쿼리 및 결과를 탐색할 수 있습니다.
