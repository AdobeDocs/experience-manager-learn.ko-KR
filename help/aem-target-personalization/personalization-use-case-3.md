---
title: Adobe Target 시각적 경험 작성기를 사용한 개인화
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Adobe Target VEC(시각적 경험 작성기)를 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여 주는 종단간 자습서입니다.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 2%

---

# Visual Experience Composer를 사용한 개인화

이 장에서는 을 사용하여 경험을 만드는 방법에 대해 알아봅니다. **시각적 경험 작성기** Target 내에서 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정함으로써.

## 시나리오 개요

WKND 사이트 홈 페이지에는 카드 레이아웃의 형태로 도시 주변에서 할 수 있는 로컬 활동 또는 모범 사례가 표시됩니다. 마케터로서 카드 레이아웃을 다시 정렬하여 사용자 참여 및 드라이브 전환에 어떻게 영향을 주는지 확인함으로써 홈 페이지를 수정하는 작업이 할당되었습니다.

### 관련 사용자

이 연습을 수행하려면 다음 사용자를 참여시키고 관리 액세스 권한이 필요할 수 있는 몇 가지 작업을 수행해야 합니다.

* **컨텐츠 제작자/컨텐츠 편집기** (Adobe Experience Manager)
* **마케터** (Adobe Target / 최적화 팀)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 사전 요구 사항

* **AEM**
   * [AEM 게시 인스턴스](./implementation.md#getting-aem) 4503에서 실행
   * [Adobe Experience Platform Launch을 사용하여 Adobe Target과 통합된 AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 조직에 대한 액세스 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud이 프로비저닝됨 [Adobe Target](https://experiencecloud.adobe.com)

## 마케터 활동

1. 마케터는 Adobe Target 내에서 A/B 타겟 활동을 만듭니다.
   1. Adobe Target 창에서 다음으로 이동합니다. **활동** 탭.
   2. 클릭 **활동 만들기** 버튼을 클릭하고 활동 유형을 다음으로 선택 **A/B 테스트**
      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-ab-activity.png)
   3. 다음 항목 선택 **웹** 채널 및 선택 **시각적 경험 작성기**.
   4. 다음을 입력합니다. **활동 URL** 및 클릭 **다음** 를 클릭하여 시각적 경험 작성기를 엽니다.
      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 대상 **시각적 경험 작성기** 로드하려면 활성화 **안전하지 않은 스크립트 로드 허용** 을 클릭하고 페이지를 다시 로드합니다.
      ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Visual Experience Composer 편집기에서 WKND Site 홈 페이지가 열려 있습니다.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **경험 A** 에서는 기본 WKND 홈 페이지를 제공하며 의 콘텐츠 레이아웃을 편집하겠습니다. **경험 B**.
      ![경험 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 카드 레이아웃 컨테이너 중 하나를 클릭합니다(*베스트 로스터스*) 및 선택 **재배열** 옵션을 선택합니다.
      ![컨테이너 선택](assets/personalization-use-case-3/container-selection.png)
   9. 재배열할 컨테이너를 클릭하고 원하는 위치로 드래그 앤 드롭합니다. 을(를) 재배열합니다. *베스트 로스터스* 첫 번째 행 1번째 열부터 첫 번째 행 3번째 열까지 컨테이너입니다. 이제 *베스트 로스터스* 컨테이너가 다음에 있습니다. *사진 전시회* 컨테이너.
      ![컨테이너 교체](assets/personalization-use-case-3/container-swap.png)
      **교체 후**
      ![컨테이너가 교체됨](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 마찬가지로 다른 카드 컨테이너의 위치를 다시 정렬합니다.
      ![컨테이너가 교체됨](assets/personalization-use-case-3/after-swap-all.png)
   11. 슬라이드 구성 요소 아래와 카드 레이아웃 위에 헤더 텍스트를 추가하겠습니다.
   12. 슬라이드 컨테이너를 클릭하고 **다음 항목 뒤에 삽입 > HTML** 옵션을 사용하여 HTML을 추가할 수 있습니다.
      ![추가 텍스트](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![추가 텍스트](assets/personalization-use-case-3/after-changes.png)
   13. 클릭 **다음** 을 클릭하여 활동을 계속합니다.
   14. 다음 항목 선택 **트래픽 할당 방법** as manual 및 allot 100% traffic to **경험 B**.
      ![경험 B 트래픽](assets/personalization-use-case-2/traffic.png)
   15. **다음**&#x200B;을 클릭합니다.
   16. 제공 **목표 지표** 을 클릭하여 A/B 테스트를 저장하고 닫습니다.
      ![A/B 테스트 목표 지표](assets/personalization-use-case-2/goal-metric.png)
   17. 이름 입력(**WKND 홈 페이지 새로 고침**)을 클릭하여 제품에서 사용할 수 있습니다.
   18. 활동 세부 사항 화면에서 다음을 확인하십시오. **활성화** 활동.
      ![활동 활성화](assets/personalization-use-case-3/save-activity.png)
   19. WKND 홈 페이지(http://localhost:4503/content/wknd/en.html)로 이동한 다음 WKND 홈 페이지 새로 고침 A/B 테스트 활동에 추가한 변경 사항을 확인합니다.
      ![WKND 홈 페이지 새로 고침](assets/personalization-use-case-3/activity-result.png)
   20. 브라우저 콘솔을 열고 네트워크 탭을 검사하여 WKND 홈 페이지 새로 고침 A/B 테스트 활동에 대한 타겟 응답을 찾습니다.
      ![네트워크 활동](assets/personalization-use-case-3/activity-result.png)

## 요약

이 장에서 마케터는 테스트를 실행하기 위한 코드를 변경하지 않고 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 시각적 경험 작성기를 사용하여 경험을 만들 수 있습니다.
