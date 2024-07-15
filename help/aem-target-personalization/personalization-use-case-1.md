---
title: AEM Experience Fragments 및 Adobe Target을 사용한 Personalization
description: Adobe Experience Manager Experience Fragments 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 튜토리얼입니다.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1088
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 0%

---

# AEM Experience Fragments 및 Adobe Target을 사용한 Personalization

AEM Experience Fragments를 HTML 오퍼로서 Adobe Target으로 내보낼 수 있으므로 AEM의 편의성과 기능을 Target의 강력한 AI(Automated Intelligence) 및 기계 학습(ML) 기능과 결합하여 경험을 다양한 규모로 테스트 및 개인화할 수 있습니다.

AEM은 모든 콘텐츠 및 에셋을 중앙 위치에 가져와서 개인화 전략을 실행합니다. AEM을 사용하면 코드를 작성하지 않고도 한 위치에서 데스크탑, 태블릿 및 모바일 장치의 콘텐츠를 쉽게 만들 수 있습니다. 모든 디바이스를 위해 페이지를 만들 필요가 없이, 컨텐츠를 사용하여 각 경험이 자동으로 조정됩니다. AEM

Target을 사용하면 행동 변수, 컨텍스트 변수 및 오프라인 변수를 통합하는 규칙 기반 및 AI 기반 머신 러닝 접근 방식의 조합을 기반으로 다양한 규모로 개인화된 경험을 제공할 수 있습니다.  Target을 사용하면 A/B와 MVT(다변량) 활동을 쉽게 설정 및 실행하여 최상의 오퍼, 콘텐츠 및 경험을 결정할 수 있습니다.

경험 조각은 Target을 사용하여 비즈니스 결과를 이끄는 마케터와 콘텐츠 크리에이터를 연결하기 위한 매우 큰 단계를 나타냅니다.

## 시나리오 개요

WKND 사이트는 미국 전역의 **SkateFest Challenge**&#x200B;을(를) 웹 사이트를 통해 발표할 계획이며, 사이트 사용자가 각 주에서 실시하는 오디션에 등록하도록 합니다. 마케터로서 WKND 사이트 홈 페이지에서 캠페인을 실행하기 위해 사용자의 위치와 관련된 배너 메시지와 이벤트 세부 사항 페이지에 대한 링크가 있는 작업이 할당되었습니다. WKND 사이트 홈 페이지를 살펴보고 현재 위치를 기반으로 사용자에 개인화된 경험을 만들고 전달하는 방법을 알아보겠습니다.

### 관련 사용자

이 연습을 수행하려면 다음 사용자를 참여시키고 관리 액세스 권한이 필요할 수 있는 몇 가지 작업을 수행해야 합니다.

* **콘텐츠 제작자/콘텐츠 편집기**(Adobe Experience Manager)
* **마케터**(Adobe Target/최적화 팀)

### 사전 요구 사항

