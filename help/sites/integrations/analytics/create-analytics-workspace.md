---
title: Analysis Workspace을 사용하여 데이터 분석
description: Adobe Experience Manager 사이트에서 캡처한 데이터를 Adobe Analytics 보고서 세트의 지표 및 차원에 매핑하는 방법을 알아봅니다. Adobe Analytics의 Analysis Workspace 기능을 사용하여 자세한 보고 대시보드를 만드는 방법을 알아봅니다.
version: cloud-service
topic: 통합
feature: Adobe 클라이언트 데이터 레이어
role: User
level: Intermediate
kt: 6409
thumbnail: KT-6296.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2184'
ht-degree: 0%

---


# Analysis Workspace을 사용하여 데이터 분석

Adobe Experience Manager 사이트에서 캡처한 데이터를 Adobe Analytics 보고서 세트의 지표 및 차원에 매핑하는 방법을 알아봅니다. Adobe Analytics의 Analysis Workspace 기능을 사용하여 자세한 보고 대시보드를 만드는 방법을 알아봅니다.

## 빌드할 내용

WKND 마케팅 팀은 홈 페이지에서 가장 잘 수행하는 CTA(Call to Action) 단추를 이해하려고 합니다. 이 자습서에서는 Analysis Workspace에서 다양한 CTA 버튼의 성능을 시각화하고 사이트에서 사용자 동작을 이해하는 새 프로젝트를 만듭니다. 사용자가 WKND 홈 페이지에서 CTA(Call to Action) 단추를 클릭하면 Adobe Analytics을 사용하여 다음 정보가 캡처됩니다.

**Analytics 변수**

