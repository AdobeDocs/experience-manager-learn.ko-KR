---
title: AEM 및 Adobe Target 시작하기
seo-title: AEM 및 Adobe Target 시작하기
description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다. 또한 이 자습서에서는 종단 간 프로세스와 관련된 다양한 성향과 이들이 서로 협력하는 방법에 대해서도 학습합니다
seo-description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다. 또한 이 자습서에서는 종단 간 프로세스와 관련된 다양한 성향과 이들이 서로 협력하는 방법에 대해서도 학습합니다
feature: 경험 구성요소
topic: 개인화
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 2%

---


# AEM 및 Adobe Target 시작하기 {#getting-started-with-aem-target}

AEM과 Target은 모두 기능이 중복되는 강력한 솔루션입니다. 고객은 이러한 제품을 어떻게 언제 사용하는지 파악하여 개인화된 경험을 전달할 수 있습니다. 모든 최종 사용자에게 최적화된 경험을 전달하려면 조직 내의 다양한 팀이 긴밀하게 작업하고 누가 어떤 작업을 수행하는지 정의해야 합니다.

이 자습서에서는 조직에 가장 적합한 내용과 다른 팀이 협력하는 방법을 이해하는 데 도움이 되는 AEM 및 Target에 대한 세 가지 다른 시나리오를 다룹니다.

* 시나리오 1 :AEM 경험 조각을 사용한 개인화
* 시나리오 2 :Visual Experience Composer를 사용한 개인화
* 시나리오 3 :전체 웹 페이지 경험의 개인화

## AEM 경험 구성요소 {#personalization-using-aem-experience-fragment}를 사용한 개인화

이 시나리오에서는 AEM 및 Target을 사용합니다. 두 제품 모두 나름의 장점을 가지고 있으며, 사이트의 사용자에게 개인화된 경험을 전달하는 데 있어서 특정 사용자를 기반으로 이러한 콘텐츠를 제공하려면 **개인화된 콘텐츠(AEM의 콘텐츠)**&#x200B;와 **지능형 방법(Target)**&#x200B;이 필요합니다.

AEM을 사용하면 개인화된 콘텐츠를 제작하여 모든 콘텐츠 및 자산을 중앙 위치에 가져와서 개인화 전략을 실행합니다. AEM을 사용하면 코드를 작성하지 않고도 한 위치에서 데스크톱, 태블릿 및 모바일 장치용 콘텐츠를 쉽게 만들 수 있습니다. 모든 장치를 위해 페이지를 만들 필요가 없이, AEM은 콘텐츠를 사용하여 각 경험을 자동으로 조정합니다. 단추를 누르면 AEM에서 Adobe Target으로 컨텐츠를 내보낼 수도 있습니다.

이제 Target에서 AEM의 오퍼 형태로 개인화된 컨텐츠가 있습니다. Target을 사용하면 행동, 컨텍스트 및 오프라인 변수를 통합하는 규칙 기반 기계 학습 접근 방식과 AI 기반 기계 학습 접근 방식을 조합하여 규모에 맞게 이러한 오퍼를 제공할 수 있습니다.  Target을 사용하면 A/B와 MVT(다변량) 활동을 쉽게 설정 및 실행하여 최상의 오퍼, 콘텐츠 및 경험을 결정할 수 있습니다.

**경험** 조각은 콘텐츠/경험 작성자를 Target을 사용하여 비즈니스 결과를 이끄는 개인화 전문가에게 연결하기 위한 큰 단계를 나타냅니다.

* AEM 컨텐츠 편집기 는 개인화된 컨텐츠를 경험 조각 및 그 변형으로 작성합니다
* AEM은 경험 조각 HTML을 Target으로 내보냅니다&#x200B;.
* Target&#x200B;은 활동에서 AEM 경험 조각 마크업을 오퍼로 사용합니다
* Target은 경험 조각 HTML을 전달하고 AEM은 참조된 이미지를 제공합니다

   ![경험 조각 다이어그램을 사용한 개인화](assets/personalization-use-case-1/use-case-1-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)
* [기존 Cloud Services을 사용한 AEM 및 Adobe Target](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를  [자세히 살펴보십시오](./personalization-use-case-1.md).***

## Visual Experience Composer를 사용한 개인화

마케터는 Adobe Target VEC(시각적 경험 작성기)를 사용하여 테스트를 실행하도록 코드를 변경하지 않고 웹 사이트에서 빠른 변경을 수행할 수 있습니다. VEC는 사이트 컨텍스트에서 개인화된 경험과 오퍼를 쉽게 만들고 테스트할 수 있는 WYSIWYG(표시되는 항목) 사용자 인터페이스입니다. 웹 페이지(또는 오퍼) 또는 모바일 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 Target 활동에 대한 경험과 오퍼를 만들 수 있습니다.

VEC는 Adobe Target의 주요 기능 중 하나입니다. VEC를 통해 마케터와 디자이너가 시각적 인터페이스를 사용하여 콘텐츠를 만들고 변경할 수 있습니다. 코드를 직접 편집하지 않고도 많은 디자인 옵션을 선택할 수 있습니다. 작성기에서 사용할 수 있는 편집 옵션을 사용하여 HTML과 JavaScript를 편집할 수도 있습니다.

* 컨텐츠는 AEM에 있으며 컨텐츠 편집자는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다
* Target은 개인화된 컨텐츠를 제공합니다
* Adobe Target VEC를 사용하여 새로운 콘텐츠가 만들어집니다
* AEM 호스팅 사이트 및 AEM이 아닌 호스팅 사이트에 모두 적용됩니다

   ![시각적 경험 작성기 다이어그램을 사용한 개인화](assets/personalization-use-case-3/use-case-diagram-3.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를  [자세히 살펴보십시오.](./personalization-use-case-3.md)***

## 전체 웹 페이지 경험의 개인화

Adobe Experience Manager과 Adobe Target을 통합하면 사이트 사용자에게 개인화된 경험을 제공할 수 있습니다. 또한 지정된 테스트 기간 동안 전환을 최고로 향상시킨 웹 사이트 컨텐츠 버전을 더 잘 이해할 수 있도록 도와줍니다. 예를 들어, A/B 테스트는 두 개 이상의 웹 사이트 컨텐츠 버전을 비교하여 전환, 판매 또는 식별되는 기타 지표를 가장 잘 높이는 방법을 확인합니다. 마케터는 Adobe Target 내에서 활동을 만들어 사용자가 사이트 컨텐츠와 상호 작용하는 방법과 사이트 지표에 미치는 영향을 이해할 수 있습니다.

* 컨텐츠는 AEM에 있으며 컨텐츠 편집자는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다
* Target은 개인화된 컨텐츠를 제공합니다
* 여기에 새로 만들어지는 순 컨텐츠가 없습니다
* AEM 사이트와 비 AEM 사이트 모두에 적용됩니다

   ![다이어그램](assets/personalization-use-case-2/use-case-2-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 시나리오를  [자세히 살펴보십시오.](./personalization-use-case-2.md)***
