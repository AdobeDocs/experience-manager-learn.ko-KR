---
title: AEM as a Cloud Service의 순회 경고
description: AEM as a Cloud Service의 순회 경고를 완화하는 방법을 알아봅니다.
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
source-git-commit: 45061581322e23efb936e91c11be48ceac64183b
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 5%

---


# 순회 경고

_순회 경고란 무엇입니까?_

순회 경고는 다음과 같습니다 __aemerror__ AEM Publish 서비스에서 실행 중인 쿼리가 제대로 수행되지 않음을 나타내는 로그 명령문입니다. 순회 경고는 일반적으로 AEM에서 다음 두 가지 방법으로 표시됩니다.

1. __느린 쿼리__ 인덱스를 사용하지 않으므로 응답 시간이 느려집니다.
1. __실패한 쿼리__: `RuntimeNodeTraversalException`로 설정되면 경험이 중단됩니다.

순회 경고를 선택 취소하도록 허용하면 AEM 성능이 느려지고 사용자에게 잘못된 경험이 발생할 수 있습니다.

## 순회 경고를 해결하는 방법

순회 경고를 완화하려면 다음 세 가지 간단한 단계를 사용하여 액세스할 수 있습니다. 분석, 조정 및 확인 최적의 조정을 식별하기 전에 몇 가지 반복 조정 및 확인을 수행합니다.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="분석" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="분석">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">문제 분석</p>
               <p class="is-size-6">검색되는 쿼리를 식별하고 이해합니다.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">분석</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="조정" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="조정">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">코드 또는 구성 조정</p>
               <p class="is-size-6">쿼리 순회를 방지하기 위해 쿼리 및 인덱스를 업데이트합니다.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">조정</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="확인" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="확인">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">조정이 작동했는지 확인</p>                       
               <p class="is-size-6">쿼리 및 색인에 대한 변경 내용을 확인하면 순번이 제거됩니다.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">확인</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. 분석{#analyze}

먼저 순회 경고를 표시하는 AEM 게시 서비스를 식별합니다. 이렇게 하려면 Cloud Manager에서 다음을 수행합니다. [게시 서비스 다운로드 `aemerror` 로그](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager)과거 모든 환경(개발, 스테이지 및 프로덕션)의 {target=&quot;_blank&quot;} __3일__.

![AEM as a Cloud Service 로그 다운로드](./assets/traversals/download-logs.jpg)

로그 파일을 열고 Java™ 클래스를 검색합니다 `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. 순회 경고가 포함된 로그에 다음과 유사한 일련의 문이 포함됩니다.

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

쿼리 실행 컨텍스트에 따라 로그 명령문에 쿼리 작성자에 대한 유용한 정보가 포함될 수 있습니다.

+ 쿼리 실행과 연결된 HTTP 요청 URL

   + 예: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak 쿼리 구문

   + 예: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath 쿼리

   + 예: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ 쿼리를 실행하는 코드

   + 예:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__실패한 쿼리__ 다음에 `RuntimeNodeTraversalException` 문, 다음과 비슷합니다.

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. 조정{#adjust}

잘못된 쿼리 및 해당 호출 코드가 검색되면 조정을 수행해야 합니다. 순회 경고를 줄이기 위해 두 가지 유형의 조정을 수행할 수 있습니다.

### 쿼리 조정

__쿼리 변경__ 기존 인덱스 제한 사항으로 확인되는 새 쿼리 제한을 추가하려면 가능하면 쿼리를 변경하여 인덱스를 변경하는 것이 좋습니다.

+ [쿼리 성능을 조정하는 방법 알아보기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}

### 인덱스 조정

__AEM 인덱스 변경(또는 생성)__ 이렇게 하면 기존 쿼리 제한이 인덱스 업데이트로 해결될 수 있습니다.

+ [기존 인덱스를 조정하는 방법 알아보기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}
+ [인덱스를 만드는 방법 알아보기](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target=&quot;_blank&quot;}

## 3. 확인{#verify}

쿼리, 인덱스 또는 둘 다 중 하나에 대한 조정을 확인해야 순회 경고를 줄일 수 있습니다.

![쿼리 설명](./assets/traversals/verify.gif)

다음의 경우에만 [질의 조정](#adjust-the-query) 이렇게 하면 개발자 콘솔을 통해 AEM as a Cloud Service에서 쿼리를 직접 테스트할 수 있습니다. [쿼리 설명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}. AEM 작성자 서비스에 대해 쿼리 설명 실행이지만, 작성 및 게시 서비스에서 색인 정의가 동일하므로 AEM 작성자 서비스에 대한 쿼리 유효성 검사가 충분합니다.

If [색인에 대한 조정](#adjust-the-index) 이(가) 만들어지면 인덱스를 AEM as a Cloud Service에 배포해야 합니다. 인덱스 조정을 배포하면 개발자 콘솔의 [쿼리 설명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;} 를 사용하여 쿼리를 추가로 실행하고 조정할 수 있습니다.

궁극적으로, 모든 변경 사항(쿼리 및 코드)은 Git에 커밋되고 Cloud Manager를 사용하여 AEM as a Cloud Service에 배포됩니다. 배포되면 원래 순회 경고와 연결된 코드 경로를 다시 테스트하고 순회 경고가 더 이상 `aemerror` 로그.

## 기타 리소스

AEM 인덱스, 검색 및 순회 경고를 이해하려면 이러한 다른 유용한 리소스를 확인하십시오.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - 검색 및 색인 지정" tabindex="-1"><img class="is-bordered-r-small" src="../../../cloud-5/imgs/009-thumb.png" alt="Cloud 5 - 검색 및 색인 지정"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - 검색 및 색인 지정">Cloud 5 - 검색 및 색인 지정</a></p>
               <p class="is-size-6">Cloud 5 팀에서는 AEM as a Cloud Service에서 검색 및 색인 생성에 대한 세부 사항을 살펴봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="콘텐츠 검색 및 색인 지정" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="콘텐츠 검색 및 색인 지정">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="콘텐츠 검색 및 색인 지정">콘텐츠 검색 및 색인 지정 설명서</a></p>
               <p class="is-size-6">AEM as a Cloud Service에서 인덱스를 만들고 관리하는 방법을 알아봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Oak 색인 현대화" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Oak 색인 현대화">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Oak 색인 현대화">Oak 색인 현대화</a></p>
               <p class="is-size-6">AEM 6 Oak 색인 정의를 AEM as a Cloud Service 호환으로 변환하고 인덱스를 계속 유지하는 방법을 알아봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="색인 정의 설명서" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="색인 정의 설명서">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="색인 정의 설명서">Lucene 색인 설명서</a></p>
               <p class="has-ellipsis is-size-6">Apache Oak Jackrabbit Lucene 색인 참조는 지원되는 모든 Lucene 인덱스 구성을 문서화하는 데 사용됩니다.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>


