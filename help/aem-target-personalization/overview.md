---
title: AEM 및 Adobe Target 시작하기
description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여 주는 전체 튜토리얼입니다. 이 튜토리얼에서는 전체 프로세스에 참여하는 다양한 페르소나와 그들이 서로 협업하는 방법에 대해서도 배울 수 있습니다.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# AEM Sites와 Adobe Target 통합 {#getting-started-with-aem-target}

AEM과 Target은 모두 기능이 중복되는 강력한 솔루션입니다. 고객은 이러한 제품을 어떻게 언제 사용하는지 파악하여 개인화된 경험을 전달할 수 있습니다. 모든 최종 사용자에게 최적화된 경험을 전달하려면 조직 내의 다양한 팀이 긴밀하게 작업하고 누가 어떤 작업을 수행하는지 정의해야 합니다.

이 튜토리얼에서는 AEM과 Target에 대한 세 가지 시나리오를 다루며, 이를 통해 조직에 가장 적합한 접근 방식을 이해하고 다양한 팀 간의 협업 방식을 파악할 수 있습니다.

* 시나리오 1: AEM 경험 조각을 사용한 개인화
* 시나리오 2: 시각적 경험 작성기를 사용한 개인화
* 시나리오 3: 전체 웹 페이지 경험의 개인화

## AEM 경험 조각을 사용한 개인화 {#personalization-using-aem-experience-fragment}

이 시나리오에서는 AEM과 Target을 사용합니다. 분명히 두 제품 모두 각자의 장점이 있으며, 사이트 사용자에게 개인화된 경험을 제공하려면 **개인화된 콘텐츠(AEM의 콘텐츠)**&#x200B;와 특정 사용자에 따라 이러한 콘텐츠를 제공하는 **지능적인 방식(Target)**&#x200B;이 모두 필요합니다.

AEM은 개인화된 콘텐츠를 만들 수 있도록 도와주며, 모든 콘텐츠와 자산을 중앙 위치에서 통합 관리하여 개인화 전략을 추진할 수 있도록 지원합니다. AEM을 사용하면 코드를 작성하지 않고도 한 위치에서 데스크탑, 태블릿 및 모바일 디바이스의 콘텐츠를 쉽게 만들 수 있습니다. 모든 디바이스를 위해 페이지를 만들 필요가 없이, AEM에서는 콘텐츠를 사용하여 각 경험이 자동으로 조정됩니다. 버튼 하나만 누르면 AEM의 콘텐츠를 Adobe Target으로 오퍼로 내보낼 수도 있습니다.

이제 Target에서는 AEM에서 만든 오퍼 형태의 개인화된 콘텐츠를 이용할 수 있습니다. Target을 사용하면 행동 변수, 컨텍스트 변수 및 오프라인 변수를 통합하는 규칙 기반 및 AI 기반 머신 러닝 접근 방식의 결합을 기반으로 이들 오퍼를 다양한 규모로 제공할 수 있습니다.  Target을 사용하면 A/B 및 다변량(MVT) 활동을 쉽게 설정 및 실행하여 최상의 오퍼, 콘텐츠 및 경험을 결정할 수 있습니다.

**경험 조각**&#x200B;은 콘텐츠/경험 작성자와 Target을 사용하여 비즈니스 결과를 이끄는 개인화 전문가를 연결해 주는 중요한 단계입니다.

* AEM 콘텐츠 편집기는 개인화된 콘텐츠를 경험 조각 및 그 변형으로 작성합니다.
* AEM은 경험 조각의 HTML을 Target으로 내보냅니다.&#x200B;
* Target은 AEM 경험 조각의 마크업을 활동 내 오퍼로 사용합니다.
* Target은 경험 조각 HTML을 전달하고, AEM은 참조된 이미지를 제공합니다.

  ![경험 조각을 사용한 개인화 다이어그램](assets/personalization-use-case-1/use-case-1-diagram.png)

