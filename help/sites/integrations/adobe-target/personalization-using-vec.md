---
title: Visual Experience Composer를 사용한 개인화
description: 시각적 경험 작성기를 사용하여 Adobe Target 활동을 만드는 방법을 알아봅니다.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# Visual Experience Composer를 사용한 개인화 {#personalization-vec}

VEC(시각적 경험 작성기)를 사용하여 A/B 테스트 Target 활동을 만드는 방법을 알아봅니다.

## 사전 요구 사항

AEM 웹 사이트에서 VEC를 사용하려면 다음 설정을 완료해야 합니다.

1. [AEM 웹 사이트에 Adobe Target 추가](./add-target-launch-extension.md)
1. [Launch에서 Adobe Target 호출 트리거](./load-and-fire-target.md)

## 시나리오 개요

WKND 사이트 홈 페이지에는 로컬 활동이나 도시 주변에서 정보 카드 형태로 수행할 수 있는 가장 좋은 작업이 표시됩니다. 마케팅 담당자는 모험 섹션 Teaser에 텍스트를 변경하고 전환을 향상시키는 방법을 이해하여 홈 페이지를 수정하는 작업을 지정받았습니다.

## VEC(시각적 경험 작성기)를 사용하여 A/B 테스트를 만드는 절차

1. 에 로그인합니다. [Adobe Experience Cloud](https://experience.adobe.com/), 탭하기 __Target__&#x200B;로 이동합니다. __활동__ 탭

   + 표시되지 않으면 __Target__ Experience Cloud 대시보드에서 오른쪽 상단의 조직 전환기에서 올바른 Adobe 조직이 선택되어 있고 사용자에게 의 Target 액세스 권한이 부여되었는지 확인합니다 [Adobe Admin Console](https://adminconsole.adobe.com/).

1. 클릭 **활동 만들기** 단추를 누른 다음 **A/B 테스트** 활동

   ![A/B 활동](assets/ab-target-activity.png)

1. 을(를) 선택합니다 **시각적 경험 작성기** 옵션을 선택하고 활동 URL을 제공한 다음 **다음**

   ![활동 URL](assets/ab-test-url.png)

1. 새 활동을 만든 후 시각적 경험 작성기에 왼쪽에 두 개의 탭이 표시됩니다. *경험 A* 및 *경험 B*. 목록에서 경험을 선택합니다. 를 사용하여 목록에 새 경험을 추가할 수 있습니다 **경험 추가 를 참조하십시오** 버튼을 클릭합니다.

   ![경험 A](assets/experience.png)

1. 페이지에서 이미지나 텍스트를 선택하여 수정 작업을 시작하거나 코드 편집기를 사용하여 요소를 선택하고 HTML 할 수 있습니다.

   ![요소](assets/select-element.png)

1. 텍스트 변경 *서부오스트레일리아의 캠핑* to *오스트레일리아의 모험*. 경험에 추가된 변경 사항 목록이 수정 사항 아래에 표시됩니다. 를 클릭하고 수정된 항목을 편집하여 해당 CSS 선택기와 추가된 새 컨텐츠를 볼 수 있습니다.

   ![모험](assets/adventures.png)

1. 이름 변경 *경험 A* to *모험*
1. 마찬가지로 텍스트를 *경험 B* 변환 전: *서부오스트레일리아의 캠핑* to *호주 황야 탐험*.

   ![탐색](assets/explore.png)

1. 클릭 **다음** 타깃팅으로 이동하고 두 경험 간에 50~50의 수동 트래픽 할당을 유지하겠습니다.

   ![타깃팅](assets/targeting.png)

1. 목표 및 설정에 대해 보고 소스를 Adobe Target으로 선택하고 페이지 보기 작업을 사용하여 목표 지표를 전환으로 선택합니다.

   ![목표](assets/goals.png)

1. 활동의 이름을 입력하고 저장합니다.
1. 저장된 활동을 활성화하여 변경 사항을 실시간으로 푸시합니다.

   ![목표](assets/activate.png)

1. 새 탭에서 사이트 페이지(3단계의 활동 URL)를 열고 A/B 테스트 활동에서 경험(모험 또는 탐색) 중 하나를 볼 수 있어야 합니다.

   ![목표](assets/publish.png)

## 요약

이 장에서 마케터는 테스트를 실행하기 위해 코드를 변경하지 않고 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 시각적 경험 작성기를 사용하여 경험을 만들 수 있었습니다.

## 지원 링크

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
