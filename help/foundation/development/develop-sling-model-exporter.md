---
title: AEM에서 Sling 모델 내보내기 개발
description: 이 기술에서는 Sling Model Exporter에서 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 렌디션을 향상하고 Exporter 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 지정하는 방법에 대해 설명합니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Sling 모델 내보내기 개발

이 기술에서는 Sling Model Exporter에서 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 렌디션을 향상하고 Exporter 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 지정하는 방법에 대해 설명합니다.

Sling Model Exporter가 Sling Models v1.3.0에 도입되었습니다. 이 새로운 기능을 사용하면 Sling Model에 새로운 주석을 추가하여 모델을 다른 Java 객체로 내보내거나 일반적으로 JSON과 같은 다른 형식으로 직렬화할 수 있는 방법을 정의할 수 있습니다.

Apache Sling은 다른 웹 서비스 및 JavaScript 애플리케이션과 같은 프로그래밍 방식의 웹 소비자가 Sling 모델을 JSON 개체로 내보내 소비할 수 있는 가장 일반적인 사례를 다룹니다.

## Sling 모델 익스포터에 대한 AEM 구성

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] 는 의 기능입니다 [!DNL Apache Sling] AEM 제품 릴리스 주기에 직접 연결되지 않은 프로젝트 . [!DNL Sling Model Exporter] 는 AEM 6.3 이상과 호환됩니다.

## 용 사용 사례 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] HTL(또는 이전 JSP)을 통해 HTML 변환을 지원하는 비즈니스 로직이 이미 포함되어 있으며 프로그래밍 방식의 웹 서비스 또는 JavaScript 애플리케이션에서 소비할 JSON과 동일한 비즈니스 표현을 표시하는 Sling 모델을 활용할 수 있습니다.

## Sling 모델 내보내기 만들기

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

활성화 [!DNL Exporter] 지원 [!DNL Sling Model] 는 `@Exporter` Java 클래스에 대한 주석.

## Sling 모델 내보내기 옵션 적용

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] 는 내보내기 구현에 모델별 내보내기 옵션 전달을 지원하여 [!DNL Sling Model] 가 마지막으로 내보내집니다. 이러한 옵션은 일반적으로 [!DNL Sling Model] 를 내보내는 경우와 아래에 설명된 인라인 주석을 통해 수행할 수 있는 데이터 포인트마다 다릅니다.

[!DNL Jackson Exporter] 옵션은 다음과 같습니다.

* [매퍼 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [직렬화 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 적용 중 [!DNL Jackson] 주석

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

내보내기 구현은 또한 보고서에서 인라인으로 적용할 수 있는 주석을 지원할 수 있습니다 [!DNL Sling Model] 클래스를 사용하면 데이터를 내보내는 방법을 보다 세밀하게 제어할 수 있습니다.

* [[!DNL Jackson Exporter] 주석](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 코드 보기 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 지원 자료 {#supporting-materials}

* [[!DNL Jackson Mapper] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 문서](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
