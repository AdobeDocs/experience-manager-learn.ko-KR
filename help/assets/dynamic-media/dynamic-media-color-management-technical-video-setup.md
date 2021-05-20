---
title: AEM Dynamic Media을 사용한 색상 관리 이해
description: 이 비디오에서는 Dynamic Media 색상 관리를 살펴보고 AEM Assets용 의 색상 수정 미리 보기 기능을 제공하는 데 사용할 수 있는 방법을 알아봅니다.
sub-product: dynamic-media
feature: 이미지 프로필, 비디오 프로필
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 15%

---


# AEM Dynamic Media을 사용한 색상 관리 이해{#understanding-color-management-with-aem-dynamic-media}

이 비디오에서는 Dynamic Media 색상 관리를 살펴보고 AEM Assets용 의 색상 수정 미리 보기 기능을 제공하는 데 사용할 수 있는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Dynamic Media ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) AEM을 활성화하여 이 기능을 사용합니다.

이 기능은 AEM 6.1 및 6.2 버전에서 기능 팩으로 사용할 수 있습니다.

## 색상 관리 구성 노드 {#xml-template-for-the-color-management-configuration-node}에 대한 XML 템플릿

다음은 색상 관리 구성 노드의 XML 템플릿입니다. 이 XML 템플릿은 AEM 개발 프로젝트에 복사되며 프로젝트에 적합한 구성으로 구성할 수 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### 기본 Adobe 색상 프로필 목록은 {#list-of-default-adobe-color-profiles-are-listed-below} 아래에 나와 있습니다

| 이름 | 색상 공간 | 설명 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Coated FOGRA39(ISO 12647-2:2004) |
| 코팅그래콜 | CMYK | 코팅 GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| 유럽 ISOCoated | CMYK | 유럽 ISO 코팅 FOGRA27 |
| 유로스케일 코팅 | CMYK | 유로스케일 코팅 v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | 일본 컬러 2001 코팅 |
| JapanColor신문 | CMYK | 일본 컬러 2002 신문 |
| JapanColorUncoated | CMYK | 일본 컬러 2001 미코팅 |
| JapanColorWebCoated | CMYK | 일본 컬러 2003 웹 코팅 |
| JapanWebCoated | CMYK | 일본 웹 코팅(광고) |
| NewslettrintSNAP2007 | CMYK | 미국 신문(SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop 4 기본 CMYK |
| PS5Default | CMYK | Photoshop 5 기본 CMYK |
| SheetfedCoated | CMYK | 미국 Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | 미국 판금 미코팅 v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 코팅되지 않은 FOGRA29(ISO 12647-2:2004) |
| WebCoated | CMYK | 미국 웹 코팅(SWOP) v2 |
| WebCoatedFogra28 | CMYK | 웹 코팅 FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | 웹 코팅 SWOP 2006년 3등급 용지 |
| WebCoatedGrade5 | CMYK | 웹 코팅 SWOP 2006년 5등급 용지 |
| WebUncoated | CMYK | 미국 웹 코팅되지 않은 v2 |
| 광역 색상 영역RGB | RGB | 광역 RGB |

## 추가 리소스{#additional-resources}

* [Dynamic Media 색상 관리 구성](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
