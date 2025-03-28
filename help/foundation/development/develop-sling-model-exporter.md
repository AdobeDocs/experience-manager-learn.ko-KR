---
title: AEM에서 Sling 모델 내보내기 개발
description: 이 기술에서는 Sling 모델 익스포터와 함께 사용할 AEM을 설정하고, 익스포터 프레임워크를 사용하여 기존 Sling 모델을 JSON으로의 렌디션으로 개선하며, 익스포터 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 정의하는 방법에 대해 설명합니다.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Sling 모델 내보내기 개발

이 기술에서는 Sling 모델 익스포터와 함께 사용할 AEM을 설정하고, 익스포터 프레임워크를 사용하여 기존 Sling 모델을 JSON으로의 렌디션으로 개선하며, 익스포터 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 정의하는 방법에 대해 설명합니다.

Sling 모델 내보내기는 Sling 모델 v1.3.0에 도입되었습니다. 이 새로운 기능을 사용하면 모델 을 다른 Java 개체로 내보내거나 더 일반적으로 JSON과 같은 다른 형식으로 직렬화하는 방법을 정의하는 슬링 모델에 새 주석을 추가할 수 있습니다.

Apache Sling은 다른 웹 서비스 및 JavaScript 애플리케이션과 같은 프로그래밍 방식의 웹 소비자가 사용할 수 있도록 Sling 모델을 JSON 객체로 내보내는 가장 일반적인 사례를 처리할 수 있도록 Json JSON 익스포터를 제공합니다.

## Sling 모델 내보내기를 위한 AEM 구성

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter]은(는) [!DNL Apache Sling] 프로젝트의 기능이며 AEM 제품 릴리스 주기에 직접 바인딩되지 않습니다. [!DNL Sling Model Exporter]은(는) AEM 6.3 이상과 호환됩니다.

## [!DNL Sling Model Exporter]의 사용 사례

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter]은(는) HTL(또는 이전의 JSP)을 통해 HTML 렌디션을 지원하고 프로그래밍 방식의 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있도록 JSON과 동일한 비즈니스 표현을 표시하는 비즈니스 논리를 이미 포함하는 Sling 모델을 활용하는 데 적합합니다.

## 슬링 모델 내보내기 만들기

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

[!DNL Sling Model]에서 [!DNL Exporter] 지원을 사용하도록 설정하는 것은 Java 클래스에 `@Exporter` 주석을 추가하는 것만큼 쉽습니다.

## 슬링 모델 내보내기 옵션 적용

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter]에서는 [!DNL Sling Model]을(를) 최종적으로 내보내는 방법을 유도하기 위해 모델별 내보내기 옵션을 내보내기 구현으로 전달할 수 있습니다. 이러한 옵션은 일반적으로 아래 설명된 인라인 주석을 통해 수행할 수 있는 데이터 포인트와 비교하여 [!DNL Sling Model]을(를) 내보내는 방법에 &quot;전역&quot;으로 적용됩니다.

[!DNL Jackson Exporter] 옵션은 다음과 같습니다.

* [매퍼 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [직렬화 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson]개 주석 적용 중

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Exports 구현은 [!DNL Sling Model] 클래스에서 인라인으로 적용할 수 있는 주석을 지원할 수도 있으며, 이러한 주석은 데이터를 내보내는 방법에 대해 더 세밀한 수준의 제어를 제공할 수 있습니다.

* [[!DNL Jackson Exporter] 주석](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 코드 보기 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 지원 자료 {#supporting-materials}

* [[!DNL Jackson Mapper] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 문서](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
