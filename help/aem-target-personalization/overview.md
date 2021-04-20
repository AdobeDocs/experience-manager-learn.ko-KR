---
title: AEM 및 Adobe Target 시작하기
seo-title: AEM 및 Adobe Target 시작하기
description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 제작 및 전달하는 방법을 소개하는 종합적인 자습서입니다. 이 자습서에서는 엔드 투 엔드 프로세스와 관련된 다양한 성향과 이들이 어떻게 서로 협업하는지도 살펴봅니다
seo-description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 제작 및 전달하는 방법을 소개하는 종합적인 자습서입니다. 이 자습서에서는 엔드 투 엔드 프로세스와 관련된 다양한 성향과 이들이 어떻게 서로 협업하는지도 살펴봅니다
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 2%

---


# AEM 및 Adobe Target 시작하기 {#getting-started-with-aem-target}

AEM과 Target은 모두 기능이 중복되는 강력한 솔루션입니다. 고객은 경우에 따라 개인화된 경험을 제공하기 위해 이러한 제품을 어떻게 언제 어떻게 사용하는지 파악해야 합니다. 모든 최종 사용자를 위해 최적화된 경험을 전달하려면 조직 내의 여러 팀이 긴밀하게 작업하고 작업을 수행하는 사용자를 정의해야 합니다.

이 튜토리얼에서는 AEM 및 Target에 대한 3가지 시나리오를 다룹니다. 이러한 시나리오는 조직에 가장 적합한 항목과 서로 다른 팀이 협업하는 방법을 이해하는 데 도움이 됩니다.

* 시나리오 1:AEM 경험 조각을 사용한 개인화
* 시나리오 2:Visual Experience Composer를 사용한 개인화
* 시나리오 3:전체 웹 페이지 경험의 개인화

## AEM 경험 조각을 사용한 개인화 {#personalization-using-aem-experience-fragment}

이 시나리오의 경우 AEM 및 Target을 사용할 것입니다. 두 제품 모두 고유한 장점을 가지고 있으며, 사이트의 사용자에게 개인화된 경험을 제공하는 데 있어서 특정 사용자를 기반으로 이러한 컨텐츠를 제공하는 데 **개인화된 컨텐츠(AEM의 컨텐츠)** 및 **지능형 방법(Target)**&#x200B;이 필요합니다.

AEM을 사용하면 개인화된 콘텐츠를 제작하여 모든 콘텐츠와 자산을 중앙에서 통합하여 개인화 전략에 도움이 됩니다. AEM을 사용하면 코드를 작성하지 않고도 데스크탑, 태블릿 및 모바일 디바이스에 배포할 컨텐츠를 한 곳에서 손쉽게 제작할 수 있습니다. 모든 디바이스에 맞는 페이지를 제작할 필요가 없습니다. AEM은 컨텐츠를 사용하여 각 경험을 자동으로 조정합니다. 단추 푸시를 사용하여 AEM에서 Adobe Target으로 컨텐츠를 내보낼 수도 있습니다.

이제 Target의 AEM에서 제공하는 오퍼 형태로 개인화된 컨텐츠가 제공됩니다. Target을 사용하면 행동, 컨텍스트 및 오프라인 변수를 결합하는 규칙 기반의 머신 러닝 방식과 AI 기반의 머신 러닝 방식을 결합하여 이러한 제안을 규모에 맞게 제공할 수 있습니다.  Target을 사용하면 A/B 및 MVT(Multivariate Variate) 활동을 손쉽게 설정하고 실행하여 최상의 오퍼, 컨텐츠 및 경험을 결정할 수 있습니다.

**경험** 조각은 컨텐츠/경험 작성자를 Target을 사용하여 비즈니스 결과를 추진하는 개인화 전문가와 연결시키기 위한 엄청난 진보를 제공합니다.

* AEM 컨텐츠 편집자는 맞춤형 컨텐츠를 경험 조각 및 변형으로 제작합니다.
* AEM은 경험 조각 HTML을 Target으로 &#x200B; 내보냅니다.
* Target&#x200B;은 활동에서 오퍼로 AEM 경험 조각 마크업을 사용합니다.
* Target은 경험 조각 HTML을 전달하고 AEM은 참조된 이미지를 제공합니다.

   ![경험 조각 다이어그램을 사용한 개인화](assets/personalization-use-case-1/use-case-1-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)
* [이전 Cloud Services을 사용하는 AEM 및 Adobe Target](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를 자세히  [살펴보십시오](./personalization-use-case-1.md).***

## Visual Experience Composer를 사용한 개인화

마케터는 Adobe Target VEC(Visual Experience Composer)를 사용하여 테스트를 실행하기 위해 코드를 변경하지 않고도 웹 사이트에서 빠르게 변경할 수 있습니다. VEC는 사이트 컨텍스트에서 개인화된 경험과 오퍼를 쉽게 만들고 테스트할 수 있는 WYSIWYG(What you see is what you get) 사용자 인터페이스입니다. 웹 페이지(또는 오퍼) 또는 모바일 웹 페이지의 레이아웃 및 컨텐츠를 드래그 앤 드롭하거나 교체하고 수정하여 Target 활동에 대한 경험과 오퍼를 만들 수 있습니다.

VEC는 Adobe Target의 주요 기능 중 하나입니다. VEC를 사용하면 마케터와 디자이너는 시각적인 인터페이스를 사용하여 컨텐츠를 만들고 변경할 수 있습니다. 코드를 직접 편집하지 않고도 다양한 디자인 옵션을 선택할 수 있습니다. 작성기에서 사용할 수 있는 편집 옵션을 사용하여 HTML 및 JavaScript를 편집할 수도 있습니다.

* 컨텐츠는 AEM에 있고 컨텐츠 편집자는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다.
* 개인화된 컨텐츠 제공 Target
* Adobe Target VEC를 사용하여 새 컨텐츠 순 만들기
* AEM 호스팅 사이트와 AEM이 아닌 호스팅 사이트에 모두 적용됩니다.

   ![Visual Experience Composer 다이어그램을 사용한 개인화](assets/personalization-use-case-3/use-case-diagram-3.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를 자세히  [살펴보십시오.](./personalization-use-case-3.md)***

## 전체 웹 페이지 경험의 개인화

Adobe Experience Manager과 Adobe Target의 통합을 통해 사이트 사용자에게 개인화된 경험을 제공할 수 있습니다. 또한 지정된 테스트 기간 동안 전환율을 가장 향상시킬 수 있는 웹 사이트 컨텐츠 버전을 보다 잘 파악할 수 있습니다. 예를 들어 A/B 테스트는 두 개 이상의 웹 사이트 컨텐츠 버전을 비교하여 전환, 판매 또는 식별된 기타 지표를 가장 잘 높여주는 버전을 확인합니다. 마케터는 Adobe Target 내에서 활동을 만들어 사용자가 사이트 컨텐츠와 상호 작용하는 방법 및 사이트 지표에 미치는 영향을 이해할 수 있습니다.

* 컨텐츠는 AEM에 있고 컨텐츠 편집자는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다.
* 개인화된 컨텐츠 제공 Target
* 여기에서 새 콘텐츠를 새로 만들지 않았습니다.
* AEM 사이트와 AEM 이외의 사이트에 모두 적용

   ![다이어그램](assets/personalization-use-case-2/use-case-2-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를 자세히  [살펴보십시오.](./personalization-use-case-2.md)***
