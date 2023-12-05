---
title: AEM Dynamic Media을 통한 색상 관리 이해
description: 이 비디오에서는 Dynamic Media 색상 관리 및 AEM Assets용에서 색상 교정 미리 보기 기능을 제공하는 데 사용할 수 있는 방법에 대해 알아봅니다.
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 343
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 11%

---

# AEM Dynamic Media을 통한 색상 관리 이해{#understanding-color-management-with-aem-dynamic-media}

이 비디오에서는 Dynamic Media 색상 관리 및 AEM Assets용에서 색상 교정 미리 보기 기능을 제공하는 데 사용할 수 있는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[Dynamic Media 활성화](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) 이 기능을 사용하려면 AEM에서 을 참조하십시오.

이 기능은 AEM 6.1 및 6.2 버전에서 기능 팩으로 사용할 수 있습니다.

## 색상 관리 구성 노드용 XML 템플릿 {#xml-template-for-the-color-management-configuration-node}

다음은 색상 관리 구성 노드에 대한 XML 템플릿입니다. 이 XML 템플릿은 AEM 개발 프로젝트에 복사할 수 있으며 프로젝트에 적합한 구성으로 구성할 수 있습니다.

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

### 기본 Adobe 색상 프로파일 목록은 아래에 나와 있습니다 {#list-of-default-adobe-color-profiles-are-listed-below}

| 이름 | 색상 공간 | 설명 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| 애플RGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | 코팅 FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | 코팅 FOGRA39(ISO 12647-2:2004) |
| CoatedGraaCol | CMYK | 코팅 GRACoL 2006 (ISO 12647-2:2004) |
| 색상 일치RGB | RGB | ColorMatch RGB |
| 유럽 ISOC 기반 | CMYK | 유럽 ISO 코팅 FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoating | CMYK | Japan Color 2001 코팅 |
| 일본 컬러 신문 | CMYK | 일본 색상 2002 신문 |
| JapanColorUncoating | CMYK | Japan Color 2001 무코팅 |
| JapanColorWebCoating | CMYK | Japan Color 2003 웹 코팅 |
| JapanWebCoated | CMYK | Japan Web Coated (Ad) |
| 신문 인쇄2007 | CMYK | 미국 신문용지(SNAP 2007) |
| NTSC | RGB | NTSC(1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop 4 기본 CMYK |
| PS5Default | CMYK | Photoshop 5 기본 CMYK |
| SheetfedCoating | CMYK | U.S. Sheetfed Coated v2 |
| SheetfedUncoating | CMYK | 미국 Sheetfed Uncoated v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 코팅되지 않은 FOGRA29(ISO 12647-2:2004) |
| 웹 코팅 | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | 웹 코팅 FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web Coated SWOP 2006 Grade 3 용지 |
| WebCoatedGrade5 | CMYK | 웹코팅 SWOP 2006 Grade 5 용지 |
| 웹 코팅되지 않음 | CMYK | U.S. Web Uncoated v2 |
| 광영역RGB | RGB | 광영역 RGB |

## 추가 리소스{#additional-resources}

* [Dynamic Media 색상 관리 구성](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
