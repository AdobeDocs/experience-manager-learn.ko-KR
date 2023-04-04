---
title: Adobe Experience Manager Sites의 구성 요소 아이콘 사용자 지정
description: 구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약자로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 보다 빠르게 작성하는 데 필요한 구성 요소를 찾을 수 있습니다.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# 구성 요소 아이콘 사용자 지정 {#developing-component-icons-in-aem-sites}

구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약자로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 보다 빠르게 작성하는 데 필요한 구성 요소를 찾을 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

이제 구성 요소 브라우저가 일관된 회색 테마에 표시되어 다음을 표시합니다.

* **[!UICONTROL 구성 요소 그룹]**
* **[!UICONTROL 구성 요소 제목]**
* **[!UICONTROL 구성 요소 설명]**
* **[!UICONTROL 구성 요소 아이콘]**
   * 구성 요소 제목의 처음 두 문자 *(기본값)*
   * 사용자 지정 PNG 이미지 *(개발자가 구성)*
   * 사용자 지정 SVG 이미지 *(개발자가 구성)*
   * CoralUI 아이콘 *(개발자가 구성)*

## 구성 요소 아이콘 구성 옵션 {#component-icon-configuration-options}

### 약자 {#abbreviations}

기본적으로 구성 요소 제목(**[cq:구성 요소]@jcr:title**)은 약어로 사용됩니다. 예를 들어 **[cq:구성 요소]@jcr:title=문서 목록** 약어는 &quot;**Ar**&quot;.

약어는 **[cq:구성 요소]@abbreviation** 속성을 사용합니다. 이 값은 2자 이상의 문자를 사용할 수 있지만 시각적 외란을 방지하기 위해 약어를 2자로 제한하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI 아이콘 {#coralui-icons}

AEM에서 제공하는 CoralUI 아이콘은 구성 요소 아이콘에 사용할 수 있습니다. CoralUI 아이콘을 구성하려면 **[cq:구성 요소]@cq:icon** 원하는 CoralUI 아이콘의 HTML 아이콘 속성 값(다음에 열거됨)에 대한 속성 [CoralUI 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 이미지 {#png-images}

구성 요소 아이콘에 PNG 이미지를 사용할 수 있습니다. PNG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 이미지를 **nt:file** 명명된 이름 **cq:icon.png** 아래에 **[cq:구성 요소]**.

PNG의 배경색은 투명 또는 배경색이 **#707070**.

PNG 이미지의 크기가 **20px x x 20px**. 그러나 망막 디스플레이를 수용하려면 **40px** by **40px** 더 좋을 것 같습니다

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG 이미지 {#svg-images}

SVG 이미지(벡터 기반)는 구성 요소 아이콘에 사용할 수 있습니다. SVG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 SVG을 **nt:file** 명명된 이름 **cq:icon.svg** 아래에 **[cq:구성 요소]**.

SVG 이미지에는 배경색이 **#707070** 그리고 **20px x x 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 추가 리소스 {#additional-resources}

* [사용 가능한 CoralUI 아이콘](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
