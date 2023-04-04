---
title: oak-run.jar를 사용하여 인덱스 관리
description: oak-run.jar의 index 명령을 사용하면 인덱스 통계를 수집하고 색인 일관성 검사를 실행하고 인덱스를 직접 재색인화하는 등 AEM에서 Oak 인덱스를 관리하기 위해 많은 기능을 통합합니다.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# oak-run.jar를 사용하여 인덱스 관리

[!DNL oak-run.jar]의 index 명령을 사용하면 여러 기능을 통합하여 관리할 수 있습니다 [!DNL Oak]인덱스 통계 수집, 인덱스 일관성 검사 실행, 인덱스 직접 재인덱싱 등 AEM의 200개의 인덱스.

>[!NOTE]
>
>이 문서 및 비디오에서는 색인 지정 및 재색인화라는 용어를 서로 교환하여 사용할 수 있으며 동일한 작업으로 간주됩니다.

## [!DNL oak-run.jar] index Command 기본 사항

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 버전 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 사용된 은 AEM 인스턴스에 사용된 Oak 버전과 일치해야 합니다.
* 다음을 사용하여 인덱스 관리 [!DNL oak-run.jar] 활용 **[!DNL index]** 다양한 플래그를 사용하여 다양한 작업을 지원합니다.

   * `java -jar oak-run*.jar index ...`

## 인덱스 통계

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 오프라인 분석을 위해 모든 인덱스 정의, 중요한 색인 통계 및 색인 컨텐츠를 덤프합니다.
* 인덱스 통계 수집은 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## 인덱스 일관성 검사

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` lucene Oak 인덱스가 손상되었는지 빠르게 확인합니다.
* 일관성 확인 수준 1 및 2의 경우 사용 중인 AEM 인스턴스에서 일관성 검사를 실행할 수 있습니다.

## TarMK 온라인 색인 생성 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 온라인 색인 생성 [!DNL TarMK] 사용 [!DNL oak-run.jar] 설정보다 빠릅니다. `reindex=true` on `oak:queryIndexDefinition` 노드 아래에 있어야 합니다. 이러한 성능이 증가했지만 [!DNL oak-run.jar] 색인을 수행하려면 유지 관리 창이 필요합니다.

* 온라인 색인 생성 [!DNL TarMK] 사용 [!DNL oak-run.jar] 다음과 같이 하십시오. **not** AEM 인스턴스 유지 관리 창의 외부에 있는 AEM 인스턴스에 대해 실행됩니다.

## oak-run.jar를 사용한 TarMK 오프라인 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 의 오프라인 색인 지정 [!DNL TarMK] 사용 [!DNL oak-run.jar] 가장 간단한 [!DNL oak-run.jar] 기반 인덱싱 방식 [!DNL TarMK] 단일 [!DNL oak-run.jar] 명령을 실행하려면 AEM 인스턴스가 종료되어야 합니다.

## oak-run.jar를 사용한 TarMK 대역 외 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 대역 외 인덱싱 설정 [!DNL TarMK] 사용 [!DNL oak-run.jar] 색인화가 in-use AEM 인스턴스에 미치는 영향을 최소화합니다.
* 대역 외 인덱싱은 재색인화할 시간이 사용 가능한 유지 관리 기간을 초과하는 AEM 설치에 권장되는 색인 지정 방법입니다.

## oak-run.jar를 사용한 MongoMK Online 색인 생성

* 온라인 인덱스 [!DNL oak-run.jar] on [!DNL MongoMK] 및 [!DNL RDBMK] 는 재색인화를 위한 권장 방법입니다 [!DNL MongoMK] (및 [!DNL RDBMK]) AEM 설치를 참조하십시오. **에 다른 방법을 사용할 필요가 없습니다. [!DNL MongoMK] 또는 [!DNL RDBMK].**
* 이 인덱스는 클러스터의 단일 AEM 인스턴스에만 대해서만 실행해야 합니다.
* 온라인 색인 생성 [!DNL MongoMK] 저장소 순번이 단일 항목만 발생하므로 실행 중인 AEM 클러스터에 대해 실행해도 안전합니다 [!DNL MongoDB] 노드 아래에 배치되므로 다른 사용자가 성능에 큰 영향을 주지 않고 요청을 계속 제공할 수 있습니다.

다음 [!DNL oak-run.jar] 인덱스 명령에서 온라인 색인화를 수행합니다. [!DNL MongoMK] 은 [와 동일 [!DNL TarMK] 을 사용하여 온라인 색인 생성 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 세그먼트 저장소 매개 변수가 [!DNL MongoDB] 노드 저장소가 포함된 인스턴스입니다.

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
