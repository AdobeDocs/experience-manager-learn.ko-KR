---
title: oak-run.jar를 사용하여 인덱스 관리
description: oak-run.jar의 index 명령을 사용하면 인덱스 통계를 수집하고 색인 일관성 검사를 실행하고 인덱스를 직접 재색인화하는 등 AEM에서 Oak 인덱스를 관리하기 위해 많은 기능을 통합합니다.
version: 6.4, 6.5
feature: 검색
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: 공연
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# oak-run.jar를 사용하여 인덱스 관리

[!DNL oak-run.jar]의 index 명령을 사용하면 인덱스 통계를 수집하고 인덱스 일관성 검사를 실행하며 인덱스 자체를 재색인화하는  [!DNL Oak]등 AEM의 200개 인덱스를 관리하기 위해 다양한 기능을 통합합니다.

>[!NOTE]
>
>이 문서 및 비디오에서는 색인 지정 및 재색인화라는 용어를 서로 교환하여 사용할 수 있으며 동일한 작업으로 간주됩니다.

## [!DNL oak-run.jar] index Command 기본 사항

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 사용된 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 버전은 AEM 인스턴스에 사용된 Oak 버전과 일치해야 합니다.
* [!DNL oak-run.jar]을 사용하여 인덱스를 관리하는 경우 다양한 플래그가 있는 **[!DNL index]** 명령을 사용하여 다른 작업을 지원합니다.

   * `java -jar oak-run*.jar index ...`

## 인덱스 통계

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 오프라인 분석을 위해 모든 인덱스 정의, 중요한 색인 통계 및 색인 컨텐츠를 덤프합니다.
* 인덱스 통계 수집은 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## 인덱스 일관성 검사

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` lucene Oak 인덱스가 손상되었는지 빠르게 확인합니다.
* 일관성 확인 수준 1 및 2의 경우 사용 중인 AEM 인스턴스에서 일관성 검사를 실행할 수 있습니다.

## [!DNL oak-run.jar]을(를) 사용하는 TarMK 온라인 인덱싱 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* [!DNL oak-run.jar]을 사용하여 [!DNL TarMK]의 온라인 색인화가 `oak:queryIndexDefinition` 노드에서 `reindex=true`를 설정하는 것보다 빠릅니다. 이러한 성능 향상에도 불구하고 [!DNL oak-run.jar]을(를) 사용하는 온라인 색인화에는 색인화를 수행하기 위한 유지 관리 창이 계속 필요합니다.

* [!DNL oak-run.jar]을 사용하는 [!DNL TarMK]의 온라인 인덱싱은 AEM 인스턴스 유지 관리 창 외부의 AEM 인스턴스에 대해 **실행되지 않아야 합니다.**

## oak-run.jar를 사용한 TarMK 오프라인 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* [!DNL oak-run.jar]을 사용하는 [!DNL TarMK]의 오프라인 색인화는 단일 [!DNL oak-run.jar] 명령이 필요하므로 [!DNL TarMK]에 대한 가장 간단한 [!DNL oak-run.jar] 기반 인덱싱 접근 방법이지만 AEM 인스턴스를 종료해야 합니다.

## oak-run.jar를 사용한 TarMK 대역 외 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* [!DNL oak-run.jar]을 사용하여 [!DNL TarMK]에 대한 대역 외 인덱싱은 사용 중인 AEM 인스턴스에 색인화가 미치는 영향을 최소화합니다.
* 대역 외 인덱싱은 재색인화할 시간이 사용 가능한 유지 관리 기간을 초과하는 AEM 설치에 권장되는 색인 지정 방법입니다.

## oak-run.jar를 사용한 MongoMK Online 색인 생성

* [!DNL MongoMK] 및 [!DNL RDBMK]에 [!DNL oak-run.jar]이 있는 온라인 색인은 [!DNL MongoMK](및 [!DNL RDBMK]) AEM 설치를 다시 색인화하는 데 권장되는 방법입니다. **또는 에는 다른 방법 [!DNL MongoMK] 을 사용할 수 없습니다  [!DNL RDBMK].**
* 이 인덱스는 클러스터의 단일 AEM 인스턴스에만 대해서만 실행해야 합니다.
* 저장소 순번이 단일 [!DNL MongoDB] 노드에서만 발생하므로 실행 중인 AEM 클러스터에 대해 [!DNL MongoMK]의 온라인 인덱싱은 실행해도 안전하며, 다른 사용자는 성능에 큰 영향을 주지 않고 요청을 계속 제공할 수 있습니다.

[!DNL MongoMK]의 온라인 색인을 수행하기 위한 [!DNL oak-run.jar] index 명령은 [!DNL TarMK] Online 색인화와 동일한 [이고, 세그먼트 저장소 매개 변수가 Node 스토어를 포함하는 [!DNL MongoDB] 인스턴스를 가리키는 차이입니다. [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)

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
* [Apache Jackrabbit Oak-run.jar 색인 명령 설명서](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
