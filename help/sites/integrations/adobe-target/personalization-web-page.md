---
title: 전체 웹 페이지 경험의 개인화
description: Adobe Target을 사용하여 AEM 웹 사이트 페이지를 새 페이지로 리디렉션하는 Target 활동을 만드는 방법을 알아봅니다.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: 통합
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---


# 전체 웹 페이지 경험의 개인화 {#personalization-fpe}

AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션하는 활동을 만드는 방법을 알아봅니다.

## 전제 조건

AEM 웹 사이트의 전체 페이지를 개인화하려면 다음 설정을 완료해야 합니다.

1. [AEM 웹 사이트에 Adobe Target 추가](./add-target-launch-extension.md)
1. [Launch에서 Adobe Target 호출 트리거](./load-and-fire-target.md)

## 시나리오 개요

WKND 사이트는 홈 페이지를 다시 디자인했으며 현재 홈 페이지 방문자를 새 홈 페이지로 리디렉션하려고 합니다. 동시에, 재설계된 홈 페이지가 사용자 참여도와 매출을 개선하는 방법을 이해합니다. 마케터는 방문자를 새 홈 페이지로 리디렉션하는 활동을 만드는 작업을 지정받았습니다. WKND 사이트 홈 페이지를 살펴보고 Adobe Target을 사용하여 활동을 만드는 방법을 알아봅니다.

## VEC(시각적 경험 작성기)를 사용하여 A/B 테스트를 만드는 절차

1. Adobe Target에 로그인하고 활동 탭으로 이동합니다
1. **활동 만들기** 단추를 클릭한 다음 **A/B 테스트** 활동을 선택합니다

   ![A/B 활동](assets/ab-target-activity.png)

1. **시각적 경험 작성기** 옵션을 선택하고 활동 URL을 제공한 다음 **다음**&#x200B;을 클릭합니다

   ![활동 URL](assets/ab-test-url.png)

1. 새 활동을 만든 후 시각적 경험 작성기에 왼쪽에 두 개의 탭이 표시됩니다.*경험 A* 및 *경험 B*. 목록에서 경험을 선택합니다. **경험 추가** 단추를 사용하여 목록에 새 경험을 추가할 수 있습니다.

   ![경험 선택 사항](assets/experience-options.png)

1. 경험 A에 사용할 수 있는 옵션을 보고 **URL로 리디렉션** 옵션을 선택하고 새 WKND 사이트 홈 페이지에 대한 URL을 제공합니다.

   ![리디렉션 URL](assets/redirect-url.png)

1. *경험 A*&#x200B;의 이름을 *새 WKND 홈 페이지* 및 *경험 B*&#x200B;을 *WKND 홈 페이지*&#x200B;로 바꾸십시오

   ![모험](assets/new-experiences.png)

1. **다음**&#x200B;을 클릭하여 타깃팅으로 이동하고 두 경험 사이에 50-50의 수동 트래픽 할당을 유지합니다.

   ![타깃팅](assets/targeting.png)

1. 목표 및 설정에 대해 보고 소스를 Adobe Target으로 선택하고 페이지 보기 작업을 사용하여 목표 지표를 전환으로 선택합니다.

   ![목표](assets/goals.png)

1. 활동의 이름을 입력하고 저장합니다.
1. 저장된 활동을 활성화하여 변경 사항을 실시간으로 푸시합니다.

   ![목표](assets/activate.png)

1. 새 탭에서 사이트 페이지(3단계의 활동 URL)를 열고 A/B 테스트 활동에서 경험(WKND 홈 페이지 또는 새 WKND 홈 페이지) 중 하나를 볼 수 있습니다. `us/en.html` 로 리디렉션합니다  `us/home.html`.

   ![목표](assets/redirect-test.png)

## 요약

마케터는 AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션하는 활동을 만들 수 있었습니다.

## 지원 링크

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

