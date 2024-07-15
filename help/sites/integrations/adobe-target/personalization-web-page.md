---
title: 전체 웹 페이지 경험의 Personalization
description: Adobe Target을 사용하여 AEM 웹 사이트 페이지를 새 페이지로 리디렉션하기 위한 Target 활동을 만드는 방법을 알아봅니다.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 전체 웹 페이지 경험의 Personalization {#personalization-fpe}

AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션하는 활동을 만드는 방법을 알아봅니다.

## 사전 요구 사항

AEM 웹 사이트의 전체 페이지를 개인화하려면 다음 설정을 완료해야 합니다.

1. [AEM 웹 사이트에 Adobe Target 추가](./add-target-launch-extension.md)
1. [태그에서 Adobe Target 호출 트리거](./load-and-fire-target.md)

## 시나리오 개요

WKND 사이트는 홈 페이지를 다시 디자인하고 현재 홈 페이지 방문자를 새 홈 페이지로 리디렉션하려고 합니다. 또한 재설계된 홈 페이지가 사용자 참여 및 매출을 개선하는 데 어떻게 도움이 되는지도 이해합니다. 마케터로서 방문자를 새 홈 페이지로 리디렉션하는 활동을 만드는 작업이 할당되었습니다. WKND 사이트 홈 페이지를 살펴보고 Adobe Target을 사용하여 활동을 만드는 방법을 알아봅니다.

## VEC(시각적 경험 작성기)를 사용하여 A/B 테스트를 만드는 절차

1. Adobe Target에 로그인하여 활동 탭으로 이동합니다
1. **활동 만들기** 단추를 클릭한 다음 **A/B 테스트** 활동을 선택하십시오.

   ![A/B 활동](assets/ab-target-activity.png)

1. **시각적 경험 작성기** 옵션을 선택하고 활동 URL을 입력한 다음 **다음**&#x200B;을 클릭합니다

   ![활동 URL](assets/ab-test-url.png)

1. 활동을 만든 후 시각적 경험 작성기 왼쪽에 *경험 A* 및 *경험 B* 두 개의 탭이 표시됩니다. 목록에서 경험을 선택합니다. **경험 추가** 단추를 사용하여 새 경험을 목록에 추가할 수 있습니다.

   ![경험 옵션](assets/experience-options.png)

1. 경험 A에 사용할 수 있는 옵션을 확인한 다음 **URL로 리디렉션** 옵션을 선택하고 새 WKND 사이트 홈 페이지의 URL을 제공하십시오.

   ![리디렉션 URL](assets/redirect-url.png)

1. *경험 A*&#x200B;에서 *새 WKND 홈 페이지* 및 *경험 B*&#x200B;에서 *WKND 홈 페이지*(으)로 이름 바꾸기

   ![모험](assets/new-experiences.png)

1. **다음**&#x200B;을(를) 클릭하여 타깃팅으로 이동하고 두 경험 간에 50-50의 수동 트래픽 할당을 유지합니다.

   ![타깃팅](assets/targeting.png)

1. 목표 및 설정의 경우 보고 소스를 Adobe Target으로 선택하고 목표 지표를 페이지 보기 작업이 있는 전환으로 선택합니다.

   ![목표](assets/goals.png)

1. 활동의 이름을 입력하고 저장합니다.
1. 저장된 활동을 활성화하여 변경 사항을 실시간으로 푸시합니다.

   ![목표](assets/activate.png)

1. 새 탭에서 사이트 페이지(3단계의 활동 URL)를 열면 A/B 테스트 활동에서 경험(WKND 홈 페이지 또는 새 WKND 홈 페이지)을 볼 수 있습니다. `us/en.html`이(가) `us/home.html`(으)로 리디렉션합니다.

   ![목표](assets/redirect-test.png)

## 요약

마케터는 AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션하는 활동을 만들 수 있었습니다.

## 지원 링크

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
