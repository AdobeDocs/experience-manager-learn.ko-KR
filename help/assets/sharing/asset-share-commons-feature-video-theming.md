---
title: Asset Share Commons에서 테마 소개
description: Assets Share Commons에 대한 기능 및 기술 이해 자료
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
doc-type: Tutorial
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
duration: 734
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 1%

---

# Asset Share Commons에서 테마 소개 {#asset-share-commons-theme}

Asset Share Commons에서 간단한 소개입니다. 이 비디오는 사용자 정의 색상 구성표로 새 테마를 만드는 과정을 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

이 비디오에서는 Asset Share Commons Dark 테마를 기반으로 새 테마를 만듭니다. 색상 구성표는 사이트에 일관된 모양과 느낌을 제공하기 위해 사용자 정의 로고와 일치합니다.

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

[사용자 지정 클라이언트 라이브러리 테마](assets/asc-theme-demo.zip) 다운로드

## 추가 리소스{#additional-resources}

* [Asset Share Commons 릴리스 다운로드](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ 릴리스 다운로드](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
