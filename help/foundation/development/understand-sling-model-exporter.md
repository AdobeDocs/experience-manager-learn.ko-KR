---
title: AEM의 Sling Model Exporter 이해
description: Apache Sling Models 1.3.0에서는 Sling Model 개체를 사용자 정의 추상체로 내보내거나 직렬화할 수 있는 세련된 방법인 Sling Model Exporter를 도입합니다. 이 문서에서는 Sling Model Exporter 프레임워크를 활용하여 Sling Model을 JSON으로 일련화하는 것과 함께 Sling Models를 사용하여 HTL 스크립트를 채우는 일반적인 사용 사례를 소개합니다.
version: 6.3, 6.4, 6.5
sub-product: 기반, 컨텐츠 서비스
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# 이해 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 [!DNL Sling Model Exporter]은 개체를 사용자 정의 추상체로 내보내거나 직렬화할 수 있는 세련된 [!DNL Sling Model] 방법입니다. 이 문서에서는 HTML 스크립트 [!DNL Sling Models] 를 채우는 데 사용하는 기존의 사용 사례와 JSON에 [!DNL Sling Model Exporter] 프레임워크를 활용하여 [!DNL Sling Model] JSON에 정리합니다.

## 기존 슬링 모델 HTTP 요청 흐름

일반적인 사용 사례로는 리소스 또는 요청에 대한 비즈니스 추상 [!DNL Sling Models] 을 제공하여 비즈니스 기능에 액세스하기 위한 인터페이스를 HTL 스크립트(또는 이전 JSP)에 제공합니다.

일반적인 패턴은 AEM 구성 요소 또는 페이지 [!DNL Sling Models] [!DNL Sling Model] 를 나타내는 개발 중이며, 개체를 사용하여 데이터를 HTL 스크립트와 함께 브라우저에 표시되는 HTML의 최종 결과를 제공합니다.

### Sling 모델 HTTP 요청 흐름

![슬링 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM의 리소스에 대한 요청이 수행됩니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스 `sling:resourceType`에 따라 적절한 스크립트가 해결됩니다.

1. 스크립트는 요청이나 리소스를 원하는 대로 조정합니다 [!DNL Sling Model].

1. 스크립트에서는 [!DNL Sling Model] 개체를 사용하여 HTML 변환을 생성합니다.

1. 스크립트로 생성된 HTML은 HTTP 응답으로 반환됩니다.

이러한 기존 패턴은 HTML을 생성하는 작업 환경에서 잘 작동하므로 HTL을 통해 손쉽게 활용할 [!DNL Sling Model] 수 있습니다. JSON 또는 XML과 같이 구조화된 데이터를 더 많이 생성하는 것은 HTL이 이러한 포맷의 정의에 자연스럽게 영향을 주지 않기 때문에 훨씬 더 지루하게 느껴집니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter] 는 Jackson Exporter가 &quot;일반&quot; 개체를 JSON으로 자동으로 일련화하는 Sling [!DNL Sling Model] 을 제공합니다. Jackson Exporter는 구성 가능한 반면, 핵심 인스턴스에서 개체를 검사하여 &quot;getter&quot; 메서드를 JSON 키로 사용하고 getter 반환 값을 JSON 값으로 사용하여 JSON을 생성합니다. [!DNL Sling Model]

직접 정리를 [!DNL Sling Models] 사용하면 일반 [!DNL Sling Model] 요청 흐름(위 참조)을 사용하여 만든 HTML 응답으로 일반 웹 요청을 모두 처리할 수 있고 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현물도 노출할 수 있습니다.

![Sling Model Exporter HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 흐름에서는 제공된 Jackson Exporter를 사용하여 JSON 출력을 생성하는 흐름을 설명합니다. 사용자 지정 수출업체 사용은 동일한 흐름을 따르지만 출력 포맷을 따릅니다.*

1. HTTP GET 요청은 선택기와 확장이 Exporter에 등록된 AEM의 리소스에 대해 [!DNL Sling Model]수행됩니다.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling은 요청된 리소스, 선택기 및 확장자를 Exporter와 매핑되는 동적으로 생성된 Sling Exporter Servlet `sling:resourceType`[!DNL Sling Model] 으로 해결합니다.
1. 해결된 Sling Exporter 서블릿은 요청 또는 리소스에서 채택된 [!DNL Sling Model Exporter] [!DNL Sling Model] 객체에 대해 호출합니다(Sling Models 어댑터 테이블에 의해 결정됨).
1. 내보내기는 내보내기 옵션 및 내보내기 전용 Sling 모델 주석을 [!DNL Sling Model] 기반으로 하여 일련화하고 결과를 Sling Exporter 서블릿으로 반환합니다.
1. Sling Exporter Servlet은 HTTP 응답에 있는 JSON 변환 [!DNL Sling Model] 을 반환합니다.

>[!NOTE]
>
>Apache Sling 프로젝트는 JSON에 serialize하는 Jackson Exporter [!DNL Sling Models] 를 제공하지만 Exporter 프레임워크도 사용자 지정 Proporter를 지원합니다. 예를 들어, 프로젝트는 XML로 직렬화하는 사용자 지정 익스포터를 구현할 수 [!DNL Sling Model] 있습니다.

>[!NOTE]
>
>일련화 [!DNL Sling Model Exporter] 는 *물론* Java 개체로도 내보낼 수 있습니다 [!DNL Sling Models]. 다른 Java 개체로 내보내기는 HTTP 요청 플로우에서 역할을 수행하지 않으므로 위 다이어그램에 나타나지 않습니다.

## 지원 자료

* [ [!DNL Sling Model Exporter] ApacheFramework 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
