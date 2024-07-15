---
title: AEM Assets을 사용하여 스마트 번역 검색 설정
description: 스마트 번역 검색을 사용하면 영어가 아닌 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. 스마트 번역 검색용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성해야 하고, 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩도 설치 및 구성해야 합니다.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 603
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# AEM Assets을 사용하여 스마트 번역 검색 설정{#set-up-smart-translation-search-with-aem-assets}

스마트 번역 검색을 사용하면 영어가 아닌 검색어를 사용하여 영어 콘텐츠로 확인할 수 있습니다. 스마트 번역 검색용 AEM을 설정하려면 Apache Oak Search Machine Translation OSGi 번들을 설치 및 구성해야 하고, 번역 규칙이 포함된 관련 무료 및 오픈 소스 Apache Joshua 언어 팩도 설치 및 구성해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>스마트 번역 검색은 필요한 각 AEM 인스턴스에 대해 설정해야 합니다.

1. Oak Search Machine Translation OSGi 번들 다운로드 및 설치
   * AEM의 Oak 버전에 해당하는 [Oak Search Machine Translation OSGi 번들 다운로드](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22).
   * 다운로드한 Oak 검색 기계 번역 OSGi 번들을 [`/system/console/bundles`](http://localhost:4502/system/console/bundles)을(를) 통해 AEM에 설치합니다.

2. Apache Joshua 언어 팩 다운로드 및 업데이트
   * 원하는 [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)을 다운로드하고 압축 해제합니다.
   * `joshua.config` 파일을 편집하고 다음으로 시작하는 두 줄을 주석 처리합니다.

     ```
     feature-function = LanguageModel ...
     ```

   * 언어 팩의 모델 폴더 크기를 결정하고 기록하십시오. 이는 AEM에 필요한 추가 힙 공간의 양에 영향을 미칩니다.
   * 압축 해제된 Apache Joshua 언어 팩 폴더(`joshua.config` 편집 포함)를 다음으로 이동

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

      * AEM의 사전 언어 부족 힙 크기 + 모델 디렉토리 크기를 가장 가까운 2GB로 반올림함
      * 예를 들어, 사전 언어 팩을 실행하려면 AEM 설치에 8GB의 힙이 필요하며 언어 팩의 모델 폴더가 3.8GB의 압축되지 않은 경우 새 힙 크기는 다음과 같습니다.

        총 `12GB`에 대해 원래 `8GB` + ( `3.75GB`을(를) 가장 가까운 `2GB`(즉, `4GB`)(으)로 반올림함

   * 컴퓨터에 사용할 수 있는 메모리가 이 정도인지 확인합니다.
   * AEM의 시작 스크립트를 업데이트하여 새 힙 크기에 맞게 조정

      * 예: `java -Xmx12g -jar cq-author-p4502.jar`

   * 힙 크기를 늘린 상태로 AEM을 다시 시작합니다.

   >[!NOTE]
   >
   >특히 여러 언어 팩을 사용하는 경우 언어 팩에 필요한 힙 공간이 커질 수 있습니다.
   >
   >
   >**인스턴스에 충분한 메모리가 있는지**&#x200B;을(를) 항상 확인하십시오.
   >
   >
   >언어 팩을 설치하지 않고 허용되는 성능을 지원하려면 **기본 힙을 항상 계산해야 합니다**.

4. Apache Jackrabbit Oak 기계 번역을 통해 언어 팩을 등록합니다. 전체 텍스트 검색어 공급자 OSGi 구성

   * 각 언어 팩에 대해 AEM 웹 콘솔의 구성 관리자를 통해 [새 Apache Jackrabbit Oak 기계 번역 전체 텍스트 검색어 공급자 OSGi 구성을 만듭니다](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory).

      * `Joshua Config Path`은(는) joshua.config 파일의 절대 경로입니다. AEM 프로세스는 언어 팩 폴더의 모든 파일을 읽을 수 있어야 합니다.
      * `Node types`은(는) 전체 텍스트 검색에 이 언어 팩을 번역할 수 있는 후보 노드 유형입니다.
      * `Minimum score`은(는) 번역된 용어가 사용되는 최소 신뢰 점수입니다.

         * 예를 들어, hombre(스페인어: &quot;man&quot;)는 신뢰도 점수가 `0.9`인 영어 단어 &quot;man&quot;으로 번역되고 신뢰도 점수가 `0.2`인 영어 단어 &quot;human&quot;으로 번역될 수 있습니다. 최소 점수를 `0.3`(으)로 조정하면 &quot;hombre&quot;에서 &quot;man&quot; 번역으로 유지되지만, 이 번역 점수 `0.2`이(가) 최소 점수 `0.3`보다 작으므로 &#39;hombre&#39;에서 &quot;human&quot; 번역으로 무시됩니다.

5. 에셋에 대해 전체 텍스트 검색 수행
   * dam:Asset은 이 언어 팩이 다시 등록된 노드 유형이므로 유효성을 검사하려면 전체 텍스트 검색을 사용하여 AEM Assets을 검색해야 합니다.
   * AEM > Assets 로 이동하고 Omnisearch를 엽니다. 언어 팩이 설치된 언어로 용어를 검색합니다.
   * 필요에 따라 OSGi 구성에서 최소 점수를 조정하여 결과의 정확성을 확인합니다.

6. 언어 팩 업데이트 중
   * Apache Joshua 언어 팩은 Apache Joshua 프로젝트에 의해 완전히 관리되며, 업데이트 또는 수정은 Apache Joshua 프로젝트의 재량입니다.
   * 언어 팩이 업데이트되는 경우 AEM에 업데이트를 설치하려면 위의 2 - 4단계를 따라야 하며 필요에 따라 힙 크기를 늘리거나 줄여야 합니다.

      * 압축이 해제된 언어 팩을 crx-quickstart/opt 폴더로 이동할 때 새 폴더로 복사하기 전에 기존 언어 팩 폴더를 이동하십시오.

   * AEM을 다시 시작할 필요가 없는 경우 AEM이 업데이트된 파일을 처리할 수 있도록 업데이트된 언어 팩과 관련된 Apache Jackrabbit Oak 기계 번역 전체 텍스트 검색어 공급자 OSGi 구성을 다시 저장해야 합니다.

## damAssetLucene 인덱스 업데이트 중 {#updating-damassetlucene-index}

[AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)이(가) AEM 스마트 변환의 영향을 받으려면 AEM의 `/oak   :index  /damAssetLucene` 인덱스를 업데이트하여 predictedTags(&quot;스마트 태그&quot;의 시스템 이름)를 자산의 집계 Lucene 인덱스의 일부로 표시해야 합니다.

`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`에서 구성은 다음과 같은지 확인하십시오.

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

* [Apache Oak 검색 컴퓨터 번역 OSGi 번들](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua 언어 팩](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM 스마트 태그](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [쿼리 및 색인화 모범 사례](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
