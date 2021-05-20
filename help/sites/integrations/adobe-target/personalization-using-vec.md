---
title: Visual Experience Composer를 사용한 개인화
description: 시각적 경험 작성기를 사용하여 Adobe Target 활동을 만드는 방법을 알아봅니다.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: 통합
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 0%

---


# 시각적 경험 작성기를 사용한 개인화 {#personalization-vec}

VEC(시각적 경험 작성기)를 사용하여 A/B 테스트 Target 활동을 만드는 방법을 알아봅니다.

## 전제 조건

AEM 웹 사이트에서 VEC를 사용하려면 다음 설정을 완료해야 합니다.

1. [AEM 웹 사이트에 Adobe Target 추가](./add-target-launch-extension.md)
1. [Launch에서 Adobe Target 호출 트리거](./load-and-fire-target.md)

## 시나리오 개요

WKND 사이트 홈 페이지에는 로컬 활동이나 도시 주변에서 정보 카드 형태로 수행할 수 있는 가장 좋은 작업이 표시됩니다. 마케팅 담당자는 모험 섹션 Teaser에 텍스트를 변경하고 전환을 향상시키는 방법을 이해하여 홈 페이지를 수정하는 작업을 지정받았습니다.

## VEC(시각적 경험 작성기)를 사용하여 A/B 테스트를 만드는 절차

1. [Adobe Experience Cloud](https://experience.adobe.com/)에 로그인하고 __Target__&#x200B;을 탭하고 __활동__ 탭으로 이동합니다

   + Experience Cloud 대시보드에 __Target__&#x200B;이 표시되지 않으면 오른쪽 상단의 조직 전환기에서 올바른 Adobe 조직이 선택되고 사용자에게 [Adobe Admin Console](https://adminconsole.adobe.com/)에서 Target에 대한 액세스 권한이 부여되었는지 확인하십시오.

1. **활동 만들기** 단추를 클릭한 다음 **A/B 테스트** 활동을 선택합니다

   ![A/B 활동](assets/ab-target-activity.png)

1. **시각적 경험 작성기** 옵션을 선택하고 활동 URL을 제공한 다음 **다음**&#x200B;을 클릭합니다

   ![활동 URL](assets/ab-test-url.png)

1. 새 활동을 만든 후 시각적 경험 작성기에 왼쪽에 두 개의 탭이 표시됩니다.*경험 A* 및 *경험 B*. 목록에서 경험을 선택합니다. **경험 추가** 단추를 사용하여 목록에 새 경험을 추가할 수 있습니다.

   ![경험 A](assets/experience.png)

1. 페이지에서 이미지나 텍스트를 선택하여 수정 작업을 시작하거나 코드 편집기를 사용하여 HTML 요소를 선택하고 사용할 수 있습니다.

   ![요소](assets/select-element.png)

1. 텍스트를 *호주 서부에서 캠핑*&#x200B;에서 *오스트레일리아 모험*&#x200B;으로 변경합니다. 경험에 추가된 변경 사항 목록이 수정 사항 아래에 표시됩니다. 를 클릭하고 수정된 항목을 편집하여 해당 CSS 선택기와 추가된 새 컨텐츠를 볼 수 있습니다.

   ![모험](assets/adventures.png)

1. *경험 A*&#x200B;의 이름을 *Adventure*&#x200B;로 변경합니다.
1. 마찬가지로, *경험 B*&#x200B;의 텍스트를 *서부 오스트레일리아에 있는 캠핑*&#x200B;에서 *호주 야생화 탐색*&#x200B;으로 업데이트하십시오.

   ![탐색](assets/explore.png)

1. **다음**&#x200B;을 클릭하여 타깃팅으로 이동하고 두 경험 사이에 50-50의 수동 트래픽 할당을 유지하겠습니다.

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
