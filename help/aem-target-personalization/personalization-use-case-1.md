---
title: AEM 경험 구성요소 및 Adobe Target을 사용한 개인화
seo-title: Adobe Experience Manager(AEM) 경험 구성요소 및 Adobe Target을 사용한 개인화
description: Adobe Experience Manager 경험 구성요소 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다.
seo-description: Adobe Experience Manager 경험 구성요소 및 Adobe Target을 사용하여 개인화된 경험을 만들고 전달하는 방법을 보여주는 종단간 자습서입니다.
feature: 경험 구성요소
topic: 개인화
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1732'
ht-degree: 1%

---


# AEM 경험 구성요소 및 Adobe Target을 사용한 개인화

AEM Experience Fragments를 HTML 오퍼로 Adobe Target에 내보낼 수 있는 기능을 사용하면 AEM의 편의성과 기능을 Target의 강력한 AI(Automated Intelligence) 및 기계 학습(ML) 기능을 결합하여 경험을 다양한 규모로 테스트 및 개인화할 수 있습니다.

AEM에서는 모든 콘텐츠 및 자산을 중앙 위치에 가져와서 개인화 전략을 실행합니다. AEM을 사용하면 코드를 작성하지 않고도 한 위치에서 데스크톱, 태블릿 및 모바일 장치용 콘텐츠를 쉽게 만들 수 있습니다. 모든 장치를 위해 페이지를 만들 필요가 없이, AEM은 콘텐츠를 사용하여 각 경험을 자동으로 조정합니다.

Target을 사용하면 행동, 컨텍스트 및 오프라인 변수를 통합하는 규칙 기반 기계 학습 접근 방식과 AI 기반 기계 학습 접근 방식을 조합하여 규모에 맞게 개인화된 경험을 제공할 수 있습니다.  Target을 사용하면 A/B와 MVT(다변량) 활동을 쉽게 설정 및 실행하여 최상의 오퍼, 콘텐츠 및 경험을 결정할 수 있습니다.

경험 조각은 Target을 사용하여 비즈니스 결과를 이끄는 마케터와 콘텐츠 작성자를 연결하기 위한 매우 큰 단계를 나타냅니다.

## 시나리오 개요

WKND 사이트는 웹사이트를 통해 미국 전역에 **SkateFest Challenge**&#x200B;를 발표할 계획이며, 사이트 사용자가 각 주에서 수행되는 오디션에 등록하게 하려고 합니다. 마케터는 WKND 사이트 홈 페이지에서 사용자 위치와 관련된 배너 메시지와 이벤트 세부 사항 페이지에 대한 링크가 포함된 캠페인을 실행하는 작업이 지정되었습니다. WKND 사이트 홈 페이지를 살펴보고 현재 위치를 기반으로 사용자를 위해 개인화된 경험을 만들고 전달하는 방법을 알아봅니다.

### 관련 사용자

이 연습에서는 다음 사용자가 참여해야 하며 관리 액세스 권한이 필요할 수 있는 일부 작업을 수행해야 합니다.

* **컨텐츠 생성자/컨텐츠 편집기** (Adobe Experience Manager)
* **마케터** (Adobe Target/최적화 팀)

### 전제 조건

