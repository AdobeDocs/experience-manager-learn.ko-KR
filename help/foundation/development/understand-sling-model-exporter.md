---
title: AEM의 Sling 모델 내보내기 이해
description: Apache Sling 모델 1.3.0은 Sling 모델 개체를 사용자 지정 추상으로 내보내거나 직렬화하는 우아한 방법인 Sling 모델 내보내기를 도입했습니다. 이 문서에서는 Sling 모델을 사용하여 HTL 스크립트를 채우는 기존의 사용 사례를 Sling 모델 내보내기 프레임워크를 활용하여 Sling 모델을 JSON으로 직렬화하는 방법과 나란히 다룹니다.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# [!DNL Sling Model Exporter] 이해

Apache [!DNL Sling Models] 1.3.0에서는 [!DNL Sling Model] 개체를 사용자 지정 추상으로 내보내거나 직렬화하는 우아한 방법인 [!DNL Sling Model Exporter]을(를) 도입했습니다. 이 문서에서는 [!DNL Sling Models]을(를) 사용하여 HTL 스크립트를 채우는 기존 사용 사례를 [!DNL Sling Model Exporter] 프레임워크를 활용하여 [!DNL Sling Model]을(를) JSON으로 직렬화하는 것과 병치합니다.

## 기존 Sling 모델 HTTP 요청 흐름

[!DNL Sling Models]의 기존 사용 사례는 리소스 또는 요청에 대한 비즈니스 추상화를 제공하는 것입니다. 이 리소스 또는 요청은 HTL 스크립트(또는 이전 JSP)에 비즈니스 기능에 액세스하기 위한 인터페이스를 제공합니다.

일반적인 패턴은 AEM 구성 요소 또는 페이지를 나타내는 [!DNL Sling Models]을(를) 개발하고 [!DNL Sling Model] 개체를 사용하여 브라우저에 표시되는 HTML의 최종 결과와 함께 HTL 스크립트에 데이터를 제공하는 것입니다.

### Sling 모델 HTTP 요청 흐름

![Sling 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. AEM의 리소스에 대해 [!DNL HTTP GET] 요청이 이루어졌습니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스의 `sling:resourceType`을(를) 기반으로 적절한 스크립트가 확인됩니다.

1. 스크립트는 요청 또는 리소스를 원하는 [!DNL Sling Model]에 맞게 조정합니다.

1. 스크립트는 [!DNL Sling Model] 개체를 사용하여 HTML 렌디션을 생성합니다.

1. 스크립트에서 생성한 HTML이 HTTP 응답에서 반환됩니다.

HTL을 통해 [!DNL Sling Model]을(를) 쉽게 활용할 수 있으므로 이 기존 패턴은 HTML 생성 컨텍스트에서 잘 작동합니다. HTL이 이러한 형식의 정의에 자연스럽게 영향을 주지 않기 때문에 JSON 또는 XML과 같은 보다 구조화된 데이터를 만드는 것은 훨씬 지루한 작업입니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter]에는 &quot;일반&quot; [!DNL Sling Model] 개체를 JSON으로 자동으로 직렬화하는 Jackson Exporter가 제공된 Sling이 포함되어 있습니다. Jackson Exporter는 구성 가능한 반면 코어에서 [!DNL Sling Model] 개체를 검사하고 &quot;getter&quot; 메서드를 JSON 키로 사용하여 JSON을 생성하며 getter 반환 값은 JSON 값으로 생성됩니다.

[!DNL Sling Models]을(를) 직접 직렬화하면 일반 웹 요청을 기존 [!DNL Sling Model] 요청 흐름(위 참조)을 사용하여 만든 HTML 응답으로 모두 서비스할 수 있지만 웹 서비스나 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현물을 표시할 수도 있습니다.

![Sling 모델 내보내기 HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 흐름은 제공된 Jackson 익스포터를 사용하여 JSON 출력을 생성하는 흐름을 설명합니다. 사용자 지정 내보내기를 사용하면 흐름은 동일하지만 출력 형식이 사용됩니다.*

1. [!DNL Sling Model]의 Exporter에 등록된 선택기 및 확장이 있는 AEM의 리소스에 대해 HTTP GET 요청이 이루어집니다.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling은 요청된 리소스의 `sling:resourceType`, 선택기 및 확장을 내보내기가 있는 [!DNL Sling Model]에 매핑된 동적으로 생성된 Sling 내보내기 서블릿으로 확인합니다.
1. 해결된 Sling 내보내기 서블릿은 요청 또는 리소스에서 조정된 [!DNL Sling Model] 개체에 대해 [!DNL Sling Model Exporter]을(를) 호출합니다(Sling 모델 적응성에 의해 결정됨).
1. 내보내기는 내보내기 옵션 및 내보내기별 Sling 모델 주석을 기반으로 [!DNL Sling Model]을(를) 정리하고 결과를 Sling 내보내기 서블릿에 반환합니다.
1. Sling 내보내기 서블릿이 HTTP 응답에서 [!DNL Sling Model]의 JSON 렌디션을 반환합니다.

>[!NOTE]
>
>Apache Sling 프로젝트는 [!DNL Sling Models]을(를) JSON으로 직렬화하는 Jackson Exporter를 제공하지만, Exporter 프레임워크는 사용자 지정 Exporter도 지원합니다. 예를 들어 프로젝트는 [!DNL Sling Model]을(를) XML로 직렬화하는 사용자 지정 Exporter를 구현할 수 있습니다.

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]뿐만 아니라 Java 개체로 내보낼 수도 있습니다. 다른 Java 개체로 내보내기는 HTTP 요청 흐름에서 역할을 하지 않으므로 위의 다이어그램에 나타나지 않습니다.

## 지원 자료

* [Apache [!DNL Sling Model Exporter] 프레임워크 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
