---
title: AEM Assets을 사용하여 스마트 번역 검색 설정
description: 스마트 번역 검색을 사용하면 영어가 아닌 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. 스마트 번역 검색용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성해야 하며, 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩도 설치 및 구성해야 합니다.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 653
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# AEM Assets을 사용하여 스마트 번역 검색 설정{#set-up-smart-translation-search-with-aem-assets}

스마트 번역 검색을 사용하면 영어가 아닌 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. 스마트 번역 검색용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성해야 하며, 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩도 설치 및 구성해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>스마트 번역 검색은 필요한 각 AEM 인스턴스에 대해 설정해야 합니다.

1. Oak Search Machine Translation OSGi 번들 다운로드 및 설치
   * [Oak 검색 기계 번역 OSGi 번들 다운로드](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) AEM Oak 버전에 해당합니다.
   * 를 통해 다운로드한 Oak 검색 기계 번역 OSGi 번들을 AEM에 설치합니다. [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Apache Joshua 언어 팩 다운로드 및 업데이트
   * 원하는 를 다운로드하고 압축 해제합니다. [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * 편집 `joshua.config` 다음으로 시작하는 2개의 줄을 기록하고 주석 처리합니다.

     ```
     feature-function = LanguageModel ...
     ```

   * 언어 팩의 모델 폴더 크기를 결정하고 기록하십시오. 이는 AEM에 필요한 추가 힙 공간의 양에 영향을 미칩니다.
   * 압축 해제된 Apache Joshua 언어 팩 폴더 이동( `joshua.config` (편집) 대상

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     예:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. 업데이트된 힙 메모리 할당으로 AEM 다시 시작
   * AEM 중지
   * AEM에 대한 새 필수 힙 크기 확인

      * AEM 사전 언어 부족 힙 크기 + 모델 디렉터리 크기를 가장 가까운 2GB로 반올림함
      * 예를 들어, 사전 언어 팩을 실행하려면 AEM 설치에 8GB의 힙이 필요하며 언어 팩의 모델 폴더가 3.8GB의 압축되지 않은 경우 새 힙 크기는 다음과 같습니다.

        원본 `8GB` + ( `3.75GB` 가장 가까운 자리로 반올림됨 `2GB`, `4GB`) `12GB`

   * 컴퓨터에 사용할 수 있는 메모리가 이 정도인지 확인합니다.
   * AEM 시작 스크립트를 업데이트하여 새 힙 크기에 맞게 조정

      * 예. `java -Xmx12g -jar cq-author-p4502.jar`

   * 힙 크기를 늘린 상태로 AEM을 다시 시작합니다.

   >[!NOTE]
   >
   >특히 여러 언어 팩을 사용하는 경우 언어 팩에 필요한 힙 공간이 커질 수 있습니다.
   >
   >
   >항상 다음을 확인하십시오. **인스턴스에 충분한 메모리가 있습니다.** 할당된 힙 공간의 증가를 수용할 수 있습니다.
   >
   >
   >다음 **기본 힙은 언어 팩 없이 허용되는 성능을 지원하도록 항상 계산되어야 합니다.** 설치됨.

4. Apache Jackrabbit Oak 기계 번역을 통해 언어 팩을 등록합니다. 전체 텍스트 검색어 공급자 OSGi 구성

   * 각 언어 팩의 경우, [새 Apache Jackrabbit Oak 기계 번역 전체 텍스트 검색어 공급자 OSGi 구성 만들기](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) AEM 웹 콘솔의 구성 관리자를 통해

      * `Joshua Config Path` 는 joshua.config 파일의 절대 경로입니다. AEM 프로세스는 언어 팩 폴더의 모든 파일을 읽을 수 있어야 합니다.
      * `Node types` 은 전체 텍스트 검색에 이 언어 팩을 번역할 후보 노드 유형입니다.
      * `Minimum score` 은 사용할 번역된 용어에 대한 최소 신뢰 점수입니다.

         * 예를 들어 hombre(스페인어: &quot;man&quot;)는 신뢰 점수가 인 영어 단어 &quot;man&quot;으로 번역할 수 있습니다. `0.9` 또한 영어 단어 &quot;인간&quot;을 신뢰도 점수로 번역합니다. `0.2`. 최소 점수를으로 조정 `0.3`를 지정하면 &quot;hombre&quot;에서 &quot;man&quot; 번역으로 유지되지만, 이 번역 점수처럼 &quot;hombre&quot;에서 &quot;human&quot; 번역은 무시됩니다. `0.2` 은(는) 의 최소 점수보다 작습니다. `0.3`.

5. 에셋에 대해 전체 텍스트 검색 수행
   * dam:Asset은 이 언어 팩이 다시 등록된 노드 유형이므로 유효성을 검사하려면 전체 텍스트 검색을 사용하여 AEM Assets을 검색해야 합니다.
   * AEM > Assets로 이동하고 Omnisearch를 엽니다. 언어 팩이 설치된 언어로 용어를 검색합니다.
   * 필요에 따라 OSGi 구성에서 최소 점수를 조정하여 결과의 정확성을 확인합니다.

6. 언어 팩 업데이트 중
   * Apache Joshua 언어 팩은 Apache Joshua 프로젝트에 의해 완전히 관리되며, 업데이트 또는 수정은 Apache Joshua 프로젝트의 재량입니다.
   * 언어 팩이 업데이트되는 경우 AEM에 업데이트를 설치하려면 위의 2 - 4단계를 따라야 하며 필요에 따라 힙 크기를 늘리거나 줄여야 합니다.

      * 압축이 해제된 언어 팩을 crx-quickstart/opt 폴더로 이동할 때 새 폴더로 복사하기 전에 기존 언어 팩 폴더를 이동하십시오.

   * AEM을 다시 시작할 필요가 없는 경우 AEM이 업데이트된 파일을 처리할 수 있도록 업데이트된 언어 팩과 관련된 Apache Jackrabbit Oak Machine 번역 전체 텍스트 검색어 공급자 OSGi 구성을 다시 저장해야 합니다.

## damAssetLucene 인덱스 업데이트 중 {#updating-damassetlucene-index}

주문 [AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) AEM Smart Translation의 영향을 받는 AEM `/oak   :index  /damAssetLucene` predictedTags(&quot;스마트 태그&quot;의 시스템 이름)를 자산의 집계 Lucene 인덱스에 포함되도록 표시하려면 인덱스를 업데이트해야 합니다.

아래 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, 구성은 다음과 같은지 확인합니다.

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

* [Apache Oak 검색 기계 번역 OSGi 번들](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [쿼리 및 색인 생성에 대한 우수 사례](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
