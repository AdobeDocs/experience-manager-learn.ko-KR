---
title: Asset Share Commons의 소개
description: Assets Share Commons의 기능 및 기술 이해를 위한 자료
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 3%

---


# Asset Share Commons {#asset-share-commons-theme} 의 소개

Asset Share Commons에서 이러한 내용을 간략하게 소개합니다. 이 비디오는 사용자 지정 색상 구성표를 사용하여 새 테마를 만드는 과정을 거칩니다.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

이 비디오에서는 Asset Share Commons Dark 테마를 기반으로 새 테마가 만들어집니다. 색상 구성표는 사용자 지정 로고와 일치하여 사이트에 일관된 모양과 느낌을 줍니다.

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

[사용자 지정 클라이언트 라이브러리 테마 다운로드](assets/asc-theme-demo.zip)

## 추가 리소스{#additional-resources}

* [Asset Share Commons 릴리스 다운로드](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ 릴리스 다운로드](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)