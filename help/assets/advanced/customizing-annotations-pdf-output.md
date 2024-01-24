---
title: AEM Assets에서 주석 맞춤화
description: AEM 개발자가 PDF에 출력할 때의 AEM Assets 형식 및 스타일을 구성할 수 있습니다.
feature: Collaboration
version: 6.4, 6.5
topic: Collaboration
role: Developer
level: Intermediate
last-substantial-update: 2022-06-03T00:00:00Z
doc-type: Feature Video
exl-id: 972737dd-8ca6-47b4-a4ec-b73355c09cec
duration: 15
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '60'
ht-degree: 0%

---

# AEM Assets에서 주석 맞춤화{#using-annotations-in-aem-assets}

AEM은 PDF 출력의 사용자 지정을 지원합니다.

## PDF 주석 sling:OsgiConfig 정의

PDF 주석을 사용자 정의하려면 **sling:OsgiConfig** 아래에 있는 AEM 프로젝트의 노드

`/apps/my-project/config.author/com.day.cq.dam.core.impl.annotation.pdf.AnnotationPdfConfig.xml` 필요에 따라 값을 조정합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"

    cq.dam.config.annotation.pdf.document.padding.vertical="{Long}20"
    cq.dam.config.annotation.pdf.reviewStatus.color.changesRequested="fad269"
    cq.dam.config.annotation.pdf.document.height="{Long}792"
    cq.dam.config.annotation.pdf.document.width="{Long}612"
    cq.dam.config.annotation.pdf.reviewStatus.width="{Long}150"
    cq.dam.config.annotation.pdf.font.size="{Long}7"
    cq.dam.config.annotation.pdf.font.color="3B3B3B"
    cq.dam.config.annotation.pdf.font.light="A4A4A4"
    cq.dam.config.annotation.pdf.reviewStatus.color.rejected="fa7d73"
    cq.dam.config.annotation.pdf.minImageHeight="{Long}100"
    cq.dam.config.annotation.pdf.reviewStatus.color.approved="009933"
    cq.dam.config.annotation.pdf.marginTextImage="{Long}14"
    cq.dam.config.annotation.pdf.document.padding.horizontal="{Long}20"
    cq.dam.config.annotation.pdf.annotationMarker.width="{Long}5"
    />
```
