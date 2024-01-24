---
title: AEM Sites을 사용한 스타일 시스템 모범 사례 이해
description: Adobe Experience Manager Sites을 사용한 스타일 시스템 구현과 관련된 모범 사례를 설명하는 자세한 문서입니다.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 387
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# 스타일 시스템 모범 사례 이해{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>다음 위치에서 콘텐츠를 검토하십시오. [스타일 시스템에 대한 코드 작성 방법 이해](style-system-technical-video-understand.md)를 사용하여 AEM 스타일 시스템에서 사용하는 BEM과 유사한 규칙을 이해할 수 있습니다.

AEM 스타일 시스템에 대해 구현되는 두 가지 주요 버전 또는 스타일은 다음과 같습니다.

* **레이아웃 스타일**
* **스타일 표시**

**레이아웃 스타일** 은 구성 요소의 많은 요소에 영향을 주어 구성 요소의 잘 정의되고 식별 가능한 렌디션(디자인 및 레이아웃)을 만듭니다. 이는 종종 재사용 가능한 특정 브랜드 개념에 맞게 정렬됩니다. 예를 들어 티저 구성 요소는 기존의 카드 기반 레이아웃, 가로 프로모션 스타일 또는 이미지에 텍스트를 오버레이하는 영웅 레이아웃으로 표시될 수 있습니다.

**스타일 표시** 레이아웃 스타일의 작은 변형에 영향을 주는 데 사용되지만, 레이아웃 스타일의 기본 특성이나 의도는 변경되지 않습니다. 예를 들어, Hero 레이아웃 스타일에는 기본 브랜드 색상 구성표에서 보조 브랜드 색상 구성표로 색상 구성표를 변경하는 표시 스타일이 있을 수 있습니다.

## 스타일 조직 모범 사례 {#style-organization-best-practices}

AEM 작성자가 사용할 수 있는 스타일 이름을 정의할 때 가장 좋은 방법은 다음과 같습니다.

* 작성자가 이해한 어휘를 사용하여 스타일 이름 지정
* 스타일 옵션 수 최소화
* 브랜드 표준에서 허용하는 스타일 옵션 및 조합만 표시합니다.
* 효과가 있는 스타일 조합만 표시합니다.
   * 비효과적인 조합이 노출되면 적어도 나쁜 영향을 미치지 않는지 확인하십시오

AEM 작성자가 사용할 수 있는 가능한 스타일 조합의 수가 증가함에 따라 브랜드 표준에 대해 QA하고 유효성을 검사해야 하는 순열이 더 많이 존재합니다. 옵션이 너무 많으면 원하는 효과를 내기 위해 어떤 옵션이나 조합이 필요한지 명확하지 않게 될 수 있으므로 작성자에게 혼란을 줄 수도 있습니다.

### 스타일 이름과 CSS 클래스 {#style-names-vs-css-classes}

스타일 이름 또는 AEM 작성자에게 제공되는 옵션과 구현 CSS 클래스 이름은 AEM에서 분리됩니다.

이를 통해 스타일 옵션을 AEM 작성자가 명확히 이해하고 이해할 수 있는 어휘로 레이블을 지정할 수 있지만, CSS 개발자가 미래를 대비할 수 있는 의미론적인 방식으로 CSS 클래스의 이름을 지정할 수 있습니다. 예:

구성 요소에는 브랜드의 색상으로 색칠 수 있는 옵션이 있어야 합니다 **기본** 및 **보조** 그러나 색상 AEM 작성자는 색상을 다음과 같이 알고 있습니다. **녹색** 및 **노랑**&#x200B;기본 및 보조 디자인 언어가 아닌 입니다.

AEM 스타일 시스템은 작성자에게 친숙한 레이블을 사용하여 이러한 색상 표시 스타일을 노출할 수 있습니다 **녹색** 및 **노랑**, CSS 개발자가 의 시맨틱 이름 지정을 사용할 수 있도록 하는 동안 `.cmp-component--primary-color` 및 `.cmp-component--secondary-color` 를 사용하여 CSS의 실제 스타일 구현을 정의하십시오.

의 스타일 이름 **녹색** 이(가) 다음에 매핑됨 `.cmp-component--primary-color`, 및 **노랑** 끝 `.cmp-component--secondary-color`.

향후 회사의 브랜드 색상이 변경되는 경우 변경해야 하는 것은 의 단일 구현입니다. `.cmp-component--primary-color` 및 `.cmp-component--secondary-color`및 스타일 이름을 추가합니다.

## 티저 구성 요소의 사용 사례 예 {#the-teaser-component-as-an-example-use-case}

다음은 티저 구성 요소를 스타일링하여 여러 가지 레이아웃 및 디스플레이 스타일을 갖는 사용 사례입니다.