* **AEM**
   * [localhost 4502 ](./implementation.md#getting-aem) 및 4503에서 각각 AEM 작성자 및 게시 설치
* **Experience Cloud**
   * 조직 Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com에 액세스
   * 다음 솔루션으로 제공된 Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND 사이트 홈 페이지

![AEM Target 시나리오 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 마케터는 AEM 컨텐츠 편집기와 함께 WKND SkateFest 캠페인 토론을 시작하고 요구 사항을 자세히 설명합니다.
   * ***요구 사항***:WKND 사이트 홈 페이지에서 미국의 각 주 방문자를 위한 개인화된 콘텐츠를 사용하여 WKND SkateFest 캠페인을 홍보합니다. 배경 이미지, 텍스트 및 단추가 들어 있는 홈 페이지 회전 메뉴 아래에 새 콘텐츠 블록을 추가합니다.
      * **배경 이미지**:이미지는 사용자가 WKND 사이트 페이지를 방문하는 상태와 관련이 있어야 합니다.
      * **텍스트**:&quot;Audition 등록&quot;
      * **단추**:WKND SkateFest 페이지를 가리키는 &quot;이벤트 세부 사항&quot;
      * **WKND SkateFest 페이지**:오디션 장소, 날짜, 시간 등 이벤트 세부 사항이 포함된 새로운 페이지입니다.
1. 요구 사항을 기반으로 AEM 컨텐츠 편집기는 컨텐츠 블록에 대한 경험 조각을 만들어 Adobe Target에 오퍼로 내보냅니다. 미국의 모든 상태에 대해 개인화된 컨텐츠를 제공하기 위해 컨텐츠 작성자는 하나의 경험 조각 마스터 변형을 만든 다음 각 상태에 대해 하나씩 50개의 다른 변형을 만들 수 있습니다. 관련 이미지 및 텍스트가 있는 각 상태 변형에 대한 컨텐츠를 수동으로 편집할 수 있습니다. 경험 조각을 작성할 때 컨텐츠 편집기에서 자산 파인더 옵션을 사용하여 AEM Assets 내에서 사용할 수 있는 모든 자산에 빠르게 액세스할 수 있습니다. 경험 조각을 Adobe Target으로 내보내면 모든 변형이 오퍼로 Adobe Target에 푸시됩니다.

1. AEM에서 Adobe Target으로 경험 조각을 오퍼로 내보내고 나면 마케터는 이러한 오퍼를 사용하여 Target에서 활동을 만들 수 있습니다. WKND 사이트 SkateFest 캠페인을 기반으로 마케터는 각 주의 WKND 사이트 방문자에게 개인화된 경험을 만들어 제공해야 합니다. 경험 타깃팅 활동을 만들려면 마케터가 대상자를 식별해야 합니다. WKND SkateFest 캠페인을 위해 WKND 웹 사이트를 방문하는 위치에 따라 50개의 별도 대상을 만들어야 합니다.
   * [](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 대상은 활동에 대한 타겟을 정의하며 타깃팅을 사용할 수 있는 모든 위치에서 사용됩니다. Target 대상은 정의된 방문자 기준 세트입니다. 오퍼를 특정 대상(또는 세그먼트)에 타깃팅할 수 있습니다. 해당 대상에 속하는 방문자만 타깃팅된 경험을 보게 됩니다.  예를 들어 특정 브라우저를 사용하거나 특정 지리적 위치에서 오퍼를 사용하는 방문자로 구성된 대상에게 제공할 수 있습니다.
   * [오퍼](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)는 캠페인이나 활동 중에 웹 페이지에 표시되는 콘텐츠입니다. 웹 페이지를 테스트할 때 해당 위치에서 다른 오퍼가 있는 각 경험의 성공을 측정합니다. 오퍼에는 다음을 포함한 다양한 유형의 컨텐츠가 포함될 수 있습니다.
      * 이미지
      * 텍스트
      * **HTML**
         * *HTML 오퍼는 이 시나리오의 활동에 사용됩니다*
      * 링크
      * 단추

## 컨텐츠 편집기 활동

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Adobe Target으로 내보내기 전에 경험 조각을 게시합니다.

## 마케터 활동

### 지역 타깃팅 {#marketer-audience}으로 대상 만들기

1. 조직 [Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)로 이동합니다.
1. Adobe ID을 사용하여 로그인하고 올바른 조직에 있는지 확인하십시오.
1. 솔루션 전환기에서 **Target**&#x200B;을 클릭한 다음 **launch** Adobe Target을 클릭합니다.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. **오퍼** 탭으로 이동하여 &quot;WKND&quot; 오퍼를 검색합니다. AEM에서 HTML 오퍼로 내보낸 경험 조각 변형 목록을 볼 수 있어야 합니다. 각 오퍼는 상태에 해당합니다. 예를 들어 *WKND SkateFest California*&#x200B;는 캘리포니아에서 WKND 사이트 방문자에게 제공되는 오퍼입니다.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 기본 탐색에서 **대상**&#x200B;을 클릭합니다.

   마케터는 미국 각 주에서 온 WKND 사이트 방문자를 위해 50개의 별도 대상을 만들어야 합니다.

1. 대상을 만들려면 **대상 만들기** 단추를 클릭하고 대상 이름을 입력합니다.

   **대상 이름 형식 :WKND-\&lt;>state *\>***

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. **규칙 추가 > Geo**&#x200B;를 클릭합니다.
1. **선택**&#x200B;을 클릭한 후 다음 옵션 중 하나를 선택합니다.
   * 국가
   * **상태** *(WKND Site SkateFest Campaign에 대해 상태 선택)*
   * 도시
   * 우편 번호
   * 위도
   * 경도
   * DMA
   * 모바일 통신사

   **지역**  - 국가, 시/도, 도시, 우편 번호, DMA 또는 이동통신사를 포함하여 지리적 위치를 기반으로 하는 사용자를 타깃팅하려면 대상을 사용합니다. 지리적 위치 매개 변수를 사용하여 방문자의 지리적 위치에 따라 활동과 경험을 타깃팅할 수 있습니다. 이 데이터는 각 Target 요청을 통해 전송되며 방문자의 IP 주소를 기반으로 합니다. 이러한 매개 변수는 타깃팅 값과 같이 선택합니다.

   >[!NOTE]
   >방문자의 IP 주소는 해당 방문자에 대한 지역 타깃팅 매개 변수를 해결하기 위해 방문당 한 번씩 mbox 요청을 통해 전달됩니다.

1. 연산자를 **일치**&#x200B;로 선택하고 적절한 값을 제공합니다(예:California) 및 **변경 내용을 저장**&#x200B;합니다. 이 경우 상태 이름을 입력합니다.

   ![Adobe Target - 지역 규칙](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >대상에 여러 규칙을 할당할 수 있습니다.

1. 6~9단계를 반복하여 다른 상태에 대한 대상을 만듭니다.

   ![Adobe Target - WKND 대상](assets/personalization-use-case-1/adobe-target-audiences-50.png)

이 시점에서 미국 내 다른 여러 주에 있는 모든 WKND 사이트 방문자에 대한 대상을 성공적으로 만들고 각 주에 해당하는 HTML 오퍼를 보유했습니다. 이제 WKND 사이트 홈 페이지에 대한 해당 오퍼로 대상을 타깃팅하는 경험 타깃팅 활동을 만들겠습니다.

### 지리 기반의 타깃팅으로 활동 만들기

1. Adobe Target 창에서 **활동** 탭으로 이동합니다.
1. **활동 만들기**&#x200B;를 클릭하고 **경험 타깃팅** 활동 유형을 선택합니다.
1. **웹** 채널을 선택하고 **시각적 경험 작성기**&#x200B;를 선택합니다.
1. **활동 URL**&#x200B;을 입력하고 **다음**&#x200B;을 클릭하여 시각적 경험 작성기를 엽니다.

   WKND 사이트 홈 페이지 게시 URL:http://localhost:4503/content/wknd/en.html

   ![경험 타깃팅 활동](assets/personalization-use-case-1/target-activity.png)

1. **시각적 경험 작성기**&#x200B;를 로드하려면 브라우저에서 **안전하지 않은 스크립트 로드 허용**&#x200B;을(를) 활성화하고 페이지를 다시 로드합니다.

   ![경험 타깃팅 활동](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. WKND 사이트 홈 페이지가 시각적 경험 작성기 편집기에서 열려 있는 것을 확인합니다.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VEC에 대상을 추가하려면 대상자 아래의 **경험 타깃팅 추가**&#x200B;를 클릭하고 WKND-California 대상을 선택한 다음 **다음**&#x200B;을 클릭합니다.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC 내의 WKND 사이트 페이지를 클릭하고 HTML 요소를 선택하여 WKND-California 대상을 위한 오퍼를 추가하고 **다음으로 바꾸기** 옵션을 선택한 다음 **HTML 오퍼**&#x200B;를 선택합니다.

   ![경험 타깃팅 활동](assets/personalization-use-case-1/vec-selecting-div.png)

1. 오퍼에서 **WKND-California** 대상에 대한 **WKND SkateFest California** HTML 오퍼를 선택하고 **완료**&#x200B;를 클릭합니다.
1. 이제 WKND-California 대상의 WKND 사이트 페이지에 추가된 **WKND SkateFest California** HTML 오퍼를 볼 수 있습니다.
1. 7-10단계를 반복하여 다른 상태에 대한 경험 타깃팅을 추가하고 해당 HTML 오퍼를 선택합니다.
1. 계속하려면 **다음**&#x200B;을 클릭하십시오. 대상에 대한 매핑이 표시됩니다.
1. **다음**&#x200B;을 클릭하여 목표 및 설정으로 이동합니다.
1. 보고 소스를 선택하고 활동에 대한 기본 목표를 식별합니다. 시나리오의 경우 보고 소스를 **Adobe Target**&#x200B;으로 선택하고, 활동을 **전환**&#x200B;으로 측정하고, 페이지를 본 작업 및 WKND SkateDetails 페이지를 가리키는 URL을 선택하겠습니다.

   ![목표 및 타깃팅 - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Adobe Analytics을 보고 소스로 선택할 수도 있습니다.

1. 현재 활동 이름을 마우스로 가리키면 **WKND SkateFest - USA**, **변경 내용을 저장하고 닫기**&#x200B;할 수 있습니다.
1. 활동 세부 사항 화면에서 활동을 **활성화**&#x200B;해야 합니다.

   ![활동 활성화](assets/personalization-use-case-1/activate-activity.png)

1. 이제 WKND SkateFest Campaign이 모든 WKND 사이트 방문자에게 생방송으로 제공됩니다.
1. [WKND 사이트 홈 페이지](http://localhost:4503/content/wknd/en.html)로 이동하면 지리적 위치(*상태)를 기준으로 WKND SkateFest 오퍼를 볼 수 있습니다.California*).

   ![활동 QA](assets/personalization-use-case-1/wknd-california.png)

### Target 활동 QA

1. **활동 세부 사항 > 개요** 탭에서 **활동 QA** 단추를 클릭하면 모든 경험에 대한 직접 QA 링크를 가져올 수 있습니다.

   ![활동 QA](assets/personalization-use-case-1/activity-qa.png)

1. [WKND 사이트 홈 페이지](http://localhost:4503/content/wknd/en.html)로 이동하면 지리적 위치(상태)를 기준으로 WKND SkateFest 오퍼를 볼 수 있습니다.
1. 아래 비디오를 시청하여 오퍼가 페이지에 전달되는 방법, 응답 토큰을 사용자 지정하는 방법 및 품질 검사를 수행하는 방법을 이해합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 요약

이 장에서는 컨텐츠 편집기가 Adobe Experience Manager 내에서 WKND SkateFest 캠페인을 지원하도록 모든 컨텐츠를 만들어 사용자를 지리적 위치에 따라 경험 타깃팅을 만들기 위해 HTML 오퍼로 Adobe Target에 내보낼 수 있었습니다.
