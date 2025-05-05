---
title: 단순 검색 구현 안내서
description: 단순 검색 구현은 2017년 Summit 랩 AEM Search Demystified의 자료입니다. 이 페이지에는 이 실습의 자료가 포함되어 있습니다. 실습 가이드 투어는 이 페이지의 프레젠테이션 섹션에서 실습 통합 문서를 참조하십시오.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---

# 단순 검색 구현 안내서{#simple-search-implementation-guide}

단순 검색 구현은 **Adobe Summit 랩 AEM 검색 Demystified**&#x200B;의 자료입니다. 이 페이지에는 이 실습의 자료가 포함되어 있습니다. 실습 가이드 투어는 이 페이지의 프레젠테이션 섹션에서 실습 통합 문서를 참조하십시오.

![검색 아키텍처 개요](assets/l4080/simple-search-application.png)

## 프레젠테이션 자료 {#bookmarks}

* [랩 워크북](assets/l4080/l4080-lab-workbook.pdf)
* [프레젠테이션](assets/l4080/l4080-presentation.pdf)

## 책갈피 {#bookmarks-1}

### 도구 {#tools}

* [인덱스 관리자](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [쿼리 설명](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder 디버거](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak 인덱스 정의 생성기](https://oakutils.appspot.com/generate/index)

### 챕터 {#chapters}

*아래 챕터 링크는 [초기 패키지](#initialpackages)가`http://localhost:4502`*&#x200B;에 AEM 작성자에 설치되어 있다고 가정합니다.

* [챕터 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [챕터 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [챕터 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [챕터 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [챕터 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [챕터 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [챕터 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [챕터 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [챕터 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## 패키지 {#packages}

### 초기 패키지 {#initial-packages}

* [태그](assets/l4080/summit-tags.zip)
* [단순 검색 애플리케이션 패키지](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 챕터 패키지 {#chapter-packages}

* [1장 솔루션](assets/l4080/l4080-chapter1.zip)
* [제2장 해결](assets/l4080/l4080-chapter2.zip)
* [3장 해결](assets/l4080/l4080-chapter3.zip)
* [제4장 해결](assets/l4080/l4080-chapter4.zip)
* [5장 설정](assets/l4080/l4080-chapter5-setup.zip)
* [제5장 해결](assets/l4080/l4080-chapter5-solution.zip)
* [6장 해결](assets/l4080/l4080-chapter6.zip)
* [제9장 해결](assets/l4080/l4080-chapter9.zip)

## 참조된 자료 {#reference-materials}

* [Github 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)
* [슬링 모델 내보내기](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/?lang=ko)
* [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)&#x200B;([설명서 페이지](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 수정 및 추가 작업 {#corrections-and-follow-up}

랩 토론의 수정 및 설명, 참석자의 후속 질문에 대한 답변.

1. **다시 인덱싱을 중지하는 방법**

   [AEM 웹 콘솔 > JMX](http://localhost:4502/system/console/jmx)에서 사용할 수 있는 IndexStats MBean을 통해 다시 인덱싱을 중지할 수 있습니다.

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 다시 인덱싱을 중단하려면 `abortAndPause()`을(를) 실행하십시오. `resume()`이(가) 호출될 때까지 색인을 추가 다시 색인화하도록 잠급니다.
      * `resume()`을(를) 실행하면 인덱싱 프로세스가 다시 시작됩니다.
   * 설명서: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **oak 인덱스가 여러 테넌트를 지원하는 방법은 무엇입니까?**

   Oak은 콘텐츠 트리 전체에 인덱스 배치를 지원하며 이러한 인덱스는 해당 하위 트리 내에서만 인덱싱됩니다. 예를 들어 **`/content/site-a/oak:index/cqPageLucene`**&#x200B;은(는) **`/content/site-a`.**&#x200B;에서만 콘텐츠를 색인화하기 위해 만들 수 있습니다.

   이와 동등한 방법은 **`/oak:index`** 아래 인덱스에서 **`includePaths`** 및 **`queryPaths`** 속성을 사용하는 것입니다. 예:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   이 접근 방식의 고려 사항은 다음과 같습니다.

   * 쿼리는 인덱스의 쿼리 경로 범위와 동일한 경로 제한을 지정하거나 해당 인덱스의 하위 항목이어야 합니다.
   * 범위가 넓은 인덱스(예: `/oak:index/cqPageLucene`)도 데이터를 인덱싱하므로 중복 수집 및 디스크 사용 비용이 발생합니다.
   * 중복 구성 관리(예: 동일한 쿼리 세트를 충족해야 하는 경우 여러 테넌트 인덱스에 동일한 indexRules 추가)
   * 이 방법은 AEM Author에서와 같이, 사용자 지정 사이트 검색을 위해 AEM Publish 계층에서 가장 잘 제공되며, 일반적으로 쿼리는 서로 다른 테넌트에 대한 콘텐츠 트리 상향에서 실행됩니다(예: OmniSearch를 통해). 서로 다른 인덱스 정의로 인해 경로 제한만 기반으로 다른 동작이 발생할 수 있습니다.

3. **사용 가능한 모든 분석기 목록은 어디에 있습니까?**

   Oak은 AEM에서 사용하기 위해 lucene 제공 분석기 구성 요소 세트를 노출합니다.

   * [Apache Oak 분석기 설명서](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [토큰라이저](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [필터](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [문자 필터](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **같은 쿼리에서 페이지와 Assets을 검색하는 방법**

   AEM 6.3의 새로운 기능은 제공된 동일한 쿼리에서 여러 노드 유형을 쿼리하는 기능입니다. 다음 QueryBuilder 쿼리입니다. 각 &quot;하위 쿼리&quot;는 자체 인덱스로 확인할 수 있으므로 이 예제에서는 `cq:Page` 하위 쿼리가 `/oak:index/cqPageLucene`(으)로 확인되고 `dam:Asset` 하위 쿼리가 `/oak:index/damAssetLucene`(으)로 확인됩니다.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   다음 쿼리 및 쿼리 계획을 생성합니다.

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) 및 [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)을 통해 쿼리 및 결과를 탐색합니다.

5. **동일한 쿼리에서 여러 경로를 검색하는 방법**

   AEM 6.3의 새로운 기능은 제공된 동일한 쿼리에서 여러 경로를 쿼리하는 기능입니다. 다음 QueryBuilder 쿼리입니다. 각 &quot;하위 쿼리&quot;는 자체 색인으로 확인될 수 있습니다.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   결과: 다음 쿼리 및 쿼리 계획

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) 및 [AEM Chrome 플러그인](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)을 통해 쿼리 및 결과를 탐색합니다.
