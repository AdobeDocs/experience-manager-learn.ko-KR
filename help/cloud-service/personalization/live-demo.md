---
title: Personalization 사용 사례의 라이브 데모
description: WKND 지원 웹 사이트에서 A/B 테스트, 행동 타기팅 및 알려진 사용자 개인화 예를 통해 작동 중인 경험 개인화를 경험하십시오.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: 9e99936fb03e085f6bc276c7d6ef5cc08e34d1e5
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 1%

---

# Personalization 사용 사례의 라이브 데모

A/B 테스트, 행동 타기팅 및 알려진 사용자 개인화에 대한 실제 사례를 보려면 [WKND 지원 웹 사이트](https://wknd.enablementadobe.com/us/en.html){target="wknd"}를 방문하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3476468/?captions=kor&learn=on&enablevpops)

이 페이지에서는 각 개인화 시나리오의 실습 데모를 안내합니다. 이를 사용하여 AEM 사이트에서 이러한 기능을 빌드하기 전에 가능한 기능을 살펴보십시오.

>[!IMPORTANT]
>
> 여러 브라우저 창 또는 시크릿/개인 탐색 모드에서 데모 사이트를 열어 다양한 개인화된 변형을 동시에 경험하십시오.
> 비공개 탐색 모드를 사용할 때 Firefox와 Safari는 ECID 쿠키를 차단하거나, 새로운 개인화 시나리오를 시도하기 전에 일반 탐색 모드를 사용하거나 쿠키를 지울 수 있습니다.

## 데모 사용 사례

[WKND 지원 웹 사이트](https://wknd.enablementadobe.com/us/en.html){target="wknd"}에서는 세 가지 유형의 개인 맞춤화를 보여 줍니다.

| Personalization 유형 | 표시되는 항목 | 시간 |
|---------------------|-----------------|---------|
| **동작 타깃팅** | 콘텐츠는 브라우징 비헤이비어와 관심사에 따라 조정됩니다. 일반적으로 _다음 페이지 또는 동일한 페이지 개인화_&#x200B;라고 합니다. | 실시간 및 일괄 처리 |
| **알려진 사용자 Personalization** | 여러 시스템의 데이터로 구축된 전체 고객 프로필을 기반으로 맞춤화된 경험. 일반적으로 _규모에 대한 개인화_&#x200B;라고 합니다. | 실시간 |
| **A/B 테스트** | 다양한 콘텐츠 변형을 테스트하여 최고의 성능을 찾았습니다. 일반적으로 _실험_&#x200B;이라고 합니다. | 실시간 |

## 행동 타기팅

콘텐츠는 브라우징 세션 중에 방문자의 작업 및 관심사에 따라 자동으로 조정됩니다. 일반적으로 _다음 페이지 또는 같은 페이지 개인화_&#x200B;라고 합니다.

### 홈, 모험 및 잡지 페이지

이러한 경험은 현재 탐색 동작(실시간 개인화)을 기반으로 즉시 표시됩니다. Adobe Experience Platform Edge Network을 사용하여 실시간 개인화 결정을 내릴 수 있습니다.

| 페이지 | 표시되는 항목 | 테스트 방법 | 경험 |
|------|-----------------|-------------|------------|
| [홈](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 호숫가에서 함께 자전거를 타며 공유 추억을 만드는 안내 경험을 홍보하는 개인화된 **가족 친화적인 모험 영웅 배너** | [발리 서프 캠프](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} 또는 [미식 마레 투어](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"}를 방문한 다음 홈페이지로 돌아가기 | ![홈 - 가족 모험 영웅](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [모험](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | &quot;We&#39;ve Got You Covered&quot; 메시지와 WKND의 전문 파트너가 제공하는 무료 자전거 유지 보수 서비스를 통해 사이클링에 중점을 둔 **&quot;무료 자전거 튠업&quot; 홍보 영웅** | 사이클링과 관련된 모험(예: [Cycling Tuscany](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"})을 방문한 다음 모험 페이지로 이동합니다 | ![모험 - 무료 자전거 튠업 영웅](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [모험](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | 캠핑을 테마로 한 **기어 컬렉션 영웅**&#x200B;은(는) &quot;올바른 기어로 시작하는 다음 모험&quot; 메시지와 함께 필수 캠핑 장비(침낭, 재킷, 부츠)를 선보이고 있습니다. | 캠핑 관련 모험(예: [요세미티 배낭](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"})을 방문한 다음 모험 페이지로 이동합니다 | ![모험 - 캠프 기어 컬렉션 영웅](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [잡지](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | 눈에 띄는 &quot;SALE!&quot;을 사용하여 롤링된 WKND 매거진을 선보이는 시간에 민감한 **매거진 판매 프로모션** 문제 및 야외 컬렉션에 대한 배지 및 특별 광고주 가격 책정 | 하나 이상의 매거진 문서(예: [스키 투어](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"})를 읽은 다음 매거진 랜딩 페이지로 이동 | ![매거진 - 판매 영웅](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### 모험 및 잡지 페이지(일괄 처리)

이러한 경험은 이전 동작을 기반으로 하며 다음 방문 또는 나중에 같은 날(일괄 개인화)에 표시됩니다. 데이터는 집계되어 프로필 속성으로 처리되고 Adobe Experience Platform Edge Network으로 활성화됩니다.

| 페이지 | 표시되는 항목 | 테스트 방법 | 경험 |
|------|-----------------|-------------|------------|
| [모험](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | &quot;Your Surf 여정 Starts Here&quot; 메시지와 관심 사항을 기반으로 선별된 서핑 대상 콘텐츠가 포함된 **야자수 아래의 화려한 서핑보드**&#x200B;를 제공하는 서핑 테마 영웅 | 여러 [서핑 관련 모험](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"}을(를) 방문한 다음 다음날 모험 페이지로 돌아가기 | ![모험 - 서핑 영웅](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [잡지](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | 클래식 VW 밴으로 세계 여행지를 소개하는 **개인 맞춤화된 잡지 구독 오퍼**&#x200B;로, 독점적인 구독자 혜택과 함께 &quot;개인 맞춤화된 잡지 경험&quot;을 강조합니다. | 3개 이상의 [매거진 기사](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"}를 읽은 후 다음 날 매거진 랜딩 페이지로 돌아가기 | ![매거진 - 영웅 구독](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**자세히 알아보기:** 자신의 AEM 사이트에서 동작 타깃팅을 구현할 준비가 되셨습니까? 전체 설정 프로세스를 알아보려면 [동작 타기팅 자습서](./use-cases/behavioral-targeting.md)로 시작하십시오.

## 알려진 사용자 Personalization

구매 내역 및 고객 라이프사이클 단계를 포함하여 여러 시스템의 데이터로 구축된 전체 고객 프로필을 기반으로 개인화된 경험. Adobe Experience Platform Edge Network을 사용하여 실시간 개인화 결정을 내릴 수 있습니다.

### 홈 페이지 영웅

WKND 홈 페이지 영웅 배너는 인증된 사용자 프로필을 기반으로 개인화됩니다. 다음 데모 계정으로 테스트하여 개인화된 경험을 확인하십시오.

| 페이지 | 표시되는 항목 | 테스트 방법 | 프로필 컨텍스트 | 경험 |
|------|-----------------|-------------|-----------------|------------|
| [홈](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 스키숍 인테리어는 다가오는 스키 모험을 준비하기 위한 전문가 포장 팁을 제공하는 &quot;추가 25% 할인&quot;**프로모션과 함께**&#x200B;프리미엄 스키 장비를 선보이고 있습니다 | `teddy/teddy` 또는 `asmith/asmith`(으)로 로그인하고 페이지를 새로 고칩니다. | 최근에 구입한 스키 모험, 따라서 업셀링 스키 장비 | ![홈 - 스키 장비 업셀](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**자세히 알아보기:** AEM 사이트에서 알려진 사용자 개인화를 구현할 준비가 되셨습니까? 전체 설정 프로세스를 알아보려면 [알려진 사용자 Personalization 자습서](./use-cases/known-user-personalization.md)로 시작하십시오.

## A/B 테스트(실험)

다양한 콘텐츠 변형을 테스트하여 비즈니스 목표에 가장 적합한 성과를 결정합니다. Adobe Target은 성과가 더 좋은 방문자 및 트랙에 무작위로 다양한 변형을 제공합니다. 이를 일반적으로 _실험_&#x200B;이라고 합니다.

### 홈 페이지 추천 문서

WKND 홈페이지는 _서부 호주에서 캠핑_ 추천 기사의 세 가지 변형으로 활성 A/B 테스트를 실행합니다. 각 방문자는 다음 변형 중 하나를 보기 위해 임의로 할당됩니다.

| 페이지 | 표시되는 항목 | 테스트 방법 | 경험 |
|------|-----------------|-------------|------------|
| [홈](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | &quot;우리의 특집&quot; 섹션에서 무작위로 할당된 세 가지 특집 기사 변형 중 하나: **&quot;Off the Grid: Epic Camping Routes Across Western Australia&quot;** 또는 **&quot;Wanding the Wild: Camping Adventures in Western Australia&quot;**(또는 세 번째 변형)이며, 각각 고유한 이미지와 메시지를 사용하여 가장 좋은 반향을 테스트합니다 | 다른 브라우저의 홈 페이지를 방문하거나, 시크릿/비공개 모드를 사용하거나, 쿠키를 지워 다른 변형을 볼 수 있습니다 | ![A/B 테스트 변형](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**자세히 알아보기:** 자신의 AEM 사이트에서 A/B 테스트를 구현할 준비가 되셨습니까? 전체 설정 프로세스에 대해 알아보려면 [실험(A/B 테스트) 튜토리얼](./use-cases/experimentation.md)(으)로 시작하십시오.


## 다음 단계

자체 AEM 사이트에서 개인화를 구현할 준비가 되셨습니까? 전체 설정 프로세스를 알아보려면 [Personalization 개요](./overview.md)로 시작하십시오.


