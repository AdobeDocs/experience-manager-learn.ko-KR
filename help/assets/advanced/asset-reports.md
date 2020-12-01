---
title: AEM Assets의 자산 보고서
description: 'AEM Assets은 직관적인 사용자 경험을 통해 대규모 리포지토리에 맞게 확장되는 엔터프라이즈급 보고 프레임워크를 제공합니다. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# 자산 보고서{#using-reports-in-aem-assets}

AEM Assets은 직관적인 사용자 경험을 통해 대규모 리포지토리에 맞게 확장되는 엔터프라이즈급 보고 프레임워크를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excel 공식 {#excel-formulas}

다음 수식은 비디오에서 Microsoft Excel에서 크기별 차트를 생성하는 데 사용됩니다.

### 에셋 크기 표준화 - 바이트 {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### 크기별 자산 수 {#asset-count-by-size}

#### 200KB 미만 {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200KB ~ 500KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### 500KB 초과 {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## 추가 리소스{#additional-resources}

[차트가 있는 모든 자산 Excel 파일 다운로드](./assets/asset-reports/all-assets.xlsx)
