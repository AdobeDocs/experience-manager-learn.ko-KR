---
title: oak-run.jar를 사용하여 인덱스 관리
description: oak-run.jar의 index 명령은 인덱스 통계 수집, 인덱스 일관성 검사 실행 및 인덱스 자체 리인덱싱 등 AEM에서 Oak 인덱스를 관리하는 여러 기능을 통합합니다.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# oak-run.jar를 사용하여 인덱스 관리

[!DNL oak-run.jar]의 index 명령은 관리할 여러 기능을 통합합니다. [!DNL Oak]인덱스 통계 수집, 인덱스 일관성 검사 실행 및 인덱스 자체 리인덱싱 등 AEM의 200개 인덱스.

>[!NOTE]
>
>이 문서 및 비디오에서는 색인화 및 재색인화라는 용어가 서로 교환하여 사용되며 동일한 작업으로 간주됩니다.

## [!DNL oak-run.jar] index 명령 기본 사항

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 버전 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 는 AEM 인스턴스에 사용된 Oak 버전과 일치해야 합니다.
* 를 사용하여 인덱스 관리 [!DNL oak-run.jar] 을 활용합니다. **[!DNL index]** 다양한 플래그를 사용하여 다양한 작업을 지원하는 명령입니다.

   * `java -jar oak-run*.jar index ...`

## 색인 통계

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 오프라인 분석을 위해 모든 인덱스 정의, 중요한 인덱스 통계 및 인덱스 컨텐츠를 덤프합니다.
* 인덱스 통계 수집은 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## 색인 일관성 검사

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` lucene Oak 색인이 손상되었는지 신속하게 확인합니다.
* 일관성 검사는 일관성 검사 수준 1과 2에 대해 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## 을 사용한 TarMK 온라인 색인화 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 의 온라인 색인화 [!DNL TarMK] 사용 [!DNL oak-run.jar] 설정보다 빠름 `reindex=true` 다음에 있음 `oak:queryIndexDefinition` 노드. 이러한 성능 향상에도 불구하고 온라인 색인화에는 [!DNL oak-run.jar] 인덱싱을 수행하려면 유지 관리 기간이 필요합니다.

* 의 온라인 색인화 [!DNL TarMK] 사용 [!DNL oak-run.jar] 다음과 같아야 함 **아님** AEM 인스턴스 유지 관리 창 외부에서 AEM 인스턴스에 대해 실행됩니다.

## oak-run.jar을 사용한 TarMK 오프라인 색인화

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 의 오프라인 색인화 [!DNL TarMK] 사용 [!DNL oak-run.jar] 가장 간단함 [!DNL oak-run.jar] 에 대한 기반 색인 지정 접근 방식 [!DNL TarMK] 1개가 필요하므로 [!DNL oak-run.jar] 그러나 명령을 실행하려면 AEM 인스턴스를 종료해야 합니다.

## oak-run.jar을 사용한 TarMK 대역 외 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 의 대역 외 인덱싱 [!DNL TarMK] 사용 [!DNL oak-run.jar] 인덱싱이 사용 중인 AEM 인스턴스에 미치는 영향을 최소화합니다.
* 대역 외 색인화는 다시 색인화하는 시간이 사용 가능한 유지 관리 기간을 초과하는 AEM 설치에 권장되는 색인화 방법입니다.

## oak-run.jar을 사용한 MongoMK 온라인 색인화

* 온라인 색인 [!DNL oak-run.jar] 날짜 [!DNL MongoMK] 및 [!DNL RDBMK] 는 리인덱싱에 권장되는 방법입니다. [!DNL MongoMK] (및 [!DNL RDBMK]) AEM 설치 **다음에 대해 다른 방법을 사용해서는 안 됩니다. [!DNL MongoMK] 또는 [!DNL RDBMK].**
* 이 인덱싱은 클러스터의 단일 AEM 인스턴스에 대해서만 실행해야 합니다.
* 의 온라인 색인화 [!DNL MongoMK] 저장소 순회가 단일 인스턴스에서만 발생하므로 실행 중인 AEM 클러스터에 대해 실행해도 안전합니다. [!DNL MongoDB] 다른 사용자가 성능에 큰 영향을 주지 않고 요청을 계속 제공할 수 있는 노드

다음 [!DNL oak-run.jar] 의 온라인 색인화를 수행하는 index 명령 [!DNL MongoMK] 은(는) [와 동일 [!DNL TarMK] 을 사용한 온라인 색인화 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 세그먼트 저장소 매개 변수가 [!DNL MongoDB] 노드 저장소를 포함하는 인스턴스입니다.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 지원 자료

* [다운로드 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *다운로드한 버전이 위에서 설명한 대로 AEM에 설치된 Oak 버전과 일치하는지 확인합니다*
* [Apache Jackrabbit Oak-run.jar 인덱스 명령 설명서](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
