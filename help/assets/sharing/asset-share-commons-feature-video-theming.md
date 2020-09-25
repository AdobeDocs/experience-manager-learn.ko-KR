---
title: 자산 공유 공유지의 주제 소개
seo-title: 자산 공유 공유지의 주제 소개
description: 자산 공유를 위한 기능 및 기술적 이해를 위한 자료
seo-description: 자산 공유를 위한 기능 및 기술적 이해를 위한 자료
uuid: 5991a015-392a-4bb5-8332-192681505b07
discoiquuid: 08a5a394-c62b-4748-b303-33117f283612
contentOwner: dgonzale
feature: asset-share, brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 1%

---


# 자산 공유 공유지의 주제 소개 {#asset-share-commons-theme}

에셋 공유 공유에서 간략한 소개 이 비디오는 사용자 지정 색상 구성표를 사용하여 새 테마를 만드는 과정을 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

이 비디오에서는 자산 공유 어두운 테마를 기반으로 새로운 테마가 만들어집니다. 색상 구성표는 사용자 정의 로고와 일치하여 사이트에 일관된 모양과 느낌을 줄 수 있습니다.

## 테마 변수

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

사용자 [정의 클라이언트 라이브러리 테마 다운로드](assets/asc-theme-demo.zip)

## 추가 리소스{#additional-resources}

* [자산 공유 공유기 릴리스 다운로드](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ 릴리스 다운로드](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)