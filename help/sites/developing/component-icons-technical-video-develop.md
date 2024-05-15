---
title: Adobe Experience Manager Sites에서 구성 요소 아이콘 맞춤화
description: 구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약어로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 더 빠르게 빌드하는 데 필요한 구성 요소를 찾을 수 있습니다.
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 구성 요소 아이콘 사용자 정의 {#developing-component-icons-in-aem-sites}

구성 요소 아이콘을 사용하면 작성자가 아이콘이나 의미 있는 약어로 구성 요소를 빠르게 식별할 수 있습니다. 이제 작성자가 웹 경험을 더 빠르게 빌드하는 데 필요한 구성 요소를 찾을 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

이제 구성 요소 브라우저가 일관된 회색 테마로 표시되어 다음을 표시합니다.

* **[!UICONTROL 구성 요소 그룹]**
* **[!UICONTROL 구성 요소 제목]**
* **[!UICONTROL 구성 요소 설명]**
* **[!UICONTROL 구성 요소 아이콘]**
   * 구성 요소 제목의 처음 두 글자 *(기본값)*
   * 사용자 정의 PNG 이미지 *(개발자에 의해 구성됨)*
   * 사용자 지정 SVG 이미지 *(개발자에 의해 구성됨)*
   * CoralUI 아이콘 *(개발자에 의해 구성됨)*

## 구성 요소 아이콘 구성 옵션 {#component-icon-configuration-options}

### 약자 {#abbreviations}

기본적으로 구성 요소 제목의 처음 2자(**[cq:Component]@jcr:title**) 를 약어로 사용합니다. 예를 들어 다음과 같습니다. **[cq:Component]@jcr:title=Article 목록** 약어는 &quot;로 표시됩니다&#x200B;**Ar**&quot;.

다음을 통해 약어를 사용자 지정할 수 있습니다. **[cq:Component]@abbreviation** 속성. 이 값은 2자 이상을 사용할 수 있지만 시각적 방해를 방지하려면 약어를 2자로 제한하는 것이 좋습니다.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI 아이콘 {#coralui-icons}

AEM에서 제공하는 CoralUI 아이콘을 구성 요소 아이콘에 사용할 수 있습니다. CoralUI 아이콘을 구성하려면 **[cq:Component]@cq:icon** 속성을 원하는 CoralUI 아이콘의 HTML 아이콘 속성 값에 추가합니다( [CoralUI 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 이미지 {#png-images}

구성 요소 아이콘에 PNG 이미지를 사용할 수 있습니다. PNG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 이미지를 (으)로 추가합니다 **nt:file** 명명된 **cq:icon.png** 다음 아래에 **[cq:Component]**.

PNG에는 투명한 배경이 있거나 배경색이 로 설정되어야 합니다. **#707070**.

PNG 이미지의 크기는 다음과 같이 조정됩니다. **20px x 20px**. 그러나 retina 디스플레이를 수용하려면 **40픽셀** 작성자: **40픽셀** 더 좋아질지도 모르죠

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG 이미지 {#svg-images}

SVG 이미지(벡터 기반)를 구성 요소 아이콘에 사용할 수 있습니다. SVG 이미지를 구성 요소 아이콘으로 구성하려면 원하는 SVG을 로 추가합니다. **nt:file** 명명된 **cq:icon.svg** 다음 아래에 **[cq:Component]**.

SVG 이미지의 배경색은 로 설정되어야 합니다. **#707070** 및 크기 **20px x 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 추가 리소스 {#additional-resources}

* [사용 가능한 CoralUI 아이콘](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
