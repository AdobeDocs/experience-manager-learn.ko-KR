---
title: AEM에서 Sling 모델 내보내기 프로그램 이해
description: Apache Sling Models 1.3.0에는 Sling Model 객체를 사용자 정의 추상형으로 내보내거나 직렬화할 수 있는 세련된 방법인 Sling Model Exporter가 도입되었습니다. 이 문서에서는 Sling Model Exporter 프레임워크를 활용하여 Sling 모델을 JSON으로 일련화하는 것과 함께 Sling Models를 사용하여 HTML 스크립트를 채우는 일반적인 사용 사례를 소개합니다.
version: 6.3, 6.4, 6.5
sub-product: 기반, 컨텐츠 서비스
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 0%

---


# [!DNL Sling Model Exporter] 이해

Apache [!DNL Sling Models] 1.3.0에는 [!DNL Sling Model] 개체를 사용자 정의 추상체로 내보내거나 직렬화하는 세련된 방법인 [!DNL Sling Model Exporter]이 도입되었습니다. 이 문서는 [!DNL Sling Model Exporter] 프레임워크를 활용하여 [!DNL Sling Model]을(를) JSON에 일련화하기 위해 [!DNL Sling Models]을(를) 사용하여 HTL 스크립트를 채우는 일반적인 사용 사례와 비교됩니다.

## 기존 Sling 모델 HTTP 요청 흐름

[!DNL Sling Models]의 일반적인 사용 사례는 비즈니스 기능에 액세스하기 위한 인터페이스를 제공하는 리소스 또는 요청에 대한 비즈니스 추상화를 제공하는 것입니다.

일반적인 패턴은 AEM 구성 요소 또는 페이지를 나타내는 [!DNL Sling Models]을 개발하고 있으며, 브라우저에 표시되는 HTML의 최종 결과와 함께 [!DNL Sling Model] 개체를 사용하여 데이터를 HTL 스크립트에 제공합니다.

### Sling 모델 HTTP 요청 흐름

![슬링 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM의 리소스에 대한 요청이 수행됩니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스의 `sling:resourceType`에 따라 적절한 스크립트가 해결됩니다.

1. 스크립트는 요청 또는 리소스를 원하는 [!DNL Sling Model]에 맞게 조정합니다.

1. 스크립트는 [!DNL Sling Model] 개체를 사용하여 HTML 변환을 생성합니다.

1. 스크립트로 생성된 HTML은 HTTP 응답에서 반환됩니다.

이 기존 패턴은 HTML 생성 컨텍스트에서 잘 작동하므로 [!DNL Sling Model]은 HTL을 통해 쉽게 활용할 수 있습니다. HTL이 이러한 형식의 정의에 자연스럽게 영향을 주지 않으므로 JSON 또는 XML과 같이 구조화된 데이터를 더 많이 생성하는 것은 훨씬 더 번거로운 작업입니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter]에는 &quot;ordinary&quot; [!DNL Sling Model] 개체를 JSON으로 자동으로 직렬화하는 Jackson Exporter가 제공하는 Sling이 포함되어 있습니다. 구성 가능한 Jackson Exporter는 기본적으로 [!DNL Sling Model] 개체를 검사하여 &quot;getter&quot; 메서드를 JSON 키로 사용하고 getter 반환 값을 JSON 값으로 사용하여 JSON을 생성합니다.

[!DNL Sling Models]의 직접 정리를 사용하면 일반 [!DNL Sling Model] 요청 흐름(위 참조)을 사용하여 만든 HTML 응답과 함께 일반적인 웹 요청을 모두 서비스할 수 있고 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현도 표시할 수 있습니다.

![Sling 모델 내보내기 HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 흐름은 제공된 Jackson Exporter를 사용하여 JSON 출력을 생성하는 흐름에 대해 설명합니다. 사용자 지정 내보내기 프로그램의 사용은 동일한 흐름을 따르지만 출력 형식과 함께 합니다.*

1. HTTP GET 요청은 선택기와 확장이 [!DNL Sling Model] 내보내기 도구에 등록된 AEM의 리소스에 대해 수행됩니다.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling은 요청된 리소스의 `sling:resourceType`, 선택기 및 확장을 Exporter와 함께 [!DNL Sling Model]에 매핑되는 동적으로 생성된 Sling Exporter Servlet으로 해결합니다.
1. 해결된 Sling Exporter Servlet은 요청이나 리소스에서 채택된 [!DNL Sling Model] 개체에 대해 [!DNL Sling Model Exporter]을 호출합니다(Sling Models 적응표로 결정됨).
1. 내보내기는 [내보내기 옵션] 및 [내보내기 전용 Sling 모델] 주석을 기반으로 [!DNL Sling Model]을(를) 직렬화하고 결과를 Sling Exporter Servlet에 반환합니다.
1. Sling 내보내기 서블릿은 HTTP 응답에서 [!DNL Sling Model]의 JSON 변환을 반환합니다.

>[!NOTE]
>
>Apache Sling 프로젝트는 [!DNL Sling Models]을(를) JSON에 직렬화하는 Jackson Exporter를 제공하지만 Exporter 프레임워크도 사용자 지정 Proporter를 지원합니다. 예를 들어 프로젝트는 [!DNL Sling Model]을(를) XML로 직렬화하는 사용자 지정 익스포터를 구현할 수 있습니다.

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]뿐만 아니라 Java 객체로 내보낼 수도 있습니다. 다른 Java 객체로 내보내기는 HTTP 요청 플로우에서 역할을 수행하지 않으므로 위의 다이어그램에 나타나지 않습니다.

## 지원 자료

* [ [!DNL Sling Model Exporter] ApacheFramework 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
