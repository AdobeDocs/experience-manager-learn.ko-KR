---
title: 이미지 사전 설정
description: Dynamic Media Classic의 이미지 사전 설정에는 이미지를 특정 크기, 형식, 품질 및 선명하게 하는 데 필요한 모든 설정이 포함되어 있습니다. 이미지 사전 설정은 다이내믹한 크기 지정의 주요 구성 요소입니다. Dynamic Media Classic에서 URL을 보면 이미지 사전 설정이 사용 중인지 쉽게 확인할 수 있습니다. 이미지 사전 설정, 이러한 사전 설정이 왜 유용한지, 만드는 방법에 대해 학습합니다.
sub-product: dynamic-media
feature: image-presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '705'
ht-degree: 1%

---


# 이미지 사전 설정 {#image-presets}

이미지 사전 설정은 특정 크기, 형식, 품질 및 선명하게 하기 영역에서 이미지를 만드는 데 필요한 모든 설정을 포함하는 레서피입니다. 이미지 사전 설정은 다이내믹한 크기 지정의 주요 구성 요소입니다.

Dynamic Media Classic 고객의 URL을 보면 사용 중인 이미지 사전 설정이 표시될 수 있습니다. URL의 끝에서 $name$(이름으로 대체되는 단어 또는 단어 포함)을 찾습니다.

이미지 사전 설정은 URL을 단축하므로 요청당 여러 개의 이미지 제공 지침을 작성하는 대신 단일 이미지 사전 설정을 작성할 수 있습니다. 예를 들어, 이 두 URL은 선명하게 하기 기능이 있는 동일한 300 x 300 JPEG 이미지를 생성하지만 두 번째 URL은 이미지 사전 설정을 사용합니다.

![이미지](assets/image-presets/image-preset-2.png)

The true value of Image Presets is that any Company Administrator can update the definition of that Image Preset and affect every image using that format — without changing any web code. URL에 대한 캐시가 지워진 후 이미지 사전 설정에 대한 변경 결과가 표시됩니다.

>[!IMPORTANT]
>
>이미지 크기를 조정할 때, 이미지 너비와 높이의 종횡비는 이미지가 왜곡되지 않도록 항상 비례적으로 유지되어야 합니다.

이미지 사전 설정에는 이름 양쪽에 달러 기호($)가 있고 물음표(?)를 따릅니다. 분리자.

>[!TIP]
>
>사이트에서 고유한 이미지 크기당 하나의 이미지 사전 설정을 만듭니다. 예를 들어 제품 세부 사항 페이지에 350 X 350 이미지, 검색 페이지에 120 X 120 이미지, 크로스셀링/주요 항목에 90 X 90 이미지가 필요한 경우 500개 이미지 또는 500,000개 이미지 중 세 개의 이미지 사전 설정이 필요합니다.

- [이미지 사전 설정](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html)에 대해 자세히 알아보십시오.
- [이미지 사전 설정](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)을 만드는 방법을 알아봅니다.

## 이미지 사전 설정 및 선명하게 하기

이미지 사전 설정은 일반적으로 이미지의 크기를 조정하며, 원래 크기로 이미지의 크기를 조정할 때마다 선명하게 하기 기능을 추가해야 합니다. 크기 조정 시 많은 픽셀이 병합되어 더 작은 공간에 혼합되므로 이미지가 부드럽고 흐릿하게 표시됩니다. 선명하게 하면 이미지의 가장자리 및 고대비 영역의 대비를 증가시킵니다.

Dynamic Media Classic에 업로드한 고해상도 이미지는 확대했을 때 전체 크기로 볼 때 선명하게 할 필요가 없습니다. 그러나 작은 크기라도 선명하게 하는 것이 대개 바람직합니다.

>[!TIP]
>
>이미지 크기 조정 시 항상 선명하게 하기! 즉, 모든 이미지 사전 설정(및 나중에 논의되는 뷰어 사전 설정)에 선명하게 하기를 추가해야 합니다.
>
>이미지가 좋지 않을 경우 선명하게 해야 하거나 품질이 좋지 않기 때문일 수 있습니다.

추가할 선명하게 하기 효과는 전적으로 주관적입니다. 어떤 사람들은 부드러운 이미지를 좋아하는 반면, 다른 사람들은 매우 날카롭습니다. 이미지에 선명하게 하기 필터 조합을 실행하여 이미지를 쉽게 향상시킬 수 있습니다. 그러나 이미지를 지나치게 선명하게 하거나

다음 그래픽은 선명하게 하기 세 가지 수준을 보여줍니다. 오른쪽에서 왼쪽으로 선명하게 하기, 정확한 양, 그리고 너무 많습니다.

![이미지](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic에서는 세 가지 유형의 선명하게 하기를 허용합니다.간단한 선명하게 하기, 리샘플링 모드 및 언샵 마스크

[Dynamic Media Classic Sharpening Options](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)에 대해 자세히 알아보십시오.

## 추가 리소스

[Image Preset Guide](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 이미지 품질 및 로드 속도에 맞게 최적화하는 데 사용할 설정입니다.

[이미지는 모든 부분 2입니다.품질과 속도](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/) 등 단순한 흐림 효과를 얻을 수 있습니다. 고품질의 빠른 로딩 이미지를 전달하기 위해 이미지 사전 설정을 사용하는 것에 대해 논의하는 블로그 게시물입니다.

[이미지는 웨비나를](https://dynamicmediaseries2019.enterprise.adobeevents.com/) 의미합니다. _이미지가 모든 것_ 시리즈에 있는 세 개 웨비나의 레코딩 링크 [웨비나 2](https://seminars.adobeconnect.com/p6lqaotpjnd3) 에서 이미지 사전 설정에 대해 설명합니다.
