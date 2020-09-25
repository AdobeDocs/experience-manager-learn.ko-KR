---
title: AEM에서 슬링 모델 수출업체 개발
description: 이 기술에서는 Sling Model Exporter와 함께 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 표현하기 위한 기존 Sling Model 향상, Exporter 옵션 및 Jackson 주석을 사용하여 출력을 사용자 정의하는 방법을 살펴봅니다.
version: 6.3, 6.4, 6.5
sub-product: 기반, 컨텐츠 서비스
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# 슬링 모델 수출업체 개발

이 기술에서는 Sling Model Exporter와 함께 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 표현하기 위한 기존 Sling Model 향상, Exporter 옵션 및 Jackson 주석을 사용하여 출력을 사용자 정의하는 방법을 살펴봅니다.

Sling Model Exporter는 Sling Models v1.3.0에서 도입되었습니다. 이 새로운 기능을 사용하면 모델을 다른 Java 객체로 내보낼 수 있는 방법을 정의하거나 JSON과 같은 다른 포맷으로 직렬화할 수 있는 새로운 주석을 Sling Models에 추가할 수 있습니다.

Apache Sling은 다른 웹 서비스 및 JavaScript 애플리케이션과 같은 프로그래머틱 웹 소비자가 Sling 모델을 JSON 오브젝트로 내보내 소비할 때 가장 일반적인 경우를 포함하는 Jackson JSON 내보내기 프로그램을 제공합니다.

## Sling 모델 내보내기 AEM 구성

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 는 프로젝트의 [!DNL Apache Sling] 기능으로 AEM 제품 릴리스 주기에 직접 연결되지 않습니다. [!DNL Sling Model Exporter] 은 AEM 6.3 이상과 호환됩니다.

## 사용 사례 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] HTL(또는 이전 JSP)을 통해 HTML 변환을 지원하는 비즈니스 로직이 이미 포함되어 있는 Sling 모델을 활용하는 데 적합하며, 프로그래머틱 웹 서비스 또는 JavaScript 애플리케이션을 통해 소비를 위해 JSON과 동일한 비즈니스 표현을 노출할 수 있습니다.

## 슬링 모델 내보내기 만들기

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Java 클래스에 주석 [!DNL Exporter] 을 추가하는 것만큼 손쉽게 [!DNL Sling Model] `@Exporter` a에 대한 지원을 활성화할 수 있습니다.

## 슬링 모델 내보내기 옵션 적용

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 는 최종 내보내기 방법을 유도하기 위해 모델별 내보내기 옵션 전달 [!DNL Sling Model] 을 지원합니다. 이러한 옵션은 일반적으로 내보내기의 방법에 &quot;전역적&quot; [!DNL Sling Model] 으로 적용되며, 아래에 설명된 인라인 주석을 통해 수행할 수 있는 데이터 포인트별로 적용됩니다.

[!DNL Jackson Exporter] 옵션:

* [매퍼 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [직렬화 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 주석 적용 [!DNL Jackson] 중

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Exporter 구현은 데이터의 내보내기 방법을 보다 세밀하게 제어할 수 있도록 클래스에 인라인으로 적용할 수 있는 주석을 지원할 수도 있습니다. [!DNL Sling Model]

* [[!DNL Jackson Exporter] 주석](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 코드 보기 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 지원 자료 {#supporting-materials}

* [[!DNL Jackson Mapper] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 문서](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
