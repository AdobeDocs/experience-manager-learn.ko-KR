---
title: AEM Sites을 사용한 스타일 시스템 우수 사례 이해
description: Adobe Experience Manager Sites을 사용한 스타일 시스템 구현 모범 사례를 설명하는 세부 문서입니다.
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 3%

---

# 스타일 시스템 우수 사례 이해{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>다음 위치에서 내용을 검토하십시오 [스타일 시스템에 대한 코드 지정 방법 이해](style-system-technical-video-understand.md)를 사용하여 AEM 스타일 시스템에서 사용되는 BEM 과 유사한 규칙을 이해할 수 있습니다.

AEM 스타일 시스템에 대해 구현되는 두 가지 기본 방식이나 스타일이 있습니다.

* **레이아웃 스타일**
* **스타일 표시**

**레이아웃 스타일** 구성 요소의 많은 요소에 영향을 주어 구성 요소의 정의 및 식별 가능한 표현물(디자인 및 레이아웃)을 작성하는 데, 종종 특정 재사용 가능한 브랜드 개념에 맞춰 정렬됩니다. 예를 들어 티저 구성 요소는 기존 카드 기반 레이아웃, 가로 프로모션 스타일 또는 이미지에 텍스트를 오버레이하는 영웅 레이아웃으로 표시할 수 있습니다.

**스타일 표시** 레이아웃의 일부 변형에 영향을 주는 데 사용되지만 레이아웃 스타일의 기본 특성이나 의도는 변경되지 않습니다. 예를 들어, 히어로 레이아웃 스타일에는 색상 구성표를 기본 브랜드 색상 구성표에서 보조 브랜드 색상 구성표로 변경하는 표시 스타일이 있을 수 있습니다.

## 스타일 지정 조직 우수 사례 {#style-organization-best-practices}

AEM 작성자가 사용할 수 있는 스타일 이름을 정의할 때 다음을 수행하는 것이 가장 좋습니다.

* 작성자가 알고 있는 용어를 사용하여 스타일 이름 지정
* 스타일 옵션 수 최소화
* 브랜드 표준에 의해 허용되는 스타일 옵션과 조합만 표시합니다
* 효과가 있는 스타일 조합만 표시합니다.
   * 비효율적인 조합이 노출되는 경우, 적어도 잘못된 효과가 없는지 확인하십시오

AEM 작성자가 사용할 수 있는 가능한 스타일 조합 수가 증가하면 브랜드 표준에 대해 QA와 유효성이 검사되어야 하는 순열이 더 많이 존재합니다. 너무 많은 옵션을 사용하면 원하는 효과를 만드는 데 필요한 선택 사항이나 조합이 불분명해질 수 있으므로 작성자를 혼동시킬 수도 있습니다.

### 스타일 이름과 CSS 클래스 {#style-names-vs-css-classes}

스타일 이름 또는 AEM 작성자에게 제공되는 옵션과 구현 CSS 클래스 이름은 AEM에서 분리됩니다.

이를 통해 스타일 옵션을 AEM 작성자가 명확하게 알고 이해할 수 있는 어휘로 레이블 지정할 수 있지만, 이를 통해 CSS 개발자는 향후 방증, 의미 있는 방식으로 CSS 클래스의 이름을 지정할 수 있습니다. 예:

구성 요소에는 브랜드의 **기본** 및 **보조** 하지만 AEM 작성자는 색상을 **녹색** 및 **노란색**&#x200B;기본 및 보조 의 디자인 언어가 아니라

AEM 스타일 시스템에서는 작성자에게 친숙한 레이블을 사용하여 이러한 색상 표시 스타일을 표시할 수 있습니다 **녹색** 및 **노란색**, CSS 개발자가 다음 시맨틱 이름을 사용할 수 있도록 허용 `.cmp-component--primary-color` 및 `.cmp-component--secondary-color` 를 클릭하여 CSS에서 실제 스타일 구현을 정의합니다.

스타일 이름 **녹색** 에 매핑되어 있습니다. `.cmp-component--primary-color`, 및 **노란색** to `.cmp-component--secondary-color`.

차후에 회사의 브랜드 색상이 변경되는 경우 변경해야 하는 모든 것은 `.cmp-component--primary-color` 및 `.cmp-component--secondary-color`, 및 스타일 이름.

## 티저 구성 요소를 예로 들 수 있습니다 {#the-teaser-component-as-an-example-use-case}