**이 시나리오를 구현하려면 다음 작업을 수행해야 합니다.**

* [태그와 Adobe I/O를 사용하여 AEM과 Adobe Target 통합](./implementation.md#integrating-aem-target-options)
* [레거시 클라우드 서비스를 사용하여 AEM과 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 [시나리오를 자세히 살펴보겠습니다](./personalization-use-case-1.md).***

## 시각적 경험 작성기를 사용한 개인화

마케터는 Adobe Target VEC(시각적 경험 작성기)를 사용하여 코드 변경 없이 웹 사이트에서 빠르게 테스트를 실행할 수 있습니다. VEC는 사이트 컨텍스트에서 개인화된 경험과 오퍼를 쉽게 만들고 테스트할 수 있는 WYSIWYG(What You See Is What You Get) 사용자 인터페이스입니다. 웹 페이지(또는 오퍼) 또는 모바일 웹 페이지의 레이아웃 및 콘텐츠를 끌어다 놓고, 교체하고, 수정하여 타깃 활동에 대한 경험과 오퍼를 만들 수 있습니다.

VEC는 Adobe Target의 주요 기능 중 하나입니다. VEC를 통해 마케터와 디자이너가 시각적 인터페이스를 사용하여 콘텐츠를 만들고 변경할 수 있습니다. 코드를 직접 편집하지 않고도 많은 디자인을 작성할 수 있습니다. 또한 작성기에서 사용 가능한 편집 선택 사항을 사용하여 HTML 및 JavaScript를 편집할 수도 있습니다.

* 콘텐츠는 AEM에 있으며, 콘텐츠 편집기가 사이트 페이지를 만들고 관리합니다.
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트와 개인화를 실행합니다.
* Target은 개인화된 콘텐츠를 제공합니다.
* Adobe Target VEC를 사용하여 새로운 콘텐츠가 생성됩니다.
* AEM 호스팅 사이트와 AEM에서 호스팅하지 않는 사이트 모두에 적용됩니다.

  ![시각적 규칙 편집기를 사용한 개인화 다이어그램](assets/personalization-use-case-3/use-case-diagram-3.png)

**이 시나리오를 구현하려면 다음 작업을 수행해야 합니다.**

* [태그와 Adobe I/O를 사용하여 AEM과 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 [시나리오를 자세히 살펴보겠습니다.](./personalization-use-case-3.md)***

## 전체 웹 페이지 경험의 개인화

Adobe Experience Manager를 Adobe Target과 통합하면 사이트 사용자에게 개인화된 경험을 제공하는 데 도움이 됩니다. 또한 이를 통해 특정 테스트 기간 동안 어떤 버전의 웹 사이트 콘텐츠가 전환율을 가장 잘 개선하는지 더 잘 이해할 수 있습니다. 예를 들어 A/B 테스트를 통해 두 개 이상의 웹 사이트 콘텐츠 버전을 비교하여 전환, 판매 또는 식별한 기타 지표를 가장 크게 향상시키는 콘텐츠를 확인할 수 있습니다. 마케터는 Adobe Target 내에서 활동을 만들어 사용자가 사이트 콘텐츠와 상호 작용하는 방식과 이것이 사이트 지표에 미치는 영향을 파악할 수 있습니다.

* 콘텐츠는 AEM에 있으며, 콘텐츠 편집기가 사이트 페이지를 만들고 관리합니다.
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트와 개인화를 실행합니다.
* Target은 개인화된 콘텐츠를 제공합니다.
* 여기에서는 새로운 콘텐츠가 생성되지 않습니다.
* AEM 및 AEM이 아닌 사이트 모두에 적용됩니다.

  ![다이어그램](assets/personalization-use-case-2/use-case-2-diagram.png)

**이 시나리오를 구현하려면 다음 작업을 수행해야 합니다.**

* [태그와 Adobe I/O를 사용하여 AEM과 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 [시나리오를 자세히 살펴보겠습니다.](./personalization-use-case-2.md)***
