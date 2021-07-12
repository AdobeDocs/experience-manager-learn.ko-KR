---
title: AEM Commerce 및 InDesign Server에서 자산 카탈로그 사용
description: AEM 6.4 카탈로그 개선에서는 AEM 자산 템플릿 및 InDesign Server을 사용하여 카탈로그 페이지를 만드는 기능을 제공합니다.  사용자는 InDesign 템플릿을 사용하여 카탈로그 페이지를 만들고 제품 속성을 편집 가능한 필드에 매핑할 수 있으며 나중에 다른 제품에 대한 유사한 페이지를 만드는 데 사용할 수 있습니다.
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---


# AEM Commerce 및 InDesign Server에서 자산 카탈로그 사용{#using-asset-catalog-with-aem-commerce-and-indesign-server}

AEM 6.4 카탈로그 개선에서는 AEM 자산 템플릿 및 InDesign Server을 사용하여 카탈로그 페이지를 만드는 기능을 제공합니다.  사용자는 InDesign 템플릿을 사용하여 카탈로그 페이지를 만들고 제품 속성을 편집 가능한 필드에 매핑할 수 있으며 나중에 다른 제품에 대한 유사한 페이지를 만드는 데 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22540/)

>[!NOTE]
>
>\.indd 파일을 AEM Assets에 업로드하기 전에 InDesign 서버가 실행되고 있어야 합니다.

* 크리에이티브 사용자는 InDesign 파일로 컨텐츠에 태그를 지정할 수 있습니다. 태그가 지정된 컨텐츠가 있는 InDesign 파일은 AEM Assets에 업로드할 때 편집 가능한 필드로 식별됩니다.
* 사용자는 \.indd 파일을 사용하여 카탈로그 페이지를 만들 수 있습니다. \.indd 파일 내에 있는 태그된 컨텐츠는 편집 가능한 필드로 사용할 수 있으므로 컨텐츠 작성자가 이러한 필드에 대한 컨텐츠를 수정할 수 있습니다.
* 필드 유형이 일치하는 경우 제품 속성을 편집 가능한 필드에 매핑할 수 있습니다.
* 유사한 제품에 대한 카탈로그 페이지를 쉽게 만들 수 있습니다.
* 다양한 카탈로그 페이지를 하나의 PDF 또는 \.indd 파일로 병합
