---
title: AEM의 Sling 모델 내보내기 이해
description: Apache Sling 모델 1.3.0은 Sling 모델 개체를 사용자 지정 추상으로 내보내거나 직렬화하는 우아한 방법인 Sling 모델 내보내기를 도입했습니다. 이 문서에서는 Sling 모델을 사용하여 HTL 스크립트를 채우는 기존의 사용 사례를 Sling 모델 내보내기 프레임워크를 활용하여 Sling 모델을 JSON으로 직렬화하는 방법과 나란히 다룹니다.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# 이해 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 도입 [!DNL Sling Model Exporter], 내보내기 또는 직렬화를 위한 우아한 방법 [!DNL Sling Model] 개체를 사용자 지정 추상화에 추가합니다. 이 문서에서는 의 전통적인 사용 사례를 병과합니다 [!DNL Sling Models] 을 활용하여 HTL 스크립트 채우기 [!DNL Sling Model Exporter] serialize할 프레임워크 [!DNL Sling Model] JSON에

## 기존 Sling 모델 HTTP 요청 흐름

의 기존 사용 사례 [!DNL Sling Models] 는 리소스 또는 요청에 대한 비즈니스 추상화를 제공하며, 이는 비즈니스 기능에 액세스하기 위한 인터페이스를 HTL 스크립트(또는 이전의 JSP)에 제공합니다.

일반적인 패턴이 발달하고 있습니다 [!DNL Sling Models] AEM 구성 요소 또는 페이지를 나타내고 [!DNL Sling Model] 브라우저에 표시되는 HTML의 최종 결과와 함께 HTL 스크립트에 데이터를 제공하는 개체입니다.

### Sling 모델 HTTP 요청 흐름

![Sling 모델 요청 흐름](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM의 리소스에 대해 요청이 수행됩니다.

   예: `HTTP GET /content/my-resource.html`

1. 요청 리소스 `sling:resourceType`를 클릭하여 적절한 스크립트가 해결됩니다.

1. 스크립트는 요청 또는 리소스를 원하는 대로 조정합니다 [!DNL Sling Model].

1. 스크립트는 [!DNL Sling Model] HTML 렌디션을 생성하기 위한 개체입니다.

1. 스크립트에서 생성된 HTML이 HTTP 응답에서 반환됩니다.

이러한 기존 패턴은 다음과 같이 HTML을 생성하는 맥락에서 잘 작동합니다. [!DNL Sling Model] htl을 통해 쉽게 활용할 수 있습니다. HTL이 이러한 형식의 정의에 자연스럽게 영향을 주지 않기 때문에 JSON 또는 XML과 같은 보다 구조화된 데이터를 만드는 것은 훨씬 지루한 작업입니다.

## [!DNL Sling Model Exporter] HTTP 요청 흐름

Apache [!DNL Sling Model Exporter] Jackson Exporter가 제공하는 Sling이 함께 제공됩니다. 이 제품은 자동으로 &quot;일반&quot;을 직렬화합니다. [!DNL Sling Model] JSON에 있는 객체입니다. Jackson Exporter는 구성이 매우 용이하지만 핵심에서 다음을 검사합니다. [!DNL Sling Model] 는 &quot;getter&quot; 메서드를 JSON 키로 사용하여 JSON을 생성하고 getter 반환 값을 JSON 값으로 생성합니다.

의 직접 일련화 [!DNL Sling Models] 에서는 일반 웹 요청을 일반 웹 요청을 사용하여 만든 HTML 응답으로 모두 서비스할 수 있습니다 [!DNL Sling Model] 요청 흐름(위 참조)은 웹 서비스 또는 JavaScript 애플리케이션에서 사용할 수 있는 JSON 표현물을 노출하기도 합니다.

![Sling 모델 내보내기 HTTP 요청 흐름](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*이 플로우는 제공된 Json 익스포터를 사용하여 JSON 출력을 생성하는 플로우를 설명합니다. 사용자 지정 내보내기 사용은 동일한 플로우를 따르지만 출력 형식을 사용합니다.*

1. 선택기와 확장이 와(과) 함께 등록된 AEM의 리소스에 대해 HTTP GET 요청이 이루어짐 [!DNL Sling Model]의 내보내기.

   예: `HTTP GET /content/my-resource.model.json`

1. Sling이 요청된 리소스의 을(를) 확인합니다. `sling:resourceType`, 선택기 및 확장자에 동적으로 생성된 Sling 내보내기 서블릿에 매핑됩니다. [!DNL Sling Model] 내보내기 사용.
1. 해결된 Sling 내보내기 서블릿은 [!DNL Sling Model Exporter] 에 대해 [!DNL Sling Model] 요청 또는 리소스에서 조정된 객체(Sling 모델 적응성에 의해 결정됨).
1. 내보내기는 [!DNL Sling Model] 내보내기 옵션 및 내보내기 전용 슬링 모델 주석을 기반으로 하며 결과를 슬링 내보내기 서블릿에 반환합니다.
1. Sling 내보내기 서블릿이 의 JSON 표현물을 반환합니다. [!DNL Sling Model] HTTP 응답.

>[!NOTE]
>
>Apache Sling 프로젝트는 Jackson Exporter를 통해 [!DNL Sling Models] json에 대해 내보내기 프레임워크는 사용자 지정 내보내기 도 지원합니다. 예를 들어 프로젝트는 다음을 직렬화하는 사용자 지정 내보내기 를 구현할 수 있습니다. [!DNL Sling Model] XML로.

>[!NOTE]
>
>다음을 수행할 뿐만 아니라 [!DNL Sling Model Exporter] *직렬화* [!DNL Sling Models]을 사용하여 Java 개체로 내보낼 수도 있습니다. 다른 Java 개체로 내보내기는 HTTP 요청 흐름에서 역할을 하지 않으므로 위의 다이어그램에 나타나지 않습니다.

## 지원 자료

* [Apache [!DNL Sling Model Exporter] 프레임워크 설명서](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