다음은 티저 구성 요소에 여러 가지 다른 레이아웃 및 표시 스타일을 지정할 수 있는 스타일 지정 사용 사례입니다.

이렇게 하면 스타일 이름(작성자에게 노출됨)과 관련 CSS 클래스가 구성되는 방식을 살펴봅니다.

### 티저 구성 요소 스타일 구성 {#component-styles-configuration}

다음 이미지는 [!UICONTROL 스타일] 사용 사례에서 설명한 변형에 대한 티저 구성 요소의 구성입니다.

다음 [!UICONTROL 스타일 그룹] 이름, 레이아웃 및 표시에 대해서는 이 문서에서 스타일을 개념적으로 분류하는 데 사용되는 표시 스타일 및 레이아웃 스타일의 일반 개념과 일치합니다.

다음 [!UICONTROL 스타일 그룹] 이름 및 번호 [!UICONTROL 스타일 그룹] 구성 요소 사용 사례 및 프로젝트별 구성 요소 스타일 지정 규칙에 맞게 조정해야 합니다.

예: **표시** 스타일 그룹 이름의 이름을 지정할 수 있습니다. **색상**.

![스타일 그룹 표시](assets/style-config.png)

### 스타일 선택 메뉴 {#style-selection-menu}

아래 이미지는 [!UICONTROL 스타일] 메뉴 작성자는 와 상호 작용하여 구성 요소에 적절한 스타일을 선택합니다. 참고 사항 [!UICONTROL 스타일 그래프] 스타일 이름과 이름이 모두 작성자에게 표시됩니다.

![스타일 드롭다운 메뉴](assets/style-menu.png)

### 기본 스타일 {#default-style}

기본 스타일은 종종 구성 요소의 가장 일반적으로 사용되는 스타일일 뿐만 아니라 페이지에 추가할 때 티저의 기본 스타일이 지정되지 않은 뷰입니다.

기본 스타일의 공통성에 따라 CSS가 `.cmp-teaser` (수정자 없이) 또는 `.cmp-teaser--default`.