스타일 이름(작성자에게 노출됨)과 관련 CSS 클래스를 구성하는 방법을 살펴봅니다.

### 티저 구성 요소 스타일 구성 {#component-styles-configuration}

다음 이미지는 [!UICONTROL 스타일] 사용 사례에서 논의된 변형에 대한 티저 구성 요소 구성.

다음 [!UICONTROL 스타일 그룹] 이름, 레이아웃 및 표시는 우연히 이 문서의 스타일 유형을 개념적으로 분류하는 데 사용되는 표시 스타일 및 레이아웃 스타일의 일반적인 개념과 일치합니다.

다음 [!UICONTROL 스타일 그룹] 이름 및 번호 [!UICONTROL 스타일 그룹] 는 구성 요소 사용 사례 및 프로젝트별 구성 요소 스타일 규칙에 맞게 조정되어야 합니다.

예를 들어 **표시** 스타일 그룹 이름은 **색상**.

![스타일 그룹 표시](assets/style-config.png)

### 스타일 선택 메뉴 {#style-selection-menu}

아래 이미지는 [!UICONTROL 스타일] 메뉴 작성자는 와 상호 작용하여 구성 요소에 적합한 스타일을 선택합니다. 다음을 참고하십시오. [!UICONTROL 스타일 그래프] 스타일 이름 및 이름은 모두 작성자에게 표시됩니다.

![스타일 드롭다운 메뉴](assets/style-menu.png)

### 기본 스타일 {#default-style}

기본 스타일은 종종 가장 일반적으로 사용되는 구성 요소 스타일과 페이지에 추가될 때 스타일을 지정하지 않은 기본 티저 보기입니다.

기본 스타일의 공통성에 따라 CSS는 `.cmp-teaser` (수정자 없음) 또는 `.cmp-teaser--default`.

