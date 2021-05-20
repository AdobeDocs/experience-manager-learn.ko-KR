---
title: AEM의 Sling 모델 내보내기 이해
description: Apache Sling Models 1.3.0에서는 Sling Model 개체를 사용자 정의 추상화로 내보내거나 직렬화하는 우아한 방법인 Sling Model Exporter가 도입되었습니다. 이 문서에서는 Sling 모델을 사용하여 HTL 스크립트를 채우는 일반적인 사용 사례와 함께 Sling 모델 내보내기 프레임워크를 사용하여 Sling 모델을 JSON으로 serialize하는 방법을 사용합니다.
version: 6.3, 6.4, 6.5
sub-product: foundation, content services
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# [!DNL Sling Model Exporter] 이해

Apache [!DNL Sling Models] 1.3.0에서는 [!DNL Sling Model Exporter] 개체를 사용자 지정 추상화로 내보내거나 serialize하는 우아한 방법인 [!DNL Sling Model]을 도입했습니다. 이 문서는 [!DNL Sling Model Exporter] 프레임워크를 활용하여 [!DNL Sling Model]를 JSON으로 serialize하는 것과 함께 [!DNL Sling Models]을 사용하여 HTL 스크립트를 채우는 것과 관련된 일반적인 사용 사례를 병치합니다.

## 기존 Sling 모델 HTTP 요청 흐름

기존 [!DNL Sling Models] 사용 사례는 비즈니스 기능에 액세스하기 위한 인터페이스를 제공하는 리소스 또는 요청에 대한 비즈니스 추상화를 제공하는 것입니다.

일반적인 패턴은 AEM 구성 요소 또는 페이지를 나타내는 [!DNL Sling Models]을 개발하고, [!DNL Sling Model] 개체를 사용하여 브라우저에 표시되는 HTML의 최종 결과를 사용하여 HTL 스크립트를 데이터로 제공합니다.

### Sling 모델 HTTP 요청 흐름

![Sling 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM의 리소스에 대해 요청이 수행되었습니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스의 `sling:resourceType`에 따라 적절한 스크립트가 해결됩니다.

1. 스크립트는 원하는 [!DNL Sling Model]에 요청 또는 리소스를 적용합니다.

1. 스크립트는 [!DNL Sling Model] 개체를 사용하여 HTML 변환을 생성합니다.

1. 스크립트에서 생성한 HTML은 HTTP 응답에서 반환됩니다.

이러한 기존 패턴은 HTL을 통해 [!DNL Sling Model] 을 쉽게 활용할 수 있으므로 HTML 생성 컨텍스트에서 잘 작동합니다. HTL이 이러한 형식의 정의에 자연스럽게 도움이 되지 않으므로 JSON 또는 XML과 같이 더 구조화된 데이터를 만드는 것은 훨씬 더 번거로운 작업입니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter]에는 &quot;일반&quot; [!DNL Sling Model] 개체를 자동으로 JSON으로 serialize하는 Jackson Exporter가 제공된 Sling이 포함되어 있습니다. Jackson Exporter는 구성 가능할 뿐만 아니라 코어에서 [!DNL Sling Model] 개체를 검사하고, &quot;getter&quot; 메서드를 JSON 키로 사용하여 JSON을 생성하고, getter 반환 값을 JSON 값으로 생성합니다.

[!DNL Sling Models]의 직접 직렬화를 사용하면 일반 [!DNL Sling Model] 요청 플로우를 사용하여 작성된 HTML 응답으로 두 개의 일반적인 웹 요청을 모두 처리할 수 있지만(위 참조), 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현물을 노출할 수도 있습니다.

![Sling 모델 내보내기 HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 플로우에서는 제공된 Jackson Exporter를 사용하여 JSON 출력을 생성하는 플로우에 대해 설명합니다. 사용자 지정 내보내기 사용은 같은 흐름을 따르지만 출력 형식과 함께 사용합니다.*

1. HTTP GET 요청은 [!DNL Sling Model] 의 Exporter에 등록된 선택기 및 확장이 있는 AEM의 리소스에 대해 수행됩니다.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling은 요청된 리소스의 `sling:resourceType`, 선택기 및 확장을 Exporter와 함께 [!DNL Sling Model]에 매핑된 동적으로 생성된 Sling Exporter 서블릿으로 확인합니다.
1. 해결된 Sling Exporter 서블릿은 요청 또는 리소스에서 채택된 [!DNL Sling Model] 개체(Sling Models 어댑터 테이블에 의해 결정됨)에 대해 [!DNL Sling Model Exporter]을 호출합니다.
1. 익스포터는 내보내기 옵션 및 익스포터별 Sling 모델 주석을 기반으로 [!DNL Sling Model]을 serialize하고 결과를 Sling Exporter 서블릿에 반환합니다.
1. Sling Exporter Servlet은 HTTP 응답에서 [!DNL Sling Model]의 JSON 변환을 반환합니다.

>[!NOTE]
>
>Apache Sling 프로젝트가 [!DNL Sling Models]을 JSON에 serialize하는 Jackson Exporter를 제공하는 반면, Exporter 프레임워크도 사용자 지정 Exporter를 지원합니다. 예를 들어, 프로젝트는 [!DNL Sling Model]을 XML로 직렬화하는 사용자 지정 익스포터를 구현할 수 있습니다.

>[!NOTE]
>
>는 [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]뿐만 아니라 Java 개체로 내보낼 수도 있습니다. 다른 Java 객체로 내보내기는 HTTP 요청 플로우에서 역할을 수행하지 않으므로 위의 다이어그램에 표시되지 않습니다.

## 지원 자료

* [ [!DNL Sling Model Exporter] ApacheFramework 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
