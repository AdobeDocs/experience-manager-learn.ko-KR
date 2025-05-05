---
title: Adobe Experience Manager Sites에서 구성 요소 아이콘 맞춤화
description: 구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약어로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 더 빠르게 빌드하는 데 필요한 구성 요소를 찾을 수 있습니다.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 구성 요소 아이콘 사용자 정의 {#developing-component-icons-in-aem-sites}

구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약어로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 더 빠르게 빌드하는 데 필요한 구성 요소를 찾을 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/33677?quality=12&learn=on&captions=kor)

이제 구성 요소 브라우저가 일관된 회색 테마로 표시되어 다음을 표시합니다.

* **[!UICONTROL 구성 요소 그룹]**
* **[!UICONTROL 구성 요소 제목]**
* **[!UICONTROL 구성 요소 설명]**
* **[!UICONTROL 구성 요소 아이콘]**
   * 구성 요소 제목 *(기본값)*&#x200B;의 처음 두 글자
   * 사용자 지정 PNG 이미지 *(개발자에 의해 구성됨)*
   * 사용자 지정 SVG 이미지 *(개발자에 의해 구성됨)*
   * CoralUI 아이콘 *(개발자가 구성)*

## 구성 요소 아이콘 구성 옵션 {#component-icon-configuration-options}

### 약자 {#abbreviations}

기본적으로 구성 요소 제목의 처음 두 문자(**[cq:Component]@jcr:title**)가 약어로 사용됩니다. 예를 들어 **[cq:Component]@jcr:title=Article List**&#x200B;인 경우 약어는 &quot;**Ar**&quot;로 표시됩니다.

**[cq:Component]@abbreviation** 속성을 통해 약어를 사용자 지정할 수 있습니다. 이 값은 2자 이상을 사용할 수 있지만 시각적 방해를 방지하려면 약어를 2자로 제한하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI 아이콘 {#coralui-icons}

AEM에서 제공하는 CoralUI 아이콘을 구성 요소 아이콘에 사용할 수 있습니다. CoralUI 아이콘을 구성하려면 **[cq:Component]@cq:icon** 속성을 원하는 CoralUI 아이콘의 HTML 아이콘 특성 값([CoralUI 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)에 열거됨)으로 설정합니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 이미지 {#png-images}

구성 요소 아이콘에 PNG 이미지를 사용할 수 있습니다. PNG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 이미지를 **[cq:Component]** 아래에 **cq:icon.png**(이)라는 **nt:file**(으)로 추가하십시오.

PNG에는 투명 배경이나 배경색이 **#707070**(으)로 설정되어 있어야 합니다.

PNG 이미지의 크기가 **20px, 20px**(으)로 조정됩니다. 그러나 Retina 디스플레이 **40px** x **40px**&#x200B;을(를) 수용하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG 이미지 {#svg-images}

SVG 이미지(벡터 기반)는 구성 요소 아이콘에 사용할 수 있습니다. SVG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 SVG을 **[cq:Component]** 아래에 **cq:icon.svg**(이)라는 **nt:file**(으)로 추가하십시오.

SVG 이미지의 배경색은 **#707070**(으)로 설정되어야 하며 크기는 **20px x 20px입니다.**&#x200B;입니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 추가 리소스 {#additional-resources}

* [사용 가능한 CoralUI 아이콘](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
