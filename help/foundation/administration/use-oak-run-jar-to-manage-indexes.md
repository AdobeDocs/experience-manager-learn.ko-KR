---
title: oak-run.jar를 사용하여 인덱스 관리
description: oak-run.jar의 index 명령은 인덱스 통계 수집, 인덱스 일관성 검사 실행, 색인 재지정 등 AEM의 Oak 색인을 관리하기 위한 다양한 기능을 통합합니다.
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# oak-run.jar를 사용하여 인덱스 관리

[!DNL oak-run.jar]&#39;s index command&#39;는 인덱스 통계 수집, 인덱스 일관성 검사 실행, 색인 자체 재지정 등 AEM에서 [!DNL Oak]200개의 인덱스를 관리하는 다양한 기능을 통합합니다.

>[!NOTE]
>
>이 아티클 및 비디오 내에서 색인 지정 및 다시 색인 지정 용어가 혼용되어 동일한 작업으로 간주됩니다.

## [!DNL oak-run.jar] index 명령 기초

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 사용된 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 버전은 AEM 인스턴스에서 사용되는 Oak 버전과 일치해야 합니다.
* 여러 가지 작업 [!DNL oak-run.jar] 을 지원하기 위해 다양한 플래그가 있는 **[!DNL index]** 명령을 사용하여 색인을 관리합니다.

   * `java -jar oak-run*.jar index ...`

## 색인 통계

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 오프라인 분석을 위해 모든 색인 정의, 중요한 색인 통계 및 색인 내용을 덤프합니다.
* 색인 통계 수집은 사용 중인 AEM 인스턴스에서 실행되는 것이 안전합니다.

## 색인 일관성 확인

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` lucene Oak 색인이 손상되었는지 신속하게 확인합니다.
* 일관성 검사는 일관성 확인 수준 1 및 2를 위해 사용 중인 AEM 인스턴스에서 실행되는 것이 안전합니다.

## TarMK Online 인덱싱 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 온라인 색인 [!DNL TarMK] 사용 [!DNL oak-run.jar] 이 노드에서 설정하는 `reindex=true` 것보다 `oak:queryIndexDefinition` 빠릅니다. 이러한 성능 향상에도 불구하고 색인 작업을 수행하려면 온라인 색인 작성 [!DNL oak-run.jar] 을 사용하는 유지 관리 기간이 필요합니다.

* AEM 인스턴스 유지 관리 창 외부의 AEM 인스턴스에 대해 [!DNL TarMK] 온라인 색인 작성 [!DNL oak-run.jar] **** 을 실행할 수 없습니다.

## oak-run.jar를 사용한 TarMK 오프라인 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 오프라인 인덱싱 [!DNL TarMK] 은 단일 [!DNL oak-run.jar] 명령이 필요하므로 [!DNL oak-run.jar] 가장 간단한 [!DNL TarMK] [!DNL oak-run.jar] 기반 인덱싱 접근 방식이지만 AEM 인스턴스를 종료해야 합니다.

## oak-run.jar를 사용한 TarMK 대역 외 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 사용 중인 AEM 인스턴스에 대한 색인 작성 [!DNL TarMK] 의 효과를 [!DNL oak-run.jar] 최소화합니다.
* 대역외 인덱스는 재색인 시간이 사용 가능한 유지 관리 기간을 초과하는 AEM 설치에 권장되는 인덱싱 접근 방식입니다.

## MongoMK Online 인덱싱 with oak-run.jar

* 온라인 색인 [!DNL oak-run.jar][!DNL MongoMK] 은 [!DNL RDBMK] on [!DNL MongoMK] 및 [!DNL RDBMK]AEM 설치를 다시 색인(및)하는 데 권장되는 방법입니다. **다른 방법은[!DNL MongoMK]또는[!DNL RDBMK].**
* 이 인덱스는 클러스터의 단일 AEM 인스턴스에만 실행해야 합니다.
* 저장소 트래픽이 단일 [!DNL MongoMK] 노드에서만 발생하므로 실행 중인 AEM 클러스터에 대해 온라인 색인 작성 [!DNL MongoDB] 을 실행하는 것이 안전하므로 다른 사용자가 성능에 영향을 미치지 않고 요청을 계속 제공할 수 있습니다.

온라인 인덱싱을 수행하는 [!DNL oak-run.jar] index 명령 [!DNL MongoMK] 은 [세그먼트 저장소 매개 변수 [!DNL TarMK] 가 노드 스토어를 포함하는 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 인스턴스를 가리키는 차이점과 [!DNL MongoDB] 온라인 인덱스와동일합니다.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 지원 자료

* [다운로드 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *다운로드한 버전이 위에 설명된 대로 AEM에 설치된 Oak 버전과 일치하는지 확인합니다*
* [Apache Jackrabbit Oak-run.jar Index 명령 설명서](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
