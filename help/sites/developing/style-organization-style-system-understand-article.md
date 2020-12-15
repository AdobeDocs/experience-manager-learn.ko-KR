---
title: AEM Sites을 사용한 스타일 시스템 우수 사례 이해
description: Adobe Experience Manager Sites에서 스타일 시스템 구현 시 모범 사례를 설명하는 자세한 문서입니다.
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1538'
ht-degree: 2%

---


# 스타일 시스템 우수 사례 이해{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>AEM Style System에서 사용하는 BEM 유사 규칙을 이해하려면 [Understanding how to code for the Style System](style-system-technical-video-understand.md) 의 내용을 검토하십시오.

AEM Style System에 대해 구현되는 두 가지 주요 버전 또는 스타일이 있습니다.

* **레이아웃 스타일**
* **스타일 표시**

**레이아웃** 스타일이면 구성 요소의 많은 요소에 영향을 주어 구성 요소의 정확한 정의 및 식별 가능한 변환(디자인 및 레이아웃)을 만들 수 있습니다. 이러한 변환 작업은 종종 특정 재사용 가능한 브랜드 개념에 맞게 진행됩니다. 예를 들어 티저 구성 요소는 이미지에 텍스트를 오버레이하는 영웅 레이아웃 또는 일반적인 카드 기반 레이아웃으로 표시할 수 있습니다.

**디스플레이** 스타일은 레이아웃 스타일의 사소한 변형에 영향을 주는 데 사용되지만 레이아웃 스타일의 기본 성격이나 의도는 변경되지 않습니다. 예를 들어 메인 레이아웃 스타일에는 기본 브랜드 색상 구성표에서 보조 브랜드 색상 구성표로 색상 구성표를 변경하는 표시 스타일이 있을 수 있습니다.

## 스타일 구성 우수 사례 {#style-organization-best-practices}

AEM 작성자가 사용할 수 있는 스타일 이름을 정의할 때 다음을 수행하는 것이 가장 좋습니다.

* 작성자가 알고 있는 단어를 사용하여 스타일 이름 지정
* 스타일 옵션 수 최소화
* 브랜드 표준에서 허용하는 스타일 옵션과 조합만 표시
* 효과가 있는 스타일 조합만 표시
   * 효과적인 조합이 노출되어 있는 경우 적어도 잘못된 효과가 없는지 확인하십시오.

AEM 작성자가 사용할 수 있는 가능한 스타일 조합의 수가 증가함에 따라 브랜드 표준에 대해 QAd가 되고 유효성이 검증되어야 하는 순차적 조합이 더 많이 존재합니다. 너무 많은 옵션을 사용하면 원하는 효과를 내기 위해 어떤 옵션이나 조합이 필요한지 명확하지 않을 수 있으므로 작성자를 혼동시킬 수도 있습니다.

### 스타일 이름과 CSS 클래스 {#style-names-vs-css-classes}

스타일 이름 또는 AEM 작성자에게 제공되는 옵션과 AEM에서 구현 CSS 클래스 이름이 분리됩니다.

이를 통해 스타일 옵션이 AEM 작성자가 명확하게 파악하고 명확하게 용어로 레이블이 지정되도록 할 수 있지만 CSS 개발자는 나중에 의미적이고 의미 있는 방식으로 CSS 클래스 이름을 지정할 수 있습니다. 예:

구성 요소에는 브랜드의 **기본** 및 **보조** 색상으로 색상이 지정되어야 하지만 AEM 작성자는 기본 및 보조 디자인 언어가 아닌 **녹색** 및 **노란색**&#x200B;으로 색상을 알고 있습니다.

AEM 스타일 시스템에서는 CSS 개발자가 `.cmp-component--primary-color` 및 `.cmp-component--secondary-color`의 의미 이름을 사용하여 CSS의 실제 스타일 구현을 정의할 수 있도록 하는 동시에 작성자 친화적인 레이블 **Green** 및 **Yellow**&#x200B;를 사용하여 이러한 색상 표시 스타일을 표시할 수 있습니다.

**녹색**&#x200B;의 스타일 이름이 `.cmp-component--primary-color`에 매핑되고 **노란색**&#x200B;이 `.cmp-component--secondary-color`에 매핑됩니다.

회사의 브랜드 색상이 향후 변경될 경우 변경해야 하는 모든 것은 `.cmp-component--primary-color` 및 `.cmp-component--secondary-color`의 단일 구현과 스타일 이름입니다.

## Teaser 구성 요소를 예로 들 수 있습니다. {#the-teaser-component-as-an-example-use-case}

다음은 Teaser 구성 요소의 스타일 지정 사용 사례로서 여러 가지 다른 레이아웃 및 표시 스타일을 갖습니다.

