---
title: AEM Assets을 사용하여 고급 번역 검색 설정
description: 고급 번역 검색을 사용하면 영어 이외의 검색어를 사용하여 영어 컨텐츠로 확인할 수 있습니다. Smart Translation Search를 위해 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들과 번역 규칙이 포함된 적절한 무료 및 오픈 소스 Apache Joshua 언어 팩을 설치하고 구성해야 합니다.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# AEM Assets{#set-up-smart-translation-search-with-aem-assets}을(를) 사용하여 스마트 번역 검색 설정

고급 번역 검색을 사용하면 영어 이외의 검색어를 사용하여 영어 컨텐츠로 확인할 수 있습니다. Smart Translation Search를 위해 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들과 번역 규칙이 포함된 적절한 무료 및 오픈 소스 Apache Joshua 언어 팩을 설치하고 구성해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>스마트한 번역 검색은 필요한 각 AEM 인스턴스에서 설정해야 합니다.

1. Oak Search Machine Translation OSGi 번들 다운로드 및 설치
   * [AEM Oak 버전에 해당하는 Oak Search Machine Translation OSGi ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 번들을 다운로드합니다.
   * 다운로드한 Oak Search Machine Translation OSGi 번들을 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles)를 통해 AEM에 설치합니다.

2. Apache Joshua 언어 팩 다운로드 및 업데이트
   * 원하는 [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)을 다운로드하여 압축을 해제합니다.
   * `joshua.config` 파일을 편집하고 다음으로 시작하는 2개의 줄에 주석을 설정합니다.

      ```
      feature-function = LanguageModel ...
      ```

   * 언어 팩의 모델 폴더 크기를 확인하고 기록합니다. 이는 AEM에 필요한 추가 더미 공간 크기에 영향을 줍니다.
   * 압축을 푼 Apache Joshua 언어 팩 폴더(`joshua.config` 편집 포함)를

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      예:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 업데이트된 더미 메모리 할당으로 AEM을 다시 시작합니다.
   * AEM 중지
   * AEM에 필요한 새 더미 크기 확인

      * AEM 언어 사전 부족 더미 크기 + 모델 디렉토리의 크기를 가장 가까운 2GB로 올림합니다.
      * 예:사전 언어 팩을 설치하는 경우 AEM 설치를 실행하려면 8GB 힙이 필요하고 언어 팩의 모델 폴더가 3.8GB가 압축되지 않은 경우 새 더미 크기는 다음과 같습니다.

         원본 `8GB` + ( `3.75GB`은(는) `4GB`인 가장 가까운 `2GB`로 반올림됨) 총 `12GB`
   * 컴퓨터에 사용할 수 있는 이 양의 메모리가 있는지 확인합니다.
   * 새 더미 크기에 맞게 조정되도록 AEM 시작 스크립트를 업데이트합니다.

      * 예. `java -Xmx12g -jar cq-author-p4502.jar`
   * 더프 크기가 증가하여 AEM을 다시 시작합니다.

   >[!NOTE]
   >
   >언어 팩을 여러 언어 팩을 사용하는 경우 언어 팩을 설치하는 데 필요한 더미 공간이 커질 수 있습니다.
   >
   >
   >항상 **인스턴스에 할당된 더미 공간의 증가를 수용하기에 충분한 메모리**&#x200B;이 있는지 확인하십시오.
   >
   >
   >**기본 힙은 언어 팩**&#x200B;이 설치되어 있지 않은 경우 허용 가능한 성능을 지원하기 위해 항상 계산되어야 합니다.

4. Apache Jackrabbit Oak Machine Translation 전체 텍스트 쿼리 용어 공급자 OSGi 구성을 통해 언어 팩 등록

   * 각 언어 팩에 대해 AEM 웹 콘솔의 구성 관리자를 통해 [새 Apache Jackrabbit Oak Machine Translation 전체 텍스트 쿼리 용어 공급자 OSGi 구성](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)을 만듭니다.

      * `Joshua Config Path` 는 joshua.config 파일의 절대 경로입니다. AEM 프로세스는 언어 팩 폴더의 모든 파일을 읽을 수 있어야 합니다.
      * `Node types` 는 전체 텍스트 검색이 번역용 이 언어 팩에 참여할 후보 노드 유형입니다.
      * `Minimum score` 는 번역된 용어의 최소 신뢰 점수입니다.

         * 예를 들어, hombre(&quot;man&quot;의 경우 스페인어)는 신뢰도 점수가 `0.9`인 영어 단어 &quot;man&quot;으로 번역하고 신뢰도 점수가 `0.2`인 영어 단어 &quot;human&quot;로 변환할 수 있습니다. 최소 점수를 `0.3`으로 조정하면 &quot;hombre&quot;는 &quot;man&quot; 번역을 유지하지만, 이 번역 점수가 `0.2`의 최소 점수인 `0.3`보다 작으므로 &#39;hombre&#39;를 &quot;human&quot; 번역을 폐기합니다.

5. 자산에 대해 전체 텍스트 검색 수행
   * dam:Asset은 이 언어 팩이 다시 등록된 노드 유형이므로 전체 텍스트 검색을 사용하여 AEM Assets을 검색해야 유효성을 확인할 수 있습니다.
   * AEM > 자산으로 이동하여 Omnisearch를 엽니다. 언어 팩이 설치된 언어로 용어를 검색합니다.
   * 필요에 따라 OSGi 구성에서 최소 점수를 조정하여 결과의 정확성을 보장합니다.

6. 언어 팩 업데이트
   * Apache Joshua 언어 팩은 Apache Joshua 프로젝트에 의해 완전히 관리되며, 업데이트 또는 교정은 Apache Joshua 프로젝트의 재량으로 결정됩니다.
   * 언어 팩을 업데이트한 경우 AEM에서 업데이트를 설치하려면 필요한 경우 더미 크기를 늘리거나 줄이면서 위의 2-4단계를 수행해야 합니다.

      * 압축을 푼 언어 팩을 crx-quickstart/opt 폴더로 이동할 때는 새 언어 팩을 복사하기 전에 기존 언어 팩 폴더를 이동해야 합니다.
   * AEM을 다시 시작할 필요가 없는 경우 AEM에서 업데이트된 언어 팩과 관련된 관련 Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi 구성을 다시 저장해야 업데이트된 파일이 처리됩니다.


## damAssetLucene 인덱스 업데이트 중 {#updating-damassetlucene-index}

AEM Smart Translation의 영향을 받는 [AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)를 적용하려면 AEM `/oak   :index  /damAssetLucene` 인덱스를 업데이트하여 예측된 태그(&quot;스마트 태그&quot;의 시스템 이름)를 자산의 집계 Lucene 색인에 포함시켜야 합니다.

`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags` 아래에서 구성이 다음과 같은지 확인합니다.

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
* [쿼리 및 색인화에 대한 우수 사례](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)