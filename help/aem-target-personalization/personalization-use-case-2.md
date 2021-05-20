---
title: Adobe Target을 사용한 개인화
seo-title: Adobe Target을 사용한 개인화
description: Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다.
seo-description: Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다.
feature: 경험 구성요소
topic: 개인화
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Adobe Target을 사용한 전체 웹 페이지 경험의 개인화

이전 장에서는 경험 조각으로 만들고 AEM에서 HTML 오퍼로 내보낸 콘텐츠를 사용하여 Adobe Target 내에서 지리적 위치 기반 활동을 만드는 방법을 알아보았습니다.

이 장에서는 활동 만들기 를 통해 AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션합니다.

## 시나리오 개요

WKND 사이트는 홈 페이지를 다시 디자인했으며 현재 홈 페이지 방문자를 새 홈 페이지로 리디렉션하려고 합니다. 동시에, 재설계된 홈 페이지가 사용자 참여도와 매출을 개선하는 방법을 이해합니다. 마케터는 방문자를 새 홈 페이지로 리디렉션하는 활동을 만드는 작업을 지정받았습니다. WKND 사이트 홈 페이지를 살펴보고 Adobe Target을 사용하여 활동을 만드는 방법을 알아봅니다.

### 관련 사용자

이 연습에서는 다음 사용자가 참여해야 하며 관리 액세스 권한이 필요할 수 있는 일부 작업을 수행해야 합니다.

* **컨텐츠 프로듀서/컨텐츠 편집기** (Adobe Experience Manager)
* **마케터** (Adobe Target/최적화 팀)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 전제 조건

* **AEM**
   * [localhost 4502 ](./implementation.md#getting-aem) 및 4503에서 각각 AEM 작성자 및 게시 설치
   * [Adobe Experience Platform Launch을 사용하여 Adobe Target과 통합](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 조직 Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com에 액세스
   * 다음 솔루션으로 제공된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## 컨텐츠 편집기 활동

1. 마케터는 AEM 컨텐츠 편집기와 함께 WKND 홈 페이지 재디자인 논의를 시작하고 요구 사항을 자세히 설명합니다.
   * ***요구 사항*** :카드 기반 디자인을 사용하여 WKND 사이트 홈 페이지를 재설계합니다.
2. 그런 다음 요구 사항을 기반으로 AEM 컨텐츠 편집기는 카드 기반 디자인으로 새 WKND 사이트 홈 페이지를 만들고 새 홈 페이지를 게시합니다.

## 마케터 활동

1. 마케터는 리디렉션 오퍼를 경험으로 사용하고 성공 목표 및 지표가 추가된 상태로 새 홈 페이지에 할당된 100% 웹 사이트 트래픽을 사용하여 A/B 타겟 활동을 만듭니다.
   1. Adobe Target 창에서 **활동** 탭으로 이동합니다.
   2. **활동 만들기** 단추를 클릭하고 활동 유형을 **A/B 테스트** 로 선택합니다.

      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-ab-activity.png)
   3. **웹** 채널을 선택하고 **시각적 경험 작성기**&#x200B;를 선택합니다.
   4. **활동 URL**&#x200B;을 입력하고 **다음**을 클릭하여 시각적 경험 작성기를 엽니다.
      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **시각적 경험 작성기**&#x200B;를 로드하려면 브라우저에서 **안전하지 않은 스크립트 로드 허용**을(를) 활성화하고 페이지를 다시 로드합니다.
      ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. WKND 사이트 홈 페이지가 시각적 경험 작성기 편집기에서 열려 있는 것을 확인합니다.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **경험 B** 위로 마우스를 가져간 후 다른 옵션 보기 를 선택합니다.
      ![경험 B](assets/personalization-use-case-2/redirect-url.png)
   8. **URL로 리디렉션** 옵션을 선택하고 새 WKND 홈 페이지의 URL을 입력합니다. (http://localhost:4503/content/wknd/en1.html)
      ![경험 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** 변경 사항을 저장하고 활동 생성의 다음 단계를 계속 진행합니다.
   10. **트래픽 할당 방법**&#x200B;을 수동으로 선택하고 **경험 B**에 100% 트래픽을 할당합니다.
      ![경험 B 트래픽](assets/personalization-use-case-2/traffic.png)
   11. **다음**&#x200B;을 클릭합니다.
   12. 활동에 대한 **목표 지표**를 제공하고 A/B 테스트를 저장하고 닫습니다.
      ![A/B 테스트 목표 지표](assets/personalization-use-case-2/goal-metric.png)
   13. 활동에 대한 이름(**WKND 홈 페이지 재디자인**)을 제공하고 변경 사항을 저장합니다.
   14. 활동 세부 사항 화면에서 활동을 **활성화**해야 합니다.
      ![활동 활성화](assets/personalization-use-case-2/ab-activate.png)
   15. WKND 홈 페이지(http://localhost:4503/content/wknd/en.html)으로 이동하면 다시 디자인된 WKND 사이트 홈 페이지(http://localhost:4503/content/wknd/en1.html)으로 리디렉션됩니다.
      ![WKND 홈 페이지 재설계](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 요약

이 장에서 마케터는 AEM에서 호스팅되는 사이트 페이지를 Adobe Target을 사용하여 새 페이지로 리디렉션하는 활동을 만들 수 있었습니다.