이렇게 하면 스타일 이름(작성자에게 노출)이 어떻게 구성되어 있는지, 그리고 백업 CSS 클래스가 어떻게 구성되어 있는지 살펴봅니다.

### Teaser 구성 요소 스타일 구성 {#component-styles-configuration}

다음 이미지는 사용 사례에 설명된 변형에 대한 티저 구성 요소에 대한 [!UICONTROL 스타일] 구성을 보여줍니다.

[!UICONTROL 스타일 그룹] 이름, 레이아웃 및 표시는 이 문서에서 스타일 유형을 개념적으로 분류하는 데 사용되는 표시 스타일 및 레이아웃 스타일의 일반적인 개념과 일치하여 사용합니다.

[!UICONTROL 스타일 그룹] 이름 및 [!UICONTROL 스타일 그룹]의 수는 구성 요소 사용 사례 및 프로젝트별 구성 요소 스타일 지정 규칙에 맞게 조정되어야 합니다.

예를 들어 **표시** 스타일 그룹 이름의 이름은 **색상**&#x200B;이 될 수 있습니다.

![스타일 그룹 표시](assets/style-config.png)

### 스타일 선택 메뉴 {#style-selection-menu}

아래 이미지는 [!UICONTROL 스타일] 메뉴 작성자가 상호 작용하여 구성 요소에 적합한 스타일을 선택하도록 표시합니다. 스타일 이름과 [!UICONTROL 스타일 그래프] 이름은 모두 작성자에게 표시됩니다.

![스타일 드롭다운 메뉴](assets/style-menu.png)

### 기본 스타일 {#default-style}

기본 스타일은 종종 구성 요소의 가장 일반적으로 사용되는 스타일이며 페이지에 추가할 때 티저에 대한 스타일이 지정되지 않은 기본 보기입니다.

기본 스타일의 공통성에 따라 CSS는 `.cmp-teaser`(수정자 없음) 또는 `.cmp-teaser--default`에 직접 적용될 수 있습니다.