기본 스타일 규칙이 모든 변형에 적용되는 경우가 아닌 경우 를 사용하는 것이 좋습니다 `.cmp-teaser` 기본 스타일의 CSS 클래스로, 모든 변형은 암시적으로 상속해야 하므로 BEM과 유사한 규칙을 따른다고 가정합니다. 그렇지 않은 경우 다음과 같은 기본 수정자를 통해 적용해야 합니다. `.cmp-teaser--default`: 다음에 추가되어야 합니다. [구성 요소의 스타일 구성 기본 CSS 클래스](#component-styles-configuration) 필드, 그렇지 않으면 각 변형에서 이러한 스타일 규칙을 재정의해야 합니다.

Hero 스타일과 같은 기본 스타일로 &quot;명명된&quot; 스타일을 지정할 수도 있습니다 `(.cmp-teaser--hero)` 아래에 정의되어 있지만 기본 스타일은 `.cmp-teaser` 또는 `.cmp-teaser--default` CSS 클래스 구현.

>[!NOTE]
>
>기본 레이아웃 스타일에는 표시 스타일 이름이 없지만 작성자는 AEM 스타일 시스템 선택 도구에서 표시 옵션을 선택할 수 있습니다.
>
>이는 모범 사례에 위배됩니다.
>
>**효과가 있는 스타일 조합만 표시합니다.**
>
>작성자가 다음의 표시 스타일을 선택하는 경우 **녹색** 아무 일도 일어나지 않을 거야
>
>이 사용 사례에서는 다른 모든 레이아웃 스타일을 브랜드 색상을 사용하여 색상화할 수 있어야 하므로 이 위반을 인정합니다.
>
>다음에서 **프로모션(오른쪽 정렬)** 아래 섹션에서는 원하지 않는 스타일 조합을 방지하는 방법을 살펴보겠습니다.

![기본 스타일](assets/default.png)

* **레이아웃 스타일**
   * 기본값
* **표시 스타일**
   * 없음
* **유효 CSS 클래스**: `.cmp-teaser--promo` 또는 `.cmp-teaser--default`

### 프로모션 스타일 {#promo-style}

다음 **프로모션 레이아웃 스타일** 는 사이트에서 고부가가치 콘텐츠를 홍보하는 데 사용되며, 웹 페이지에서 스페이스 밴드를 차지하도록 가로로 배치되어 있으며 브랜드 색상으로 스타일을 지정할 수 있어야 합니다. 이때 기본 프로모션 레이아웃 스타일은 검정 텍스트를 사용합니다.

이 목표를 달성하려면 **레이아웃 스타일** / **프로모션** 및 **스타일 표시** / **녹색** 및 **노랑** 는 티저 구성 요소에 대해 AEM 스타일 시스템에서 구성됩니다.

#### 프로모션 기본값

![프로모션 기본값](assets/promo-default.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **표시 스타일**
   * 없음
* **유효 CSS 클래스**: `.cmp-teaser--promo`

#### 프로모션 기본

![기본 프로모션](assets/promo-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **표시 스타일**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효 CSS 클래스**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### 프로모션 보조

![프로모션 보조](assets/promo-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **표시 스타일**
   * 스타일 이름: **노랑**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효 CSS 클래스**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### 프로모션 오른쪽 정렬 스타일 {#promo-r-align}

다음 **프로모션 오른쪽 정렬** 레이아웃 스타일은 이미지와 텍스트(오른쪽 이미지, 왼쪽 텍스트) 위치를 뒤집는 프로모션 스타일의 변형입니다.

오른쪽 정렬은 그 핵심에서 디스플레이 스타일이며 프로모션 레이아웃 스타일과 함께 선택된 디스플레이 스타일로 AEM 스타일 시스템에 입력할 수 있습니다. 이는 의 모범 사례를 위반합니다.

**효과가 있는 스타일 조합만 표시합니다.**

..에서 이미 위반되었습니다. [기본 스타일](#default-style).

오른쪽 정렬은 프로모션 레이아웃 스타일에만 영향을 주고, 다른 2개의 레이아웃 스타일인 기본값 및 영웅에는 영향을 주지 않으므로 프로모션 레이아웃 스타일 콘텐츠를 오른쪽 정렬하는 CSS 클래스를 포함하는 새 레이아웃 스타일 프로모션(오른쪽 정렬)을 만들 수 있습니다. `cmp -teaser--alternate`.

여러 스타일을 단일 스타일 항목으로 조합하면 사용 가능한 스타일과 스타일 순열의 수를 줄이는 데 도움이 될 수도 있으며, 이를 최소화하는 것이 가장 좋습니다.

CSS 클래스의 이름, `cmp-teaser--alternate`는 작성자에게 친숙한 &quot;오른쪽 정렬&quot;의 명명법과 일치하지 않아도 됩니다.

#### 프로모션 오른쪽 정렬 기본값

![프로모션 오른쪽 정렬](assets/promo-alternate-default.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **표시 스타일**
   * 없음
* **유효 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### 프로모션 오른쪽 정렬 기본

![프로모션 오른쪽 정렬 기본](assets/promo-alternate-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **표시 스타일**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 프로모션 오른쪽 정렬 보조

![프로모션 오른쪽 정렬 보조](assets/promo-alternate-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **표시 스타일**
   * 스타일 이름: **노랑**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효 CSS 클래스**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 영웅 스타일 {#hero-style}

Hero 레이아웃 스타일은 제목 및 링크가 오버레이된 구성 요소의 이미지를 배경으로 표시합니다. Promo 레이아웃 스타일과 같은 Hero 레이아웃 스타일은 브랜드 색상으로 구분되어야 합니다.

Hero 레이아웃 스타일에 브랜드 색상을 지정하려면 프로모션 레이아웃 스타일에 사용된 것과 동일한 디스플레이 스타일을 활용할 수 있습니다.

구성 요소별로 스타일 이름은 단일 CSS 클래스 세트에 매핑됩니다. 즉, 프로모션 레이아웃 스타일의 배경을 색칠하는 CSS 클래스 이름은 히어로 레이아웃 스타일의 텍스트와 링크를 색칠해야 합니다.

이는 CSS 규칙의 범위를 지정함으로써 가볍게 달성할 수 있지만, 이렇게 하려면 CSS 개발자가 이러한 순열이 AEM에서 어떻게 제정되는지 이해해야 합니다.

배경에 색을 칠하기 위한 CSS **홍보** 기본(녹색) 색상을 사용한 레이아웃 스타일:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

텍스트 색상을 지정하는 CSS **영웅** 기본(녹색) 색상을 사용한 레이아웃 스타일:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero 기본값

![영웅 스타일](assets/hero.png)

* **레이아웃 스타일**
   * 스타일 이름: **영웅**
   * CSS 클래스: `cmp-teaser--hero`
* **표시 스타일**
   * 없음
* **유효 CSS 클래스**: `.cmp-teaser--hero`

#### 영웅 예비선거

![영웅 예비선거](assets/hero-primary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션**
   * CSS 클래스: `cmp-teaser--hero`
* **표시 스타일**
   * 스타일 이름: **녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효 CSS 클래스**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### 영웅 보조

![영웅 보조](assets/hero-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름: **프로모션**
   * CSS 클래스: `cmp-teaser--hero`
* **표시 스타일**
   * 스타일 이름: **노랑**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효 CSS 클래스**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(Block Element Modifier) 설명서 웹 사이트](https://getbem.com/)
* [LESS 설명서 웹사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
