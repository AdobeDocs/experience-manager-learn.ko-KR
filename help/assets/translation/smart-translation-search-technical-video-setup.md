---
title: AEM Assets을 사용하여 스마트 번역 검색 설정
description: 스마트 번역 검색을 사용하면 영어 이외의 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. Smart Translation Search용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성하고 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩을 설치해야 합니다.
version: 6.3, 6.4, 6.5
feature: 검색
topic: 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 0%

---


# AEM Assets{#set-up-smart-translation-search-with-aem-assets}을 사용하여 스마트 번역 검색 설정

스마트 번역 검색을 사용하면 영어 이외의 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. Smart Translation Search용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성하고 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩을 설치해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>스마트 번역 검색은 필요한 각 AEM 인스턴스에 설정해야 합니다.

1. Oak Search Machine Translation OSGi 번들을 다운로드하여 설치합니다
   * [AEM Oak 버전에 해당하는 ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Oak Search Machine Translation OSGi 번들을 다운로드합니다.
   * 다운로드한 Oak Search Machine Translation OSGi 번들을 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles)를 통해 AEM에 설치합니다.

2. Apache Joshua 언어 팩을 다운로드하여 업데이트합니다
   * 원하는 [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)을 다운로드하여 압축을 해제합니다.
   * `joshua.config` 파일을 편집하고 다음으로 시작하는 두 줄에 주석을 답니다.

      ```
      feature-function = LanguageModel ...
      ```

   * 언어 팩의 모델 폴더의 크기를 확인하고 기록합니다. 이는 AEM에 필요한 추가 힙의 공간에 영향을 줍니다.
   * 압축을 푼 Apache Joshua 언어 팩 폴더( `joshua.config` 편집 포함)를 로 이동합니다.

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      예:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 업데이트된 힙메모리 할당으로 AEM을 다시 시작합니다.
   * AEM 중지
   * AEM에 필요한 새 힙크기 확인

      * AEM 사전 언어 부족 heap 크기 + 가장 가까운 2GB로 반올림된 모델 디렉터리의 크기입니다
      * 예:사전 언어 팩인 경우 AEM 설치를 실행하려면 8GB 힙이 필요하며 언어 팩의 모델 폴더는 3.8GB의 압축되지 않은 경우 새 힙의 크기는 다음과 같습니다.

         원본 `8GB` + ( `3.75GB` 총 `12GB`에 대해 가장 가까운 `2GB`(즉, `4GB`)로 반올림됨)
   * 컴퓨터에 사용 가능한 메모리가 충분한지 확인합니다.
   * AEM 시작 스크립트를 업데이트하여 새 힙크기 조정

      * 예. `java -Xmx12g -jar cq-author-p4502.jar`
   * 힙이 증가된 AEM을 다시 시작합니다.

   >[!NOTE]
   >
   >특히 여러 언어 팩을 사용하는 경우 언어 팩에 필요한 힙의 공간이 커질 수 있습니다.
   >
   >
   >항상 **인스턴스에 할당된 heap 공간의 증가를 수용할 충분한 메모리**&#x200B;가 있는지 확인하십시오.
   >
   >
   >**기본 힙은 항상 언어 팩**&#x200B;이 설치되어 있지 않고 허용 가능한 성능을 지원하도록 계산되어야 합니다.

4. Apache Jackrabbit Oak Machine Translation 전체 텍스트 쿼리 용어 공급자 OSGi 구성을 통해 언어 팩을 등록합니다

   * 각 언어 팩의 경우 [AEM Web Console의 구성 관리자를 통해 새로운 Apache Jackrabbit Oak Machine Translation 전체 텍스트 쿼리 용어 공급자 OSGi 구성](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)을 만듭니다.

      * `Joshua Config Path` joshua.config 파일의 절대 경로입니다. AEM 프로세스는 언어 팩 폴더의 모든 파일을 읽을 수 있어야 합니다.
      * `Node types` 전체 텍스트 검색을 통해 이 언어 팩을 번역하는 후보 노드 유형입니다.
      * `Minimum score` 는 번역된 용어에 대한 최소 신뢰 점수입니다.

         * 예를 들어, hombre(&quot;man&quot;의 경우 스페인어)는 영어 단어 &quot;man&quot;으로 변환되고, 신뢰 점수가 `0.9`인 영어 단어 &quot;human&quot;으로 번역할 수 있습니다. 이때 영어 단어 &quot;human&quot;은 신뢰 점수 `0.2`로 변환됩니다. 최소 점수를 `0.3`으로 조정하면 &quot;hombre&quot;가 &quot;man&quot; 번역을 유지하지만, 이 번역 점수가 `0.2`인 최소 점수인 `0.3`보다 작으므로 &#39;hombre&#39;를 &quot;human&quot; 번역을 취소하십시오.

5. 자산에 대해 전체 텍스트 검색 수행
   * dam:Asset은 이 언어 팩이 다시 등록된 노드 유형이므로 전체 텍스트 검색을 사용하여 AEM Assets을 검색해야 유효성을 확인할 수 있습니다.
   * AEM > 자산으로 이동하고 Omnisearch를 엽니다. 언어 팩이 설치된 언어로 용어를 검색합니다.
   * 필요에 따라 OSGi 구성에서 최소 점수 를 조정하여 결과의 정확성을 보장합니다.

6. 언어 팩 업데이트
   * Apache Joshua 언어 팩은 Apache Joshua 프로젝트에 의해 완전히 관리되며, 업데이트 또는 수정은 Apache Joshua 프로젝트의 재량입니다.
   * 언어 팩이 업데이트되는 경우 AEM에 업데이트를 설치하려면 위의 2~4단계를 수행하여 필요한 경우 힙의 크기를 위아래로 조정해야 합니다.

      * 압축을 푼 언어 팩을 crx-quickstart/opt 폴더로 이동할 때 새 언어 팩 폴더를 복사하기 전에 기존 언어 팩 폴더를 이동합니다.
   * AEM을 다시 시작할 필요가 없는 경우, 업데이트된 언어 팩과 관련된 관련 Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi 구성을 다시 저장해야 AEM이 업데이트된 파일을 처리합니다.


## damAssetLucene 색인 업데이트 중 {#updating-damassetlucene-index}

[AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)에 AEM Smart Translation의 영향을 받으려면 AEM `/oak   :index  /damAssetLucene` 인덱스를 업데이트하여 예측된 태그(&quot;스마트 태그&quot;의 시스템 이름)를 자산의 집계 Lucene 색인의 일부로 표시해야 합니다.

`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags` 아래에서 구성이 다음과 같은지 확인하십시오.

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## 추가 리소스{#additional-resources}

* [Apache Oak Search Machine Translation OSGi 번들](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [쿼리 및 색인 생성에 대한 우수 사례](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)