기본 스타일 규칙이 모든 변형에 적용되지 않는 것보다 더 자주 적용되는 경우, 모든 변형을 암시적으로 상속해야 하므로 모든 변형을 BEM과 같은 규칙이 수행된다고 가정할 때 기본 스타일 규칙이 `.cmp-teaser`을 기본 스타일의 CSS 클래스로 사용하는 것이 가장 좋습니다. 그렇지 않은 경우 `.cmp-teaser--default`과 같은 기본 수정자를 통해 적용해야 합니다. 이러한 수정자는 구성 요소의 스타일 구성의 기본 CSS 클래스](#component-styles-configuration) 필드에 추가해야 합니다. 그렇지 않은 경우 이러한 스타일 규칙은 각 변형에서 무시되어야 합니다.[

&quot;named&quot; 스타일을 기본 스타일로 할당할 수도 있지만(예: 아래에 정의된 Hero 스타일 `(.cmp-teaser--hero)`), `.cmp-teaser` 또는 `.cmp-teaser--default` CSS 클래스 구현에 대해 기본 스타일을 구현하는 것이 더 명확합니다.

>[!NOTE]
>
>기본 레이아웃 스타일에는 표시 스타일 이름이 없지만 작성자는 AEM 스타일 시스템 선택 도구에서 표시 옵션을 선택할 수 있습니다.
>
>우수 사례를 위반하는 경우:
>
>**효과가 있는 스타일 조합만 표시**
>
>작성자가 **녹색**&#x200B;의 표시 스타일을 선택하면 아무 일도 일어나지 않습니다.
>
>이 경우 다른 모든 레이아웃 스타일은 브랜드 색상을 사용하여 색상을 적용해야 하므로 이 위반 사실을 시인합니다.
>
>아래의 **판촉 행사(오른쪽 정렬)** 섹션에서 원하지 않는 스타일 조합을 방지하는 방법을 확인할 수 있습니다.

![기본 스타일](assets/default.png)

* **레이아웃 스타일**
   * 기본값
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**: `.cmp-teaser--promo` or  `.cmp-teaser--default`

### 프로모션 스타일 {#promo-style}

**판촉 행사 레이아웃 스타일**&#x200B;은 사이트에서 고부가가치 컨텐츠를 홍보하는 데 사용되며 가로로 배치하여 웹 페이지에서 공간 밴드를 구성하고 브랜드 색상으로 스타일을 지정할 수 있어야 하며 검정색 텍스트를 사용하는 기본 판촉 행사 레이아웃 스타일입니다.

이를 위해서는 **Promo**&#x200B;의 **레이아웃 스타일** 및 **Green** 및 **Yellow**&#x200B;의 **표시 스타일이 Teaser 구성 요소의 AEM 스타일 시스템에 구성됩니다.**

#### 프로모션 기본값

![프로모션 기본값](assets/promo-default.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**:  `.cmp-teaser--promo`

#### 프로모션 기본

![예비](assets/promo-primary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 스타일 이름:**녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### 프로모션 보조

![프로모션 보조](assets/promo-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션**
   * CSS 클래스: `cmp-teaser--promo`
* **스타일 표시**
   * 스타일 이름:**노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### 프로모션 오른쪽 정렬 스타일 {#promo-r-align}

**판촉 행사 오른쪽 정렬** 레이아웃 스타일은 이미지 및 텍스트(오른쪽 이미지, 왼쪽 텍스트)의 위치를 변형하는 판촉 행사 스타일의 변형입니다.

오른쪽에 있는 올바른 정렬은 표시 스타일이므로 AEM 스타일 시스템에 Promo 레이아웃 스타일과 함께 선택한 표시 스타일로 입력할 수 있습니다. 이는 다음 우수 사례를 위반하는 것입니다.

**효과가 있는 스타일 조합만 표시**

...[기본 스타일](#default-style)에서 이미 위반되었습니다.

올바른 정렬은 프로모션 레이아웃 스타일만 영향을 주고 다른 2개의 레이아웃 스타일에는 영향을 주지 않습니다.기본 및 hero의 경우 프로모션 레이아웃 스타일 컨텐츠를 바로 정렬하는 CSS 클래스가 포함된 새 레이아웃 스타일 프로모션(오른쪽 정렬)을 만들 수 있습니다.`cmp -teaser--alternate`.

여러 스타일을 하나의 스타일 항목으로 결합하면 사용 가능한 스타일 및 스타일 순열의 수를 줄이는 데 도움이 되므로 이를 최소화합니다.

CSS 클래스의 이름 `cmp-teaser--alternate`은 &quot;right aligned&quot;의 작성자와 친숙한 명명식과 일치하지 않아도 됩니다.

#### 판촉 행사 오른쪽 정렬 기본값

![오른쪽 정렬](assets/promo-alternate-default.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### 프로모션 오른쪽 정렬 기본

![프로모션 오른쪽 정렬 기본](assets/promo-alternate-primary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 스타일 이름:**녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 프로모션 오른쪽 정렬 보조

![프로모션 오른쪽 정렬 보조](assets/promo-alternate-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션(오른쪽 정렬)**
   * CSS 클래스: `cmp-teaser--promo cmp-teaser--alternate`
* **스타일 표시**
   * 스타일 이름:**노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 영웅 스타일 {#hero-style}

메인 레이아웃 스타일에는 구성 요소의 이미지가 제목 및 링크가 오버레이된 배경으로 표시됩니다. Promo 레이아웃 스타일과 마찬가지로 Hero 레이아웃 스타일에는 브랜드 색상으로 색상을 적용할 수 있어야 합니다.

메인 레이아웃 스타일을 브랜드 색상으로 칠하기 위해 프로모션 레이아웃 스타일에 사용된 것과 동일한 표시 스타일을 활용할 수 있습니다.

구성 요소별로 스타일 이름이 단일 CSS 클래스 집합에 매핑됩니다. 즉, 프로모션 레이아웃 스타일의 배경을 색상으로 지정하는 CSS 클래스 이름은 Hero 레이아웃 스타일의 텍스트와 링크에 색상을 지정해야 합니다.

CSS 규칙을 스코핑하여 세밀하게 수정할 수 있지만 CSS 개발자는 이러한 순차적 변형이 AEM에서 어떻게 제정되는지 이해해야 합니다.

기본(녹색) 색상으로 **Promote** 레이아웃 스타일의 배경을 색상화하기 위한 CSS:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

기본(녹색) 색상으로 **Hero** 레이아웃 스타일의 텍스트를 색상 지정하기 위한 CSS:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### 기본 영웅

![영웅 스타일](assets/hero.png)

* **레이아웃 스타일**
   * 스타일 이름:**Hero**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 없음
* **유효한 CSS 클래스**:  `.cmp-teaser--hero`

#### 영웅 예비

![영웅 예비](assets/hero-primary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 스타일 이름:**녹색**
   * CSS 클래스: `cmp-teaser--primary-color`
* **유효한 CSS 클래스**:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secondary

![Hero Secondary](assets/hero-secondary.png)

* **레이아웃 스타일**
   * 스타일 이름:**프로모션**
   * CSS 클래스: `cmp-teaser--hero`
* **스타일 표시**
   * 스타일 이름:**노란색**
   * CSS 클래스: `cmp-teaser--secondary-color`
* **유효한 CSS 클래스**:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(블록 요소 수정자) 설명서 웹 사이트](https://getbem.com/)
* [LESS 설명서 웹 사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
