---
title: AEM의 Sling 모델 내보내기 이해
description: Apache Sling Models 1.3.0에서는 Sling Model 개체를 사용자 정의 추상화로 내보내거나 직렬화하는 우아한 방법인 Sling Model Exporter가 도입되었습니다. 이 문서에서는 Sling 모델을 사용하여 HTL 스크립트를 채우는 일반적인 사용 사례와 함께 Sling 모델 내보내기 프레임워크를 사용하여 Sling 모델을 JSON으로 serialize하는 방법을 사용합니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# 이해 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0에서는 다음을 소개합니다. [!DNL Sling Model Exporter]를 내보내거나 serialize하는 우아한 방법 [!DNL Sling Model] 개체를 사용자 지정 추상화로 전환합니다. 이 문서는 기존의 사용 사례를 병치합니다 [!DNL Sling Models] 를 활용하여 HTL 스크립트를 [!DNL Sling Model Exporter] 정리 프레임워크 [!DNL Sling Model] JSON에 매핑됩니다.

## 기존 Sling 모델 HTTP 요청 흐름

에 대한 기존 사용 사례 [!DNL Sling Models] 는 HTL 스크립트(또는 이전 JSP)에 비즈니스 기능에 액세스하기 위한 인터페이스를 제공하는 리소스 또는 요청에 대한 비즈니스 추상화를 제공하기 위한 것입니다.

공통 패턴은 개발되고 있습니다 [!DNL Sling Models] 는 AEM 구성 요소 또는 페이지를 나타내며 [!DNL Sling Model] 브라우저에 표시되는 HTML의 최종 결과를 사용하여 HTL 스크립트를 데이터로 제공할 개체.

### Sling 모델 HTTP 요청 흐름

![Sling 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM의 리소스에 대해 요청이 수행되었습니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스의 `sling:resourceType`로 설정되면 적절한 스크립트가 해결됩니다.

1. 스크립트는 원하는 대로 요청 또는 리소스를 조정합니다 [!DNL Sling Model].

1. 스크립트는 [!DNL Sling Model] HTML 변환을 생성할 객체입니다.

1. 스크립트에서 생성된 HTML은 HTTP 응답에서 반환됩니다.

이 기존 패턴은 HTML을 [!DNL Sling Model] htl을 통해 쉽게 활용할 수 있습니다. HTL이 이러한 형식의 정의에 자연스럽게 도움이 되지 않으므로 JSON 또는 XML과 같이 더 구조화된 데이터를 만드는 것은 훨씬 더 번거로운 작업입니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter] Sling은 Jackson Exporter에게 &quot;평범함&quot;을 자동으로 serialize하는 Sling을 제공합니다 [!DNL Sling Model] 개체를 JSON에 포함합니다. Jackson Exporter는 구성 가능한 반면, Jackson Exporter의 코어에서는 [!DNL Sling Model] 및 는 JSON 키로 &quot;getter&quot; 메서드를 사용하고 getter 반환 값을 JSON 값으로 사용하여 JSON을 생성합니다.

의 직접 직렬화 [!DNL Sling Models] 이를 통해 일반 웹 요청을 모두 기존의 [!DNL Sling Model] 요청 흐름(위 참조)은 물론, 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현도 노출합니다.

![Sling 모델 내보내기 HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 흐름은 제공된 Jackson Exporter를 사용하여 JSON 출력을 생성하는 흐름에 대해 설명합니다. 사용자 지정 내보내기 사용은 동일한 플로우를 따르지만 출력 형식을 따릅니다.*

1. HTTP GET 요청은 선택기와 확장이 [!DNL Sling Model]&#39;s Exporter.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling은 요청된 리소스의 `sling:resourceType`, 선택기 및 확장을 동적으로 생성된 Sling Exporter 서블릿에 매핑하고 [!DNL Sling Model] 익스포터 사용.
1. 해결된 Sling 내보내기 서블릿은 [!DNL Sling Model Exporter] 반대 [!DNL Sling Model] Sling 모델 adaptables에 의해 결정된 대로 요청 또는 리소스에서 채택된 객체입니다.
1. 수출자는 [!DNL Sling Model] 내보내기 옵션 및 내보내기 관련 Sling 모델 주석을 기반으로 결과를 Sling 내보내기 서블릿으로 반환합니다.
1. Sling Exporter 서블릿은 [!DNL Sling Model] 를 반환합니다.

>[!NOTE]
>
>반면 Apache Sling 프로젝트는 Jackson Exporter를 serialize하는 기능을 제공합니다 [!DNL Sling Models] JSON에 대해 내보내기 프레임워크는 사용자 지정 Exporter도 지원합니다. 예를 들어, 프로젝트는 를 직렬화하는 사용자 지정 익스포터를 구현할 수 있습니다 [!DNL Sling Model] XML로 전환합니다.

>[!NOTE]
>
>뿐만 아니라 [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]를 Java 객체로 내보낼 수도 있습니다. 다른 Java 객체로 내보내기는 HTTP 요청 플로우에서 역할을 수행하지 않으므로 위의 다이어그램에 표시되지 않습니다.

## 지원 자료

* [Apache [!DNL Sling Model Exporter] 프레임워크 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
