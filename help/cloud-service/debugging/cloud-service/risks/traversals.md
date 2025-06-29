---
title: AEM as a Cloud Service의 순회 경고
description: AEM as a Cloud Service에서 순회 경고를 완화하는 방법에 대해 알아봅니다.
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
duration: 155
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 4%

---

# 순회 경고

>[!TIP]
>나중에 참조할 수 있도록 이 페이지에 책갈피를 표시합니다.

_순회 경고란 무엇입니까?_

순회 경고는 AEM Publish 서비스에서 성능이 낮은 쿼리가 실행되고 있음을 나타내는 __aemerror__ 로그 문입니다. 순회 경고는 일반적으로 두 가지 방법으로 AEM에서 나타납니다.

1. 인덱스를 사용하지 않는 __느린 쿼리__&#x200B;로 인해 응답 시간이 느려집니다.
1. __쿼리 실패__, `RuntimeNodeTraversalException`을(를) throw하여 경험이 중단됩니다.

순회 경고를 선택 취소하도록 허용하면 AEM 성능이 저하되고 사용자의 경험이 중단될 수 있습니다.

## 순회 경고를 해결하는 방법

순회 경고 완화는 분석, 조정 및 확인의 세 가지 간단한 단계를 사용하여 접근할 수 있습니다. 최적의 조정을 식별하기 전에 조정 및 확인 작업을 여러 번 반복해야 합니다.

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
               <p class="is-size-6">어떤 쿼리가 트래버스되는지 식별하고 이해합니다.</p>
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
               <p class="is-size-6">쿼리 및 인덱스를 업데이트하여 쿼리 트래버스를 방지합니다.</p>
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
                <p class="headline is-size-5 has-text-weight-bold">조정된 작업 확인</p>                       
               <p class="is-size-6">쿼리 및 색인의 변경 사항을 확인하여 순회를 제거합니다.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">확인</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. 분석{#analyze}

먼저 순회 경고를 표시하는 AEM Publish 서비스를 식별합니다. 이렇게 하려면 Cloud Manager에서 지난 __3일__ 동안 모든 환경(개발, 스테이지 및 프로덕션)에서 [Publish 서비스의 `aemerror` 로그를 다운로드하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html?lang=ko#cloud-manager){target="_blank"}.

![AEM as a Cloud Service 로그 다운로드](./assets/traversals/download-logs.jpg)

로그 파일을 열고 Java™ 클래스 `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`을(를) 검색합니다. 순회 경고가 포함된 로그에는 다음과 유사한 일련의 문이 포함되어 있습니다.

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

쿼리 실행의 컨텍스트에 따라 로그 문에는 쿼리 작성자에 대한 유용한 정보가 포함될 수 있습니다.

+ 쿼리 실행과 연계된 HTTP 요청 URL

   + 예: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak 쿼리 구문

   + 예: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath 쿼리

   + 예: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ 쿼리를 실행하는 코드

   + 예: `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__실패한 쿼리__ 다음에 다음과 유사한 `RuntimeNodeTraversalException` 문이 옵니다.

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. 조정{#adjust}

문제가 되는 쿼리와 해당 호출 코드가 검색되면 조정해야 합니다. 순회 경고를 완화하기 위해 두 가지 유형의 조정을 수행할 수 있습니다.

### 쿼리 조정

__쿼리를 변경__&#x200B;하여 기존 색인 제한으로 확인되는 새 쿼리 제한을 추가합니다. 가능하면 인덱스 변경보다 쿼리 변경을 선호합니다.

+ [쿼리 성능을 조정하는 방법을 알아봅니다](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=ko#query-performance-tuning){target="_blank"}

### 색인 조정

__기존 쿼리 제한 사항을 색인 업데이트로 해결할 수 있도록 AEM 색인을 변경(또는 만들기)합니다__.

+ [기존 색인을 조정하는 방법을 알아봅니다](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=ko#query-performance-tuning){target="_blank"}
+ [인덱스를 만드는 방법을 알아보세요](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=ko#create-a-new-index){target="_blank"}

## 3. 확인{#verify}

쿼리, 색인 또는 두 가지 모두에 대한 조정을 확인하여 통과 경고를 완화해야 합니다.

![쿼리 설명](./assets/traversals/verify.gif)

[쿼리를 조정](#adjust-the-query)하는 경우에만 Developer Console의 [쿼리 설명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=ko#queries){target="_blank"}을 통해 AEM as a Cloud Service에서 직접 쿼리를 테스트할 수 있습니다. 그러나 AEM Author 서비스에 대한 쿼리 실행 설명 색인 정의는 Author 및 Publish 서비스에서 동일하기 때문에 AEM Author 서비스에 대한 쿼리의 유효성 검사로 충분합니다.

[인덱스를 조정](#adjust-the-index)한 경우 인덱스를 AEM as a Cloud Service에 배포해야 합니다. 색인 조정이 배포되면 Developer Console의 [쿼리 설명](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=ko#queries){target="_blank"}을(를) 사용하여 쿼리를 실행하고 추가로 조정할 수 있습니다.

궁극적으로 모든 변경 사항(쿼리 및 코드)은 Git에 커밋되고 Cloud Manager을 사용하여 AEM as a Cloud Service에 배포됩니다. 배포되면 원래 순회 경고와 연결된 코드 경로를 다시 테스트하고 `aemerror` 로그에 더 이상 순회 경고가 표시되지 않는지 확인하십시오.

## 기타 리소스

AEM 인덱스, 검색 및 순회 경고를 이해하는 데 유용한 기타 리소스를 확인하십시오.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=ko" title="Cloud 5 - 검색 및 색인화" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5 - 검색 및 색인화"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=ko" title="Cloud 5 - 검색 및 색인화">Cloud 5 - 검색 및 색인화</a></p>
               <p class="is-size-6">Cloud 5 팀은 AEM as a Cloud Service에서 검색 및 색인화의 기능을 자세히 살펴봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=ko" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=ko" title="콘텐츠 검색 및 색인화" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="콘텐츠 검색 및 색인화">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=ko" title="콘텐츠 검색 및 색인화">콘텐츠 검색 및 색인화 설명서</a></p>
               <p class="is-size-6">AEM as a Cloud Service에서 색인을 만들고 관리하는 방법을 알아봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=ko" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=ko" title="Oak 인덱스 현대화" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Oak 인덱스 현대화">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=ko" title="Oak 인덱스 현대화">Oak 인덱스 현대화</a></p>
               <p class="is-size-6">AEM 6 Oak 색인 정의를 AEM as a Cloud Service과 호환되도록 변환하고 향후 색인을 유지 관리하는 방법에 대해 알아봅니다.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=ko" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
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
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="색인 정의 설명서">Lucene 인덱스 설명서</a></p>
               <p class="has-ellipsis is-size-6">Apache Oak Jackrabbit Lucene 색인 참조는 지원되는 모든 Lucene 색인 구성을 문서화합니다.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
