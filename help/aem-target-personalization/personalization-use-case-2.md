---
title: Adobe Target을 사용한 Personalization
description: Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여 주는 종단간 튜토리얼입니다.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---

# Adobe Target을 사용한 전체 웹 페이지 경험의 Personalization

이전 장에서는 경험 조각으로 만들고 AEM에서 HTML 오퍼로 내보낸 콘텐츠를 사용하여 Adobe Target 내에서 지리적 위치 기반 활동을 만드는 방법에 대해 알아보았습니다.

이 장에서는 Adobe Target을 사용하여 AEM에서 호스팅되는 사이트 페이지를 새 페이지로 리디렉션하기 위한 활동 만들기를 살펴봅니다.

## 시나리오 개요

WKND 사이트는 홈 페이지를 다시 디자인하고 현재 홈 페이지 방문자를 새 홈 페이지로 리디렉션하려고 합니다. 또한 재설계된 홈 페이지가 사용자 참여 및 매출을 개선하는 데 어떻게 도움이 되는지도 이해합니다. 마케터로서 방문자를 새 홈 페이지로 리디렉션하는 활동을 만드는 작업이 할당되었습니다. WKND 사이트 홈 페이지를 살펴보고 Adobe Target을 사용하여 활동을 만드는 방법을 알아봅니다.

### 관련 사용자

이 연습을 수행하려면 다음 사용자를 참여시키고 관리 액세스 권한이 필요할 수 있는 몇 가지 작업을 수행해야 합니다.

* **콘텐츠 제작자/콘텐츠 편집기**(Adobe Experience Manager)
* **마케터**(Adobe Target/최적화 팀)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 사전 요구 사항

* **AEM**
   * localhost 4502 및 4503에서 각각 실행 중인 [AEM 작성자 및 게시 인스턴스](./implementation.md#getting-aem).
   * [태그를 사용하여 Adobe Target과 통합된 AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 조직에 대한 액세스 권한 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 프로비저닝된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## 콘텐츠 편집기 활동

1. 마케터는 AEM 콘텐츠 편집기와 함께 WKND 홈 페이지 재디자인 논의를 시작하고 요구 사항에 대해 자세히 설명합니다.
   * ***요구 사항*** : 카드 기반 디자인으로 WKND 사이트 홈 페이지를 다시 디자인합니다.
2. 그런 다음 AEM 콘텐츠 편집기는 요구 사항을 기반으로 카드 기반 디자인으로 새 WKND 사이트 홈 페이지를 만들고 새 홈 페이지를 게시합니다.

## 마케터 활동

1. 마케터는 리디렉션 오퍼를 경험으로 사용하고, 성공 목표와 지표가 추가된 새 홈 페이지에 할당된 100% 웹 사이트 트래픽을 사용하여 A/B 타겟 활동을 만듭니다.
   1. Adobe Target 창에서 **활동** 탭으로 이동합니다.
   2. **활동 만들기** 단추를 클릭하고 **A/B 테스트**(으)로 활동 유형을 선택합니다.

      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-ab-activity.png)
   3. **웹** 채널을 선택하고 **시각적 경험 작성기**&#x200B;를 선택합니다.
   4. **활동 URL**&#x200B;을 입력하고 **다음**&#x200B;을 클릭하여 시각적 경험 작성기를 엽니다.

      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **시각적 경험 작성기**&#x200B;를 로드하려면 브라우저에서 **안전하지 않은 스크립트 로드 허용**&#x200B;을(를) 활성화하고 페이지를 다시 로드하십시오.

      ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Visual Experience Composer 편집기에서 WKND Site 홈 페이지가 열려 있습니다.

      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **경험 B** 위로 마우스를 가져간 후 다른 옵션 보기를 선택하십시오.

      ![경험 B](assets/personalization-use-case-2/redirect-url.png)
   8. **URL로 리디렉션** 옵션을 선택하고 새 WKND 홈 페이지의 URL을 입력합니다. (http://localhost:4503/content/wknd/en1.html)

      ![경험 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. 변경 내용을 **저장**&#x200B;하고 다음 활동 만들기 단계를 계속합니다.
   10. **트래픽 할당 방법**&#x200B;을 수동으로 선택하고 **경험 B**&#x200B;에 100% 트래픽을 할당합니다.

       ![경험 B 트래픽](assets/personalization-use-case-2/traffic.png)
   11. **다음**&#x200B;을 클릭합니다.
   12. 활동에 대한 **목표 지표**&#x200B;를 제공하고 A/B 테스트를 저장하고 닫습니다.

       ![A/B 테스트 목표 지표](assets/personalization-use-case-2/goal-metric.png)
   13. 활동에 대한 이름(**WKND 홈 페이지 다시 디자인**)을 입력하고 변경 사항을 저장하십시오.
   14. 활동 세부 정보 화면에서 활동을 **활성화**&#x200B;하세요.

       ![활동 활성화](assets/personalization-use-case-2/ab-activate.png)
   15. WKND 홈 페이지(http://localhost:4503/content/wknd/en.html)로 이동한 다음 다시 설계된 WKND 사이트 홈 페이지(http://localhost:4503/content/wknd/en1.html)로 리디렉션됩니다.

      ![다시 디자인된 WKND 홈 페이지](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 요약

이 장에서 마케터는 Adobe Target을 사용하여 AEM에서 호스팅되는 사이트 페이지를 새 페이지로 리디렉션하는 활동을 만들 수 있습니다.
