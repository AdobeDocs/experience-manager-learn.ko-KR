---
title: AEM 및 Adobe Target 시작하기
description: Adobe Experience Manager 및 Adobe Target을 사용하여 개인화된 경험을 만들고 제공하는 방법을 보여 주는 종단간 튜토리얼입니다. 또한 이 튜토리얼에서는 종단 간 프로세스에 관련된 다양한 가상 사용자와 이러한 가상 사용자가 서로 공동 작업하는 방법에 대해 알아봅니다
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 193
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%

---

# AEM Sites 및 Adobe Target 통합 {#getting-started-with-aem-target}

AEM과 Target은 모두 기능이 중복되는 강력한 솔루션입니다. 고객은 이러한 제품을 어떻게 언제 사용하는지 파악하여 개인화된 경험을 전달할 수 있습니다. 모든 최종 사용자에게 최적화된 경험을 전달하려면 조직 내의 다양한 팀이 긴밀하게 작업하고 누가 어떤 작업을 수행하는지 정의해야 합니다.

이 튜토리얼에서는 조직에 가장 적합한 기능과 서로 다른 팀이 공동 작업하는 방법을 이해하는 데 도움이 되는 AEM 및 Target에 대한 세 가지 시나리오를 다룹니다.

* 시나리오 1 : AEM 경험 조각을 사용한 개인화
* 시나리오 2: 시각적 경험 작성기를 사용한 개인화
* 시나리오 3 : 전체 웹 페이지 경험의 개인화

## AEM Experience Fragments를 사용한 개인화 {#personalization-using-aem-experience-fragment}

이 시나리오에서는 AEM 및 Target을 사용합니다. 두 제품 모두 각각의 장점이 있으므로, 사이트 사용자에게 개인화된 경험을 제공할 때 필요한 사항이 있습니다 **개인화된 콘텐츠(AEM의 콘텐츠)** 및 **지능형 방식(Target)** 특정 사용자를 기준으로 이러한 컨텐츠를 제공합니다.

AEM은 모든 콘텐츠와 에셋을 중앙 위치에 모아 개인화 전략을 가속화하는 개인화된 콘텐츠를 만들 수 있도록 지원합니다. AEM을 사용하면 코드를 작성하지 않고도 한 곳에서 데스크탑, 태블릿 및 모바일 장치의 콘텐츠를 쉽게 만들 수 있습니다. 모든 디바이스를 위해 페이지를 만들 필요가 없이, 컨텐츠를 사용하여 각 경험이 자동으로 조정됩니다. AEM 버튼을 눌러 컨텐츠를 AEM에서 Adobe Target으로 오퍼로 내보낼 수도 있습니다.

이제 Target에 AEM의 오퍼 형식으로 개인화된 콘텐츠가 있습니다. Target을 사용하면 행동 변수, 컨텍스트 변수 및 오프라인 변수를 통합하는 규칙 기반 및 AI 기반 머신 러닝 접근 방식의 조합을 기반으로 이러한 오퍼를 규모에 맞게 제공할 수 있습니다.  Target을 사용하면 A/B와 MVT(다변량) 활동을 쉽게 설정 및 실행하여 최상의 오퍼, 콘텐츠 및 경험을 결정할 수 있습니다.

**경험 조각** 는 콘텐츠/경험 작성자를 Target을 사용하여 비즈니스 결과를 이끄는 개인화 전문가와 연결하기 위한 매우 큰 단계를 나타냅니다.

* AEM 콘텐츠 편집기는 개인화된 콘텐츠를 경험 조각 및 그 변형으로 작성합니다
* AEM이 경험 조각 HTML을 Target으로 내보냅니다&#x200B;.
* Target&#x200B;은 AEM 경험 조각 마크업을 활동의 오퍼로 사용합니다
* Target은 경험 조각 HTML을 제공하고 AEM은 참조된 이미지를 제공합니다.

  ![경험 조각 다이어그램을 사용한 개인화](assets/personalization-use-case-1/use-case-1-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)
* [이전 Cloud Service을 사용하는 AEM 및 Adobe Target](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 다음을 살펴보도록 하겠습니다. [시나리오 세부 정보](./personalization-use-case-1.md).***

## Visual Experience Composer를 사용한 개인화

마케터는 Adobe Target VEC(시각적 경험 작성기)를 사용하여 테스트를 실행하기 위한 코드를 변경하지 않고 웹 사이트에서 빠르게 변경할 수 있습니다. VEC는 사이트 컨텍스트에서 개인화된 경험과 오퍼를 쉽게 만들고 테스트할 수 있는 WYSIWYG(표시되는 것은 얻는 것입니다) 사용자 인터페이스입니다. 웹 페이지(또는 오퍼) 또는 모바일 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 타겟 활동에 대한 경험과 오퍼를 만들 수 있습니다.

VEC는 Adobe Target의 주요 기능 중 하나입니다. VEC를 통해 마케터와 디자이너가 시각적 인터페이스를 사용하여 콘텐츠를 만들고 변경할 수 있습니다. 코드를 직접 편집하지 않고도 많은 디자인 선택을 할 수 있습니다. 작성기에서 사용할 수 있는 편집 옵션을 사용하여 HTML 및 JavaScript를 편집할 수도 있습니다.

* 콘텐츠는 AEM에 있으며 콘텐츠 편집기는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다
* Target은 개인화된 콘텐츠를 제공합니다.
* Adobe Target VEC를 사용하여 새로운 순 콘텐츠 만들기
* AEM 호스팅 사이트와 AEM이 아닌 호스팅 사이트에 모두 적용됩니다.

  ![시각적 경험 작성기 다이어그램을 사용한 개인화](assets/personalization-use-case-3/use-case-diagram-3.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 다음을 살펴보도록 하겠습니다. [시나리오 세부 정보.](./personalization-use-case-3.md)***

## 전체 웹 페이지 경험의 개인화

Adobe Experience Manager과 Adobe Target을 통합하면 사이트 사용자에게 개인화된 경험을 제공할 수 있습니다. 또한 지정된 테스트 기간 동안 전환을 가장 잘 개선하는 웹 사이트 콘텐츠의 버전을 더 잘 이해할 수 있도록 해줍니다. 예를 들어 A/B 테스트는 웹 사이트 콘텐츠의 버전을 두 개 이상 비교하여 사용자가 식별하는 전환, 판매 또는 기타 지표를 가장 잘 상승시키는지 확인합니다. 마케터는 Adobe Target 내에서 활동을 만들어 사용자가 사이트 콘텐츠와 상호 작용하는 방법 및 사이트 지표에 영향을 주는 방법을 이해할 수 있습니다.

* 콘텐츠는 AEM에 있으며 콘텐츠 편집기는 사이트 페이지를 만들고 관리합니다
* Target은 AEM 호스팅 사이트 페이지를 사용하여 테스트 및 개인화를 실행합니다
* Target은 개인화된 콘텐츠를 제공합니다.
* 여기에 새로운 순 콘텐츠가 만들어지지 않음
* AEM 및 비 AEM 사이트 모두에 적용됩니다.

  ![다이어그램](assets/personalization-use-case-2/use-case-2-diagram.png)

**이 시나리오를 구현하려면 다음을 수행해야 합니다.**

* [Launch 및 Adobe I/O을 사용하여 AEM 및 Adobe Target 통합](./implementation.md#integrating-aem-target-options)

***위의 통합을 구현한 후 다음을 살펴보도록 하겠습니다. [시나리오 세부 정보.](./personalization-use-case-2.md)***
