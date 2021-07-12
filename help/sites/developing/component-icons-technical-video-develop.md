---
title: Adobe Experience Manager Sites의 구성 요소 아이콘 사용자 지정
description: 구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약자로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 보다 빠르게 작성하는 데 필요한 구성 요소를 찾을 수 있습니다.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: 코어 구성 요소
topic: 개발
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---


# 구성 요소 아이콘 사용자 지정 {#developing-component-icons-in-aem-sites}

구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약자로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 보다 빠르게 작성하는 데 필요한 구성 요소를 찾을 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

이제 구성 요소 브라우저가 일관된 회색 테마에 표시되어 다음을 표시합니다.

* **[!UICONTROL 구성 요소 그룹]**
* **[!UICONTROL 구성 요소 제목]**
* **[!UICONTROL 구성 요소 설명]**
* **[!UICONTROL 구성 요소 아이콘]**
   * 구성 요소 제목 *(기본값)*&#x200B;의 처음 두 문자
   * 사용자 지정 PNG 이미지 *(개발자가 구성함)*
   * 사용자 지정 SVG 이미지 *(개발자가 구성)*
   * CoralUI 아이콘 *(개발자가 구성함)*

## 구성 요소 아이콘 구성 옵션 {#component-icon-configuration-options}

### 약자 {#abbreviations}

기본적으로 구성 요소 제목(**[cq:Component]@jcr:title**)의 처음 2자가 약어로 사용됩니다. 예를 들어 **[cq:Component]@jcr:title=Article List** 약어는 &quot;**Ar**&quot;로 표시됩니다.

약어는 **[cq:Component]@abbreviation** 속성을 통해 사용자 지정할 수 있습니다. 이 값은 2자 이상의 문자를 사용할 수 있지만 시각적 외란을 방지하기 위해 약어를 2자로 제한하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI 아이콘 {#coralui-icons}

AEM에서 제공하는 CoralUI 아이콘은 구성 요소 아이콘에 사용할 수 있습니다. CoralUI 아이콘을 구성하려면 **[cq:Component]@cq:icon** 속성을 원하는 CoralUI 아이콘의 HTML 아이콘 속성 값([CoralUI 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)에 열거됨)으로 설정합니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 이미지 {#png-images}

구성 요소 아이콘에 PNG 이미지를 사용할 수 있습니다. PNG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 이미지를 **[cq:Component]** 아래에 **nt:file** cq:icon.png **로 추가합니다.**

PNG의 배경색은 투명 또는 배경색이 **#707070**&#x200B;로 설정되어 있어야 합니다.

PNG 이미지의 크기가 **20px 20px**&#x200B;로 조정됩니다. 그러나 Retina를 수용하려면 **40px**&#x200B;에 의한 **40px**&#x200B;을 표시하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG 이미지 {#svg-images}

SVG 이미지(벡터 기반)는 구성 요소 아이콘에 사용할 수 있습니다. SVG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 SVG를 **[cq:Component]** 아래에 **nt:file** cq:icon.svg **로 추가합니다.**

SVG 이미지에는 배경색이 **#707070**&#x200B;로 설정되고 크기가 **20px x x x**&#x200B;이어야 합니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 추가 리소스 {#additional-resources}

* [사용 가능한 CoralUI 아이콘](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
