---
title: Adobe Target 시각적 경험 작성기를 사용한 Personalization
description: Adobe Target VEC(시각적 경험 작성기)를 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여 주는 종단간 자습서입니다.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---

# 시각적 경험 작성기를 사용한 Personalization

이 장에서는 Target 내에서 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 **시각적 경험 작성기**&#x200B;를 사용하여 경험을 만드는 방법에 대해 살펴봅니다.

## 시나리오 개요

WKND 사이트 홈 페이지에는 카드 레이아웃의 형태로 도시 주변에서 할 수 있는 로컬 활동 또는 모범 사례가 표시됩니다. 마케터로서 카드 레이아웃을 다시 정렬하여 사용자 참여 및 드라이브 전환에 어떻게 영향을 주는지 확인함으로써 홈 페이지를 수정하는 작업이 할당되었습니다.

### 관련 사용자

이 연습을 수행하려면 다음 사용자를 참여시키고 관리 액세스 권한이 필요할 수 있는 몇 가지 작업을 수행해야 합니다.

* **콘텐츠 제작자/콘텐츠 편집기**(Adobe Experience Manager)
* **마케터**(Adobe Target/최적화 팀)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 사전 요구 사항

* **AEM**
   * 4503에서 실행 중인 [AEM 게시 인스턴스](./implementation.md#getting-aem)
   * [태그를 사용하여 Adobe Target과 통합된 AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 조직에 대한 액세스 권한 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * [Adobe Target](https://experiencecloud.adobe.com)(으)로 프로비전된 Experience Cloud

## 마케터 활동

1. 마케터는 Adobe Target 내에서 A/B 타겟 활동을 만듭니다.
   1. Adobe Target 창에서 **활동** 탭으로 이동합니다.
   2. **활동 만들기** 단추를 클릭하고 **A/B 테스트**(으)로 활동 유형을 선택합니다.
      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-ab-activity.png)
   3. **웹** 채널을 선택하고 **시각적 경험 작성기**&#x200B;를 선택합니다.
   4. **활동 URL**&#x200B;을 입력하고 **다음**을 클릭하여 시각적 경험 작성기를 엽니다.
      ![Adobe Target - 활동 만들기](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **시각적 경험 작성기**&#x200B;를 로드하려면 브라우저에서 **안전하지 않은 스크립트 로드 허용**을(를) 활성화하고 페이지를 다시 로드하십시오.
      ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Visual Experience Composer 편집기에서 WKND Site 홈 페이지가 열려 있습니다.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **경험 A**&#x200B;에서 기본 WKND 홈 페이지를 제공하며 **경험 B**의 콘텐츠 레이아웃을 편집해 보겠습니다.
      ![경험 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 카드 레이아웃 컨테이너 중 하나(*최상의 로스터*)를 클릭하고 **다시 정렬** 옵션을 선택합니다.
      ![컨테이너 선택](assets/personalization-use-case-3/container-selection.png)
   9. 재배열할 컨테이너를 클릭하고 원하는 위치로 드래그 앤 드롭합니다. *최상의 로스터* 컨테이너를 첫 번째 행 1번째 열에서 첫 번째 행 3번째 열로 다시 정렬하겠습니다. 이제 *최고의 로스터* 컨테이너가 *사진 전시회* 컨테이너 옆에 있습니다.
      ![컨테이너 교체](assets/personalization-use-case-3/container-swap.png)
      **교체 후**
      ![컨테이너가 교환됨](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 마찬가지로 다른 카드 컨테이너의 위치를 다시 정렬합니다.
      ![컨테이너가 교환됨](assets/personalization-use-case-3/after-swap-all.png)
   11. 슬라이드 구성 요소 아래와 카드 레이아웃 위에 헤더 텍스트를 추가하겠습니다.
   12. 슬라이드 컨테이너를 클릭하고 **다음 항목 뒤에 삽입 > HTML** 옵션을 선택하여 HTML을 추가합니다.
      ![텍스트 추가](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![텍스트 추가](assets/personalization-use-case-3/after-changes.png)
   13. 활동을 계속하려면 **다음**&#x200B;을 클릭하세요.
   14. **트래픽 할당 방법**&#x200B;을 수동으로 선택하고 **경험 B**에 100% 트래픽을 할당합니다.
      ![경험 B 트래픽](assets/personalization-use-case-2/traffic.png)
   15. **다음**&#x200B;을 클릭합니다.
   16. 활동에 대한 **목표 지표**를 제공하고 A/B 테스트를 저장하고 닫습니다.
      ![A/B 테스트 목표 지표](assets/personalization-use-case-2/goal-metric.png)
   17. 활동에 대한 이름(**WKND 홈 페이지 새로 고침**)을 입력하고 변경 사항을 저장하십시오.
   18. 활동 세부 정보 화면에서 활동을 **활성화**하세요.
      ![활동 활성화](assets/personalization-use-case-3/save-activity.png)
   19. WKND 홈 페이지(http://localhost:4503/content/wknd/en.html)로 이동한 다음 WKND 홈 페이지 새로 고침 A/B 테스트 활동에 추가한 변경 사항을 확인합니다.
      ![WKND 홈 페이지 새로 고침](assets/personalization-use-case-3/activity-result.png)
   20. 브라우저 콘솔을 열고 네트워크 탭을 검사하여 WKND 홈 페이지 새로 고침 A/B 테스트 활동에 대한 타겟 응답을 찾습니다.
      ![네트워크 활동](assets/personalization-use-case-3/activity-result.png)

## 요약

이 장에서 마케터는 테스트를 실행하기 위한 코드를 변경하지 않고 웹 페이지의 레이아웃 및 콘텐츠를 드래그 앤 드롭하고, 교체하고, 수정하여 시각적 경험 작성기를 사용하여 경험을 만들 수 있습니다.
