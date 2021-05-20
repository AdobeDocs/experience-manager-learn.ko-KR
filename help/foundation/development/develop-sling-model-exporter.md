---
title: AEM에서 Sling 모델 내보내기 개발
description: 이 기술에서는 Sling Model Exporter에서 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 렌디션을 향상하고 Exporter 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 지정하는 방법에 대해 설명합니다.
version: 6.3, 6.4, 6.5
sub-product: foundation, content services
feature: API
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---


# Sling 모델 내보내기 개발

이 기술에서는 Sling Model Exporter에서 사용할 AEM 설정, Exporter 프레임워크를 사용하여 JSON으로 렌디션을 향상하고 Exporter 옵션 및 Jackson 주석을 사용하여 출력을 추가로 사용자 지정하는 방법에 대해 설명합니다.

Sling Model Exporter가 Sling Models v1.3.0에 도입되었습니다. 이 새로운 기능을 사용하면 Sling Model에 새로운 주석을 추가하여 모델을 다른 Java 객체로 내보내거나 일반적으로 JSON과 같은 다른 형식으로 직렬화할 수 있는 방법을 정의할 수 있습니다.

Apache Sling은 다른 웹 서비스 및 JavaScript 애플리케이션과 같은 프로그래밍 방식의 웹 소비자가 Sling 모델을 JSON 개체로 내보내 소비할 수 있는 가장 일반적인 사례를 다룹니다.

## Sling 모델 익스포터에 대한 AEM 구성

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 는 프로젝트의  [!DNL Apache Sling] 기능이며 AEM 제품 릴리스 주기에 직접 연결되지 않습니다. [!DNL Sling Model Exporter] 는 AEM 6.3 이상과 호환됩니다.

## [!DNL Sling Model Exporter]에 대한 사용 사례

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] HTL(또는 이전 JSP)을 통해 HTML 변환을 지원하는 비즈니스 로직이 이미 포함되어 있으며 프로그래밍 방식의 웹 서비스 또는 JavaScript 애플리케이션에서 소비할 JSON과 동일한 비즈니스 표현을 표시하는 Sling 모델을 활용할 수 있습니다.

## Sling 모델 내보내기 만들기

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

[!DNL Sling Model]에서 [!DNL Exporter] 지원을 활성화하는 것은 Java 클래스에 `@Exporter` 주석을 추가하는 것만큼 쉽습니다.

## Sling 모델 내보내기 옵션 적용

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 은 최종적으로 내보내는 방법을 유도하기 위해 모델별 내보내기 옵션 [!DNL Sling Model] 을 내보내기 구현에 전달할 수 있도록 지원합니다. 이러한 옵션은 일반적으로 [!DNL Sling Model] 내보내기 방법과 아래에 설명된 인라인 주석을 통해 수행할 수 있는 데이터 포인트에 &quot;전역적&quot;이 적용됩니다.

[!DNL Jackson Exporter] 옵션은 다음과 같습니다.

* [매퍼 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [직렬화 기능 옵션](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson] 주석 적용

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

내보내기 구현에서는 [!DNL Sling Model] 클래스에서 인라인으로 적용할 수 있는 주석을 지원할 수 있으므로 데이터를 내보내는 방법을 보다 세밀하게 제어할 수 있습니다.

* [[!DNL Jackson Exporter] 주석](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 코드 {#view-the-code} 보기

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 지원 자료 {#supporting-materials}

* [[!DNL Jackson Mapper] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 기능 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 문서](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
