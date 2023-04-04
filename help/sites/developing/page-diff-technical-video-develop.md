---
title: AEM Sites의 페이지 차이를 위한 개발
description: 이 비디오에서는 AEM Sites의 페이지 차이 기능에 대한 사용자 지정 스타일을 제공하는 방법을 보여줍니다.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 4%

---

# 페이지 차이를 위한 개발 {#developing-for-page-difference}

이 비디오에서는 AEM Sites의 페이지 차이 기능에 대한 사용자 지정 스타일을 제공하는 방법을 보여줍니다.

## 페이지 차이 스타일 사용자 지정 {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>이 비디오에서는 사용자 지정 CSS를 we.Retail 클라이언트 라이브러리에 추가합니다. 여기서 customizer의 AEM Sites 프로젝트를 변경해야 합니다. 아래 예제 코드에서: `my-project`.

AEM 페이지 차이는 `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

클라이언트 라이브러리 카테고리를 사용하지 않고 CSS의 직접 로드 때문에 사용자 지정 스타일에 대한 다른 삽입 지점을 찾아야 하며 이 사용자 지정 삽입 지점은 프로젝트의 authoring clientlib입니다.

이렇게 하면 이러한 사용자 지정 스타일 무시를 임차인별로 지정할 수 있는 이점이 있습니다.

### 작성 clientlib 준비 {#prepare-the-authoring-clientlib}

의 존재 확인 `authoring` clientlib for project에서 사용할 수 있습니다. `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 사용자 지정 CSS 제공 {#provide-the-custom-css}

프로젝트의 `authoring` clientlib `css.txt` 재정의를 수행하는 스타일을 제공하는 파일이 더 적습니다. [Less](https://lesscss.org/) 는 이 예에서 활용되는 클래스 래핑 등 여러 가지 편리한 기능으로 인해 선호됩니다.

```shell
base=./css

htmldiff.less
```

만들기 `less` 스타일 재정의가 포함된 파일 `/apps/my-project/clientlibs/authoring/css/htmldiff.less`를 채울 수 있으며 필요한 경우 오버링 스타일을 제공합니다.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### 페이지 구성 요소를 통해 작성 clientlib CSS 포함 {#include-the-authoring-clientlib-css-via-the-page-component}

프로젝트의 기본 페이지의 `/apps/my-project/components/structure/page/customheaderlibs.html` 바로 앞에 `</head>` 태그에 다음 코드를 배치하여 스타일이 로드되도록 합니다.

이러한 스타일은 [!UICONTROL 편집] 및 [!UICONTROL 미리 보기] WCM 모드.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

위의 스타일이 적용된 차이 페이지의 최종 결과는 다음과 같습니다(HTML이 추가되고 구성 요소가 변경됨).

![페이지 차이](assets/page-diff.png)

## 추가 리소스 {#additional-resources}

* [we.Retail 샘플 사이트 다운로드](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEM 클라이언트 라이브러리 사용](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Less CSS 설명서](https://lesscss.org/)
