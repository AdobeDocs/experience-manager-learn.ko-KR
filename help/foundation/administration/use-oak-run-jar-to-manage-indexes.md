---
title: oak-run.jar를 사용하여 인덱스 관리
description: oak-run.jar의 index 명령은 인덱스 통계 수집, 인덱스 일관성 검사 실행 및 인덱스 자체 리인덱싱 등 AEM에서 Oak 인덱스를 관리하는 여러 기능을 통합합니다.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# oak-run.jar를 사용하여 인덱스 관리

[!DNL oak-run.jar]의 index 명령은 인덱스 통계 수집, 인덱스 일관성 검사 실행 및 인덱스 자체 다시 인덱싱 등 AEM에서 [!DNL Oak]200개의 인덱스를 관리하는 여러 기능을 통합합니다.

>[!NOTE]
>
>이 문서 및 비디오에서는 색인화 및 재색인화라는 용어가 서로 교환하여 사용되며 동일한 작업으로 간주됩니다.

## [!DNL oak-run.jar] 인덱스 명령 기본 사항

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 사용된 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&g=org.apache.jackrabbit&a=oak-run&v=1.8.0)의 버전은 AEM 인스턴스에 사용된 Oak 버전과 일치해야 합니다.
* [!DNL oak-run.jar]을(를) 사용하여 인덱스를 관리하면 다양한 플래그가 있는 **[!DNL index]** 명령을 사용하여 다양한 작업을 지원합니다.

   * `java -jar oak-run*.jar index ...`

## 색인 통계

>[!VIDEO](https://video.tv.adobe.com/v/34856?quality=12&learn=on&captions=kor)

* `oak-run.jar`이(가) 오프라인 분석을 위해 모든 인덱스 정의, 중요한 인덱스 통계 및 인덱스 컨텐츠를 덤프합니다.
* 인덱스 통계 수집은 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## 색인 일관성 검사

>[!VIDEO](https://video.tv.adobe.com/v/36865?quality=12&learn=on&captions=kor)

* `oak-run.jar`은(는) lucene Oak 색인이 손상되었는지 빠르게 확인합니다.
* 일관성 검사는 일관성 검사 수준 1과 2에 대해 사용 중인 AEM 인스턴스에서 실행해도 안전합니다.

## [!DNL oak-run.jar]을(를) 사용한 TarMK 온라인 인덱싱 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/36867?quality=12&learn=on&captions=kor)

* [!DNL oak-run.jar]을(를) 사용하는 [!DNL TarMK]의 온라인 인덱싱이 `oak:queryIndexDefinition` 노드에서 `reindex=true`을(를) 설정하는 것보다 빠릅니다. 이렇게 성능이 향상되더라도 [!DNL oak-run.jar]을(를) 사용하는 온라인 인덱싱에는 인덱싱을 수행하기 위한 유지 관리 기간이 필요합니다.

* [!DNL oak-run.jar]을(를) 사용하는 [!DNL TarMK]의 온라인 인덱싱은 AEM의 인스턴스 유지 관리 창 외부의 AEM 인스턴스에 대해 **실행할 수 없습니다**.

## oak-run.jar을 사용한 TarMK 오프라인 색인화

>[!VIDEO](https://video.tv.adobe.com/v/36866?quality=12&learn=on&captions=kor)

* 단일 [!DNL oak-run.jar] 명령이 필요하지만 AEM 인스턴스를 종료해야 하므로 [!DNL oak-run.jar]을(를) 사용하는 [!DNL TarMK]의 오프라인 인덱싱은 [!DNL TarMK]에 대해 가장 간단한 [!DNL oak-run.jar] 기반 인덱싱 접근 방식입니다.

## oak-run.jar을 사용한 TarMK 대역 외 인덱싱

>[!VIDEO](https://video.tv.adobe.com/v/340812?quality=12&learn=on&captions=kor)

* [!DNL oak-run.jar]을(를) 사용하여 [!DNL TarMK]에서 대역 외 인덱싱을 수행하면 사용 중인 AEM 인스턴스에 대한 인덱싱의 영향이 최소화됩니다.
* 대역 외 색인화는 다시 색인화하는 시간이 사용 가능한 유지 관리 기간을 초과하는 AEM 설치에 권장되는 색인화 방법입니다.

## oak-run.jar을 사용한 MongoMK 온라인 색인화

* [!DNL MongoMK] 및 [!DNL RDBMK]에 [!DNL oak-run.jar]이(가) 있는 온라인 인덱스는 [!DNL MongoMK]&#x200B;(및 [!DNL RDBMK]) AEM 설치에 대한 리인덱싱/인덱싱의 권장 방법입니다. **다른 메서드는 [!DNL MongoMK] 또는 [!DNL RDBMK]에 사용할 수 없습니다.**
* 이 인덱싱은 클러스터의 단일 AEM 인스턴스에 대해서만 실행해야 합니다.
* 저장소 순회가 단일 [!DNL MongoDB] 노드에서만 수행되므로 [!DNL MongoMK]의 온라인 인덱싱은 실행 중인 AEM 클러스터에 대해 실행해도 안전합니다. 다른 노드에서는 성능에 큰 영향을 주지 않고 요청을 계속 제공할 수 있습니다.

[!DNL MongoMK]의 온라인 인덱싱을 수행하는 [!DNL oak-run.jar] 인덱스 명령은 세그먼트 저장소 매개 변수가 노드 저장소를 포함하는 [!DNL MongoDB] 인스턴스를 가리키는 것과 다른  [!DNL oak-run.jar][&#128279;](#tarmkonlineindexingwithoakrunjar)을(를) 사용하는  [!DNL TarMK] 온라인 인덱싱과 동일합니다.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 지원 자료

* [다운로드 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *다운로드한 버전이 위에서 설명한 대로 AEM에 설치된 Oak 버전과 일치하는지 확인*
* [Apache Jackrabbit Oak oak-run.jar 인덱스 명령 설명서](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