다음은 현재 추적 중인 Analytics 변수입니다.

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA 클릭 Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 목표 {#objective}

1. 새 보고서 세트를 만들거나 기존 보고서 세트를 사용합니다.
1. 보고서 세트에서 [전환 변수(eVar)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) 및 [성공 이벤트(이벤트)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/success-events/success-event.html)를 구성합니다.
1. 인사이트를 신속하게 작성, 분석 및 공유할 수 있는 도구의 도움으로 [Analysis Workspace 프로젝트](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)를 만들어 데이터를 분석할 수 있습니다.
1. 다른 팀 구성원과 Analysis Workspace 프로젝트를 공유합니다.

## 전제 조건

이 자습서는 [클릭한 구성 요소를 Adobe Analytics](./track-clicked-component.md)에서 계속 추적하는 것으로, 다음과 같이 가정합니다.

* [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)이 활성화된 **Launch 속성**
* **Adobe** Analytics 테스트/개발 보고서 세트 ID 및 추적 서버. [새 보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)에 대한 다음 설명서를 참조하십시오.
* [Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debugger 브라우저 확장 프로그램은 Adobe 데이터 레이어가 활성화된 AEM  [사이트](https://wknd.site/us/en.html) 에서 Launch 속성을 로드하여 구성됩니다. https://wknd.site/us/en

## 전환 변수(eVar) 및 성공 이벤트(이벤트)

Custom Insight 전환 변수(또는 eVar)은 사이트의 선택한 웹 페이지에서 Adobe 코드에 배치됩니다. 기본 목적은 사용자 지정 마케팅 보고서에서 전환 성공 지표를 세그먼트화하는 것입니다. eVar은 방문을 기반으로 할 수 있으며 쿠키와 유사하게 작동합니다. eVar 변수에 전달된 값은 미리 지정된 기간 동안 사용자를 따릅니다.

eVar을 방문자의 값으로 설정하면 Adobe은 값이 만료되기 전까지 해당 값을 자동으로 기억합니다. eVar 값이 활성 상태인 동안 방문자가 발생하는 모든 성공 이벤트는 eVar 값에 대해 계산됩니다.

eVar는 다음과 같은 원인과 효과를 측정하는 데 가장 잘 사용됩니다.

* 매출에 영향을 주는 내부 캠페인
* 궁극적으로 등록으로 이어지는 배너 광고
* 주문을 하기 전에 내부 검색이 사용된 횟수입니다

성공 이벤트는 추적할 수 있는 작업입니다. 성공 이벤트가 무엇인지 결정합니다. 예를 들어 방문자가 CTA 단추를 클릭하는 경우 클릭 이벤트를 성공 이벤트로 간주할 수 있습니다.

### eVar 구성

1. Adobe Experience Cloud 홈페이지에서 조직을 선택하고 Adobe Analytics을 시작합니다.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Analytics 도구 모음에서 **관리** > **보고서 세트**&#x200B;를 클릭하고 보고서 세트를 찾습니다.

   ![Analytics 보고서 세트](assets/create-analytics-workspace/select-report-suite.png)

1. 보고서 세트 > **설정 편집** > **전환** > **전환 변수**&#x200B;를 선택합니다.

   ![Analytics 전환 변수](assets/create-analytics-workspace/conversion-variables.png)

1. **새** 추가 옵션을 사용하여 다음과 같이 스키마를 매핑하도록 전환 변수를 만들겠습니다.

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![새 eVar 추가](assets/create-analytics-workspace/add-new-evars.png)

1. 각 eVar 및 **변경 내용을 저장**&#x200B;에 적절한 이름과 설명을 입력합니다. 다음 섹션에서 이러한 eVar를 사용하여 Analysis Workspace 프로젝트를 만들 것입니다. 따라서 사용자에게 친숙한 이름으로 변수를 쉽게 검색할 수 있습니다.

   ![eVar](assets/create-analytics-workspace/evars.png)

### 성공 이벤트 구성

다음으로, CTA 단추 클릭을 추적하기 위한 이벤트를 만들겠습니다.

1. **보고서 세트 관리자** 창에서 **보고서 세트 Id**&#x200B;를 선택하고 **설정 편집**&#x200B;을 클릭합니다.
1. **전환** > **성공 이벤트**&#x200B;를 클릭합니다.
1. **새로 추가** 옵션을 사용하여 새 사용자 지정 성공 이벤트를 만들어 CTA 단추 클릭을 추적한 다음 **변경 내용을 저장**&#x200B;합니다.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Analysis Workspace에서 새 프로젝트 만들기 {#workspace-project}

Analysis Workspace은 분석을 신속하게 작성하고 인사이트를 공유할 수 있는 유연한 브라우저 도구입니다. 드래그하여 놓기 인터페이스를 사용하여 분석을 만들고 시각화를 추가하여 데이터를 생동감 있게 표현하고 데이터 세트를 조정하며 조직 내 누구와도 프로젝트를 공유 및 예약할 수 있습니다.

그런 다음 새 [프로젝트](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace)를 만들어 사이트 전체에서 CTA 버튼의 성능을 분석할 대시보드를 만듭니다.

1. Analytics 도구 모음에서 **작업 공간**&#x200B;을 선택하고 **새 프로젝트 만들기**&#x200B;를 클릭합니다.

   ![작업 공간](assets/create-analytics-workspace/create-workspace.png)

1. **빈 프로젝트**&#x200B;에서 시작하거나, Adobe이 제공한 템플릿 또는 조직에서 만든 사용자 정의 템플릿 중 하나를 선택하여 미리 빌드된 템플릿 중 하나를 선택하십시오. 분석 또는 염두에 둔 사용 사례에 따라 몇 가지 템플릿을 사용할 수 있습니다. [사용 ](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) 가능한 다양한 템플릿 옵션에 대해 자세히 알아보십시오 .

   Analysis Workspace 프로젝트에서 패널, 표, 시각화 및 구성 요소 는 왼쪽 레일에서 액세스됩니다. 이는 프로젝트 구성 요소입니다.

   * **[구성 요소](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)**  - 구성 요소는 차원, 지표, 세그먼트 또는 날짜 범위입니다. 이 모든 구성 요소를 자유 형식 테이블에서 결합하여 비즈니스 질문에 답변할 수 있습니다. 분석하기 전에 각 구성 요소 유형을 숙지하십시오. 구성 요소 용어를 숙지하면 드래그 앤 드롭을 시작하여 자유 형식 테이블에서 분석을 빌드할 수 있습니다.
   * **[시각화](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)**  - 막대형 차트 또는 선형 차트와 같은 시각화 가 데이터 위에 추가되어 시각적으로 생동감 있게 표현됩니다. 맨 왼쪽 레일에서 가운데 시각화 아이콘을 선택하여 시각화 의 전체 목록을 확인합니다.
   * **[패널](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)**  - 패널은 테이블 및 시각화의 컬렉션입니다. Workspace의 왼쪽 위 아이콘에서 패널에 액세스할 수 있습니다. 패널 은 기간, 보고서 세트 또는 분석 사용 사례에 따라 프로젝트를 구성하려는 경우 유용합니다. Analysis Workspace에서는 다음 패널 유형을 사용할 수 있습니다.

   ![템플릿 선택](assets/create-analytics-workspace/workspace-tools.png)

### Analysis Workspace을 사용하여 데이터 시각화 추가

다음으로, WKND 사이트 홈 페이지에서 사용자가 CTA(Call to Action) 버튼과 상호 작용하는 방법을 시각적으로 보여주는 표를 만듭니다. 이러한 표현을 작성하려면 [클릭한 구성 요소를 Adobe Analytics](./track-clicked-component.md)에서 추적에 수집된 데이터를 사용하겠습니다. 다음은 WKND Site에 대한 작업 호출 버튼을 사용하여 사용자 상호 작용을 위해 추적된 데이터의 빠른 요약입니다.

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. **Page** 차원 구성 요소를 자유 형식 테이블로 끌어서 놓습니다. 이제 테이블 내에 표시되는 페이지 이름(eVar9) 및 해당 페이지 보기 수(발생 횟수)를 표시하는 시각화를 볼 수 있습니다.

   ![페이지 Dimension](assets/create-analytics-workspace/evar9-dimension.png)

1. **CTA 클릭** (event8) 지표를 발생 횟수 지표에 끌어다 놓고 바꿉니다. 이제 페이지에서 페이지 이름(eVar9)과 해당 CTA 클릭 이벤트 수를 표시하는 시각화를 볼 수 있습니다.

   ![페이지 지표 - CTA 클릭](assets/create-analytics-workspace/evar8-cta-click.png)

1. 페이지별로 템플릿 유형별로 분류하겠습니다. 구성 요소에서 페이지 템플릿 지표를 선택하고 페이지 템플릿 지표를 페이지 이름 차원으로 끌어서 놓습니다. 이제 해당 템플릿 유형별로 분류된 페이지 이름을 볼 수 있습니다.

   * **이전**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **이후**

      ![eVar5 지표](assets/create-analytics-workspace/evar5-metrics.png)

1. 사용자가 WKND 사이트 페이지에 있을 때 CTA 버튼과 상호 작용하는 방법을 이해하려면 단추 ID(eVar8) 지표를 추가하여 페이지 템플릿 지표를 추가로 분류해야 합니다.

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 아래에서는 페이지 템플릿별로 구분된 WKND 사이트의 시각적 표현을 볼 수 있으며, WKND 사이트 클릭 유도 작업(CTA) 단추와의 사용자 상호 작용에 따라 추가로 분류됩니다.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Adobe Analytics 분류를 사용하여 단추 ID 값을 사용자에게 친숙한 이름으로 바꿀 수 있습니다. 여기](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html)에서 특정 지표 [에 대한 분류를 만드는 방법에 대해 자세히 알아볼 수 있습니다. 이 경우 단추 ID를 사용자에게 친숙한 이름에 매핑하는 `eVar8`에 대한 분류 지표 `Button Section (Button ID)` 설정이 있습니다.

   ![단추 섹션](assets/create-analytics-workspace/button-section.png)

## Analytics 변수에 분류 추가

### 전환 분류

Analytics 분류는 Analytics 변수 데이터를 분류한 다음 보고서를 생성할 때 데이터를 다양한 방법으로 표시하는 방법입니다. Analytics Workspace 보고서에 단추 ID가 표시되는 방식을 개선하기 위해 단추 ID(eVar8)에 대한 분류 변수를 만들겠습니다. 분류할 때 변수와 해당 변수와 관련된 메타데이터 간의 관계를 설정하고 있습니다.

다음으로, Analytics 변수에 대한 분류를 만듭니다.

1. **관리** 도구 모음 메뉴에서 **보고서 세트**&#x200B;를 선택합니다.
1. **보고서 세트 관리자** 창에서 **보고서 세트 Id**&#x200B;를 선택하고 **설정 편집** > **전환** > **전환 분류**&#x200B;를 클릭합니다

   ![전환 분류](assets/create-analytics-workspace/conversion-classification.png)

1. **분류 유형 선택** 드롭다운 목록에서 변수(eVar8-단추 ID)를 선택하여 분류를 추가합니다.
1. 분류 섹션 아래에 나열된 분류 변수 옆에 있는 화살표를 클릭하여 새 분류를 추가합니다.

   ![전환 분류 유형](assets/create-analytics-workspace/select-classification-variable.png)

1. **분류 편집** 대화 상자에서 텍스트 분류에 적절한 이름을 제공합니다. 텍스트 분류 이름을 사용하는 차원 구성 요소가 만들어집니다.

   ![전환 분류 유형](assets/create-analytics-workspace/new-classification.png)

1. **** 변경 사항을 저장합니다.

### 분류 가져오기

가져오기를 사용하여 분류를 Adobe Analytics에 업로드합니다. 데이터를 가져오기 전에 업데이트를 위해 데이터를 내보낼 수도 있습니다. 가져오기 도구를 사용하여 가져오는 데이터는 특정 형식이어야 합니다. Adobe은 탭으로 구분된 데이터 파일에서 적절한 헤더 세부 정보와 함께 데이터 템플릿을 다운로드할 수 있는 옵션을 제공합니다. 이 템플릿에 새 데이터를 추가한 다음 FTP를 사용하여 브라우저에서 데이터 파일을 가져올 수 있습니다.

#### 분류 템플릿

분류를 마케팅 보고서로 가져오기 전에 분류 데이터 파일을 만드는 데 도움이 되는 템플릿을 다운로드할 수 있습니다. 데이터 파일은 원하는 분류를 열 제목으로 사용한 다음 해당 분류 제목 아래에 보고 데이터 세트를 구성합니다.

다음으로, 단추 ID(eVar8) 변수에 대한 분류 템플릿을 다운로드하겠습니다

1. **관리** > **분류 가져오기**&#x200B;로 이동합니다.
1. **템플릿 다운로드** 탭에서 전환 변수에 대한 분류 템플릿을 다운로드하겠습니다.
   ![전환 분류 유형](assets/create-analytics-workspace/classification-importer.png)

1. 템플릿 다운로드 탭에서 데이터 템플릿 구성을 지정합니다.
   * **보고서 세트 선택** : 템플릿에서 사용할 보고서 세트를 선택합니다. 보고서 세트와 데이터 세트가 일치해야 합니다.
   * **분류할 데이터 세트** : 데이터 파일에 대한 데이터 유형을 선택합니다. 이 메뉴에는 분류에 대해 구성된 보고서 세트의 모든 보고서가 포함됩니다.
   * **인코딩** : 데이터 파일의 문자 인코딩을 선택합니다. 기본 인코딩 형식은 UTF-8입니다.

1. **다운로드**&#x200B;를 클릭하고 템플릿 파일을 로컬 시스템에 저장합니다. 템플릿 파일은 탭으로 구분된 데이터 파일(.tab 파일 확장명)이며 대부분의 스프레드시트 애플리케이션에서 지원합니다.
1. 원하는 편집기를 사용하여 탭으로 구분된 데이터 파일을 엽니다.
1. 섹션의 9단계에서 각 eVar9 값에 대해 단추 ID(eVar9) 및 해당 단추 이름을 탭으로 구분된 파일에 추가합니다.

   ![키 값](assets/create-analytics-workspace/key-value.png)

1. **** 탭으로 구분된 파일을 저장합니다.
1. **파일 가져오기** 탭으로 이동합니다.
1. 파일 가져오기에 대한 대상을 구성합니다.
   * **보고서 세트 선택** : WKND Site AEM(보고서 세트)
   * **분류할 데이터 세트** : 단추 Id(전환 변수 eVar8)
1. **파일** 선택 옵션을 클릭하여 시스템에서 탭으로 구분된 파일을 업로드한 다음 **파일 가져오기**&#x200B;를 클릭합니다

   ![파일 가져오기](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 성공적으로 가져오면 내보내기에 적절한 변경 사항이 즉시 표시됩니다. 하지만 보고서의 데이터 변경 사항은 브라우저 가져오기를 사용하는 경우 최대 4시간, FTP 가져오기를 사용하는 경우 최대 24시간이 소요됩니다.

#### 전환 변수를 분류 변수로 바꾸기

1. Analytics 도구 모음에서 **작업 공간**&#x200B;을 선택하고 이 자습서의 [Analysis Workspace에서 새 프로젝트 만들기](#workspace-project) 섹션에서 작업 공간을 엽니다.

   ![작업 공간 단추 ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 다음으로, 작업 공간에서 CTA(Call to Action) 버튼의 ID를 표시하는 **단추 ID** 지표를 이전 단계에서 생성된 분류 이름으로 바꿉니다.

1. 구성 요소 파인더에서 **WKND CTA 단추**&#x200B;를 검색하고 **WKND CTA 단추(단추 Id)** 차원을 단추 ID 지표에 드래그하여 놓고 바꿉니다.

   * **이전**

      ![작업 영역 단추 전](assets/create-analytics-workspace/wknd-button-before.png)
   * **이후**

      ![작업 영역 단추 뒤](assets/create-analytics-workspace/wknd-button-after.png)

1. 이제 클릭유도문안(CTA) 단추의 단추 ID가 포함된 단추 ID 지표가 분류 템플릿에 제공된 해당 이름으로 대체되는 것을 볼 수 있습니다.
1. Analytics 작업 공간 테이블을 WKND 홈 페이지와 비교하고 CTA 단추 클릭 카운트 및 해당 분석을 이해하겠습니다. 작업 공간 자유 형식 테이블 데이터를 기반으로, 사용자가 호주 서부의 **SKI NOW** 단추와 호주 서부의 WKND 홈 페이지 캠핑에 대해 4번 클릭한 횟수가 22회임을 알 수 있습니다. **자세히 보기** 단추.

   ![CTA 보고서](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Adobe Analytics 작업 공간 프로젝트를 저장하고 적절한 이름과 설명을 제공해야 합니다. 선택적으로 작업 공간 프로젝트에 태그를 추가할 수 있습니다.

   ![프로젝트 저장](assets/create-analytics-workspace/save-project.png)

1. 프로젝트를 성공적으로 저장한 후 공유 옵션을 사용하여 다른 동료 또는 팀원과 작업 공간 프로젝트를 공유할 수 있습니다.

   ![프로젝트 공유](assets/create-analytics-workspace/share.png)

## 축하합니다!

Adobe Experience Manager 사이트에서 캡처한 데이터를 Adobe Analytics 보고서 세트의 지표 및 차원에 매핑하고, 지표에 대한 분류를 수행하고, Adobe Analytics의 Analysis Workspace 기능을 사용하여 세부 보고 대시보드를 만드는 방법을 방금 알아보았습니다.