* **AEM**
   * localhost 4502 및 4503에서 각각 실행 중인 [AEM 작성자 및 게시 인스턴스](./implementation.md#getting-aem).
* **Experience Cloud**
   * 조직에 대한 액세스 권한 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 프로비저닝된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 마케터는 AEM 콘텐츠 편집기와 WKND SkateFest 캠페인 토론을 시작하고 요구 사항에 대해 자세히 설명합니다.
   * ***요구 사항***: 미국에 있는 각 주의 방문자를 위한 개인화된 콘텐츠로 WKND 사이트 홈 페이지에서 WKND SkateFest 캠페인을 홍보합니다. 배경 이미지, 텍스트 및 단추를 포함하는 홈 페이지 캐러셀 아래에 새 콘텐츠 블록을 추가합니다.
      * **배경 이미지**: 이미지는 사용자가 WKND 사이트 페이지를 방문한 상태와 관련이 있어야 합니다.
      * **텍스트**: &quot;Audition 등록&quot;
      * **단추**: WKND SkateFest 페이지를 가리키는 &quot;이벤트 세부 정보&quot;
      * **WKND SkateFest 페이지**: 오디션 장소, 날짜 및 시간을 포함한 이벤트 세부 정보가 포함된 새 페이지입니다.
1. AEM 콘텐츠 편집기는 요구 사항을 기반으로 콘텐츠 블록에 대한 경험 조각을 만들어 Adobe Target에 오퍼로 내보냅니다. 미국의 모든 상태에 대해 개인화된 콘텐츠를 제공하기 위해 콘텐츠 작성자는 경험 조각 마스터 변형을 한 개 만든 다음 각 상태에 대해 한 개씩 50개의 다른 변형을 만들 수 있습니다. 관련 이미지 및 텍스트가 있는 각 상태 변형에 대한 콘텐츠는 수동으로 편집할 수 있습니다. 경험 조각을 작성할 때 콘텐츠 편집기는 에셋 파인더 옵션을 사용하여 AEM Assets 내에서 사용할 수 있는 모든 에셋에 빠르게 액세스할 수 있습니다. 경험 조각을 Adobe Target으로 내보내면 해당 변형도 모두 Adobe Target에 오퍼로 푸시됩니다.

1. AEM에서 오퍼로 경험 조각을 Adobe Target으로 내보낸 후 마케터는 이러한 오퍼를 사용하여 Target에서 활동을 만들 수 있습니다. 마케터는 WKND site SkateFest 캠페인을 기반으로 각 상태의 WKND 사이트 방문자에게 개인화된 경험을 만들어 제공해야 합니다. 경험 타깃팅 활동을 만들려면 마케터는 대상을 식별해야 합니다. WKND SkateFest 캠페인의 경우 WKND 웹 사이트를 방문하는 위치에 따라 50개의 개별 대상을 만들어야 합니다.
   * [대상](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01)은(는) 활동에 대한 대상을 정의하며 타깃팅을 사용할 수 있는 모든 곳에서 사용됩니다. Target 대상은 정의된 방문자 기준 세트입니다. 오퍼를 특정 대상(또는 세그먼트)에 타기팅할 수 있습니다. 해당 대상에 속하는 방문자에게만 이들을 타깃으로 하는 경험이 표시됩니다.  예를 들어 특정 브라우저나 특정 지리적 위치의 방문자로 구성된 대상에 오퍼를 전달할 수 있습니다.
   * [오퍼](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)는 캠페인이나 활동 중에 웹 페이지에 표시되는 콘텐츠입니다. 웹 페이지를 테스트할 때 위치에 있는 서로 다른 오퍼를 사용하여 각 경험의 성공 여부를 측정합니다. 오퍼에는 다음을 비롯한 다양한 유형의 콘텐츠가 포함될 수 있습니다.
      * 이미지
      * 텍스트
      * **HTML**
         * *HTML 오퍼가 이 시나리오의 활동에 사용됨*
      * 링크
      * 버튼

## 콘텐츠 편집기 활동

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>경험 조각을 Adobe Target으로 내보내기 전에 Publish 합니다.

## 마케터 활동

### 지리 기반의 타깃팅으로 대상 만들기 {#marketer-audience}

1. 조직 [Adobe Experience Cloud](https://experiencecloud.adobe.com/)(`<https://<yourcompany>.experiencecloud.adobe.com`)으로 이동
1. Adobe ID을 사용하여 로그인하고 올바른 조직에 속해 있는지 확인하십시오.
1. 솔루션 전환기에서 **Target**&#x200B;을 클릭한 다음 **시작** Adobe Target을 클릭합니다.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. **오퍼** 탭으로 이동하여 &quot;WKND&quot; 오퍼를 검색합니다. AEM에서 HTML 오퍼로 내보낸 경험 조각 변형 목록을 볼 수 있습니다. 각 오퍼는 상태에 해당합니다. 예를 들어 *WKND SkateFest California*&#x200B;은(는) 캘리포니아의 WKND 사이트 방문자에게 제공되는 오퍼입니다.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 기본 탐색에서 **대상**&#x200B;을 클릭합니다.

   마케터는 미국의 각 주에서 온 WKND 사이트 방문자를 위해 50개의 개별 대상을 만들어야 합니다.

1. 대상을 만들려면 **대상 만들기** 단추를 클릭하고 대상의 이름을 제공하십시오.

   **대상 이름 형식: WKND-\&lt;*상태*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. **규칙 추가 > 지역**&#x200B;을 클릭합니다.
1. **선택**&#x200B;을 클릭한 후 다음 옵션 중 하나를 선택합니다.
   * 국가
   * **상태** *(WKND Site SkateFest 캠페인에 대한 상태 선택)*
   * 도시
   * 우편번호
   * 위도
   * 경도
   * DMA
   * 모바일 통신사

   **지역** - 국가, 시/도, 도시, 우편 번호, DMA 또는 이동통신사를 포함하여 지리적 위치를 기반으로 하는 사용자를 타깃팅하려면 대상을 사용합니다. 지리적 위치 매개 변수를 사용하면 방문자의 지리적 위치에 따라 활동과 경험을 타깃팅할 수 있습니다. 이 데이터는 각 Target 요청과 함께 전송되며 방문자의 IP 주소를 기반으로 합니다. 이러한 매개 변수는 타깃팅 값과 같이 선택합니다.

   >[!NOTE]
   >방문자의 IP 주소는 해당 방문자에 대한 지역 타겟팅 매개 변수를 해결하기 위해 방문(세션)당 한 번씩 mbox 요청과 함께 전달됩니다.

1. 연산자를 **일치**(으)로 선택하고 적절한 값(예: 캘리포니아)을 입력한 다음 변경 내용을 **저장**&#x200B;합니다. 이 경우에서 주 이름을 입력합니다.

   ![Adobe Target- 지역 규칙](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >대상자에게 여러 규칙을 할당할 수 있습니다.

1. 6~9단계를 반복하여 다른 상태에 대한 대상자를 만듭니다.

   ![Adobe Target- WKND 대상](assets/personalization-use-case-1/adobe-target-audiences-50.png)

이 시점에서 미합중국의 여러 주에 걸쳐 모든 WKND 사이트 방문자에 대한 대상을 성공적으로 만들었으며 각 주에 대한 해당 HTML 오퍼도 있습니다. 이제 경험 타깃팅 활동을 만들어 WKND 사이트 홈 페이지에 대한 해당 오퍼로 대상을 타깃팅해 보겠습니다.

### 지리 기반의 타깃팅으로 활동 만들기

1. Adobe Target 창에서 **활동** 탭으로 이동합니다.
1. **활동 만들기**&#x200B;를 클릭하고 **경험 타깃팅** 활동 유형을 선택합니다.
1. **웹** 채널을 선택하고 **시각적 경험 작성기**&#x200B;를 선택합니다.
1. **활동 URL**&#x200B;을 입력하고 **다음**&#x200B;을 클릭하여 시각적 경험 작성기를 엽니다.

   WKND 사이트 홈 페이지 Publish URL: http://localhost:4503/content/wknd/en.html

   ![경험 타깃팅 활동](assets/personalization-use-case-1/target-activity.png)

1. **시각적 경험 작성기**&#x200B;를 로드하려면 브라우저에서 **안전하지 않은 스크립트 로드 허용**&#x200B;을(를) 활성화하고 페이지를 다시 로드하십시오.

   ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Visual Experience Composer 편집기에서 WKND Site 홈 페이지가 열려 있습니다.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VEC에 대상을 추가하려면 대상 아래의 **경험 타깃팅 추가**&#x200B;를 클릭하고 WKND-California 대상을 선택한 후 **다음**&#x200B;을 클릭합니다.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC 내의 WKND 사이트 페이지를 클릭하고 WKND-California 대상에 대한 오퍼를 추가할 HTML 요소를 선택한 다음 **다음으로 바꾸기** 옵션을 선택한 다음 **HTML 오퍼**&#x200B;를 선택합니다.

   ![경험 타깃팅 활동](assets/personalization-use-case-1/vec-selecting-div.png)

1. 오퍼 선택 UI에서 **WKND-California** 대상에 대한 **WKND SkateFest California** HTML 오퍼를 선택하고 **완료**&#x200B;를 클릭합니다.
1. 이제 WKND-California 대상의 WKND 사이트 페이지에 추가된 **WKND SkateFest California** HTML 오퍼를 볼 수 있습니다.
1. 7~10단계를 반복하여 다른 상태에 대한 경험 타깃팅을 추가하고 해당 HTML 오퍼를 선택합니다.
1. 계속하려면 **다음**&#x200B;을 클릭하세요. 그러면 경험에 대한 대상 매핑을 볼 수 있습니다.
1. 목표 및 설정으로 이동하려면 **다음**&#x200B;을 클릭하세요.
1. 보고 소스를 선택하고 활동에 대한 기본 목표를 식별합니다. 시나리오에서는 보고 Source을 **Adobe Target**, 측정 활동을 **전환**, 페이지에서 본 작업 및 WKND SkateFest 세부 정보 페이지를 가리키는 URL로 선택하겠습니다.

   ![목표 및 타깃팅 - 타겟](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Adobe Analytics을 보고 소스로 선택할 수도 있습니다.

1. 현재 활동 이름을 마우스로 가리키면 이름을 **WKND SkateFest - USA**(으)로 바꾼 다음 변경 내용을 **저장 및 닫기**&#x200B;할 수 있습니다.
1. 활동 세부 정보 화면에서 활동을 **활성화**&#x200B;하세요.

   ![활동 활성화](assets/personalization-use-case-1/activate-activity.png)

1. 이제 모든 WKND 사이트 방문자에게 WKND SkateFest 캠페인이 라이브됩니다.
1. [WKND 사이트 홈 페이지](http://localhost:4503/content/wknd/en.html)(으)로 이동하면 지리적 위치(*주: 캘리포니아*)를 기반으로 하는 WKND SkateFest 오퍼를 볼 수 있습니다.

   ![활동 QA](assets/personalization-use-case-1/wknd-california.png)

### 타겟 활동 QA

1. **활동 세부 정보 > 개요** 탭에서 **활동 QA** 단추를 클릭하면 모든 경험에 대한 직접 QA 링크를 가져올 수 있습니다.

   ![활동 QA](assets/personalization-use-case-1/activity-qa.png)

1. [WKND 사이트 홈 페이지](http://localhost:4503/content/wknd/en.html)로 이동하면 지리적 위치(주)에 따라 WKND SkateFest 오퍼를 볼 수 있습니다.
1. 아래 비디오를 시청하여 오퍼가 페이지에 전달되는 방법, 응답 토큰을 맞춤화하는 방법 및 품질 검사를 수행하는 방법을 알아보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 요약

이 장에서 컨텐츠 편집기는 Adobe Experience Manager 내에서 WKND SkateFest 캠페인을 지원하는 모든 컨텐츠를 만들고 Adobe Target에 HTML 오퍼로 내보내 사용자 지리적 위치를 기반으로 한 경험 타깃팅을 만들 수 있습니다.