기본 스타일 규칙이 모든 변형에 적용되지 않는 것보다 더 자주 적용되는 경우 사용하는 것이 가장 좋습니다 `.cmp-teaser` 는 기본 스타일의 CSS 클래스로, 모든 변형이 암시적으로 상속되어야 하므로 BEM 과 유사한 규칙이 수행된다고 가정합니다. 그렇지 않은 경우 다음과 같은 기본 수정자를 통해 적용해야 합니다. `.cmp-teaser--default`에 추가되어야 하는 경우 [구성 요소의 스타일 구성 기본 CSS 클래스](#component-styles-configuration) 필드, 그렇지 않으면 이러한 스타일 규칙이 각 변수에서 재정의되어야 합니다.

&quot;명명된&quot; 스타일을 기본 스타일(예: 영웅 스타일)으로 할당할 수도 있습니다 `(.cmp-teaser--hero)` 아래에 정의되어 있지만, `.cmp-teaser` 또는 `.cmp-teaser--default` CSS 클래스 구현.

>[!NOTE]
>
>기본 레이아웃 스타일에 표시 스타일 이름이 없지만 작성자는 AEM 스타일 시스템 선택 도구에서 표시 옵션을 선택할 수 있습니다.
>
>이는 우수 사례를 위반하는 것입니다.
>
>**효과가 있는 스타일 조합만 표시합니다.**
>
>작성자가 **녹색** 아무 일도 일어나지 않을 것이다.
>
>이 사용 사례에서는 다른 모든 레이아웃 스타일이 브랜드 색상을 사용하여 색상을 사용할 수 있어야 하므로 이 위반을 인정합니다.
>
>에서 **프로모션(오른쪽 정렬)** 아래 섹션에서는 원하지 않는 스타일 조합을 방지하는 방법을 확인할 수 있습니다.

![기본 스타일](assets/default.png)

* **레이아웃 스타일**
   * 기본값
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**: `.cmp-teaser--promo` 또는 `.cmp-teaser--default`

### 프로모션 스타일 {#promo-style}

다음 **프로모션 레이아웃 스타일** 는 사이트에서 고부가가치 컨텐츠를 홍보하는 데 사용되며 웹 페이지에서 공간 범위를 넓히도록 가로로 배치되며, 검정색 텍스트를 사용하는 기본 프로모션 레이아웃 스타일로 브랜드 색상으로 스타일을 지정할 수 있어야 합니다.

이를 실현하려면 **레이아웃 스타일** 의 **Promo** 그리고 **디스플레이 스타일** 의 **녹색** 및 **노란색** 티저 구성 요소에 대한 AEM 스타일 시스템에 구성되어 있습니다.

#### 프로모션 기본값

![기본](assets/promo-default.png)

* **레이아웃 스타일**
   * 스타일 이름: **Promo**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**: `.cmp-teaser--promo`

#### 프로모션 기본

![기본](assets/promo-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **Promo**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo Secondary

![Promo Secondary](assets/promo-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **Promo**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 스타일 이름: **노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### 프로모션 오른쪽 정렬 스타일 {#promo-r-align}

다음 **프로모션 오른쪽 정렬** 레이아웃 스타일은 이미지와 텍스트 위치를 변형하는 프로모션 스타일의 변형입니다(오른쪽 이미지, 왼쪽 텍스트).

오른쪽 맞춤은 해당 코어에서 디스플레이 스타일이며, 프로모션 레이아웃 스타일과 함께 선택한 디스플레이 스타일로 AEM 스타일 시스템에 입력할 수 있습니다. 이는 다음의 모범 사례를 위반합니다.

**효과가 있는 스타일 조합만 표시합니다.**

..이 문제는 이미 [기본 스타일](#default-style).

오른쪽 맞춤은 프로모션 레이아웃 스타일에만 영향을 주며 다른 두 레이아웃 스타일에는 영향을 주지 않으므로 기본 및 주인공. 프로모션 레이아웃 스타일 컨텐츠를 마우스 오른쪽 단추로 정렬하는 CSS 클래스를 포함하는 새 레이아웃 스타일 프로모션(오른쪽 정렬)을 만들 수 있습니다. `cmp -teaser--alternate`.

여러 스타일을 단일 스타일 항목으로 결합하면 사용 가능한 스타일과 스타일 순열의 수를 줄일 수 있으므로 최소한으로 하는 것이 가장 좋습니다.

CSS 클래스의 이름을 확인합니다. `cmp-teaser--alternate`를 &quot;오른쪽 정렬&quot;의 작성자와 친숙한 명명법 과 일치시킬 필요가 없습니다.

#### 프로모션 오른쪽 정렬 기본값

![오른쪽 정렬](assets/promo-alternate-default.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### 프로모션 오른쪽 정렬 기본

![프로모션 오른쪽 정렬 기본](assets/promo-alternate-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 프로모션 오른쪽 정렬 보조

![프로모션 오른쪽 정렬 보조](assets/promo-alternate-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 스타일 이름: **노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 영웅 스타일 {#hero-style}

히어로 레이아웃 스타일은 구성 요소의 이미지를 제목과 링크가 겹쳐서 배경으로 표시합니다. 프로모션 레이아웃 스타일처럼 히어로 레이아웃 스타일은 브랜드 색상으로 색상을 사용할 수 있어야 합니다.

브랜드 색상으로 히어로 레이아웃 스타일에 색상을 지정하려면 프로모션 레이아웃 스타일에 사용된 것과 동일한 표시 스타일을 활용할 수 있습니다.

구성 요소별로 스타일 이름이 단일 CSS 클래스 세트에 매핑됩니다. 즉, 프로모션 레이아웃 스타일의 배경색을 지정하는 CSS 클래스 이름은 Hero 레이아웃 스타일의 텍스트와 링크에 색상을 지정해야 합니다.

그러나 CSS 규칙을 범위 지정함으로써 세밀하게 달성할 수 있지만, 이렇게 하려면 CSS 개발자가 이러한 순열을 AEM에서 어떻게 지정하는지 이해해야 합니다.

배경색을 위한 CSS **홍보** 기본(녹색) 색상을 사용하는 레이아웃 스타일:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS를 사용하여 **Hero** 기본(녹색) 색상을 사용하는 레이아웃 스타일:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Default

![Hero Style](assets/hero.png)

* **레이아웃 스타일**
   * 스타일 이름: **Hero**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**: `.cmp-teaser--hero`

#### 기본 영웅

![기본 영웅](assets/hero-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **Promo**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secondary

![Hero Secondary](assets/hero-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **Promo**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 스타일 이름: **노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(블록 요소 수정자) 설명서 웹 사이트](https://getbem.com/)
* [LESS 설명서 웹 사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
