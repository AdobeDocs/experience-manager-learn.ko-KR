---
title: 이미지 사전 설정
description: Dynamic Media Classic의 이미지 사전 설정에는 특정 크기, 형식, 품질 및 선명하게 하기를 사용하여 이미지를 만드는 데 필요한 모든 설정이 포함되어 있습니다. 이미지 사전 설정은 동적 크기 조절의 주요 구성 요소입니다. Dynamic Media Classic의 URL을 보면 이미지 사전 설정이 사용 중인지 쉽게 알 수 있습니다. 이미지 사전 설정, 이러한 사전 설정이 유용한 이유 및 사전 설정을 만드는 방법에 대해 알아봅니다.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 1%

---

# 이미지 사전 설정 {#image-presets}

이미지 사전 설정은 기본적으로 특정 크기, 형식, 품질 및 선명하게 하기 이미지를 만드는 데 필요한 모든 설정을 포함하는 레시피입니다. 이미지 사전 설정은 동적 크기 조절의 주요 구성 요소입니다.

Dynamic Media Classic 고객에 대한 URL을 살펴보면 사용 중인 이미지 사전 설정이 표시될 수 있습니다. URL의 끝에서 $name$을(를) 찾아보십시오(모든 단어 또는 단어가 name으로 대체됨).

이미지 사전 설정은 URL을 단축하므로 요청당 여러 개의 이미지 제공 지침을 작성하는 대신 하나의 이미지 사전 설정을 작성할 수 있습니다. 예를 들어 이 두 URL은 선명하게 하기를 사용하여 동일한 300 x 300 JPEG 이미지를 생성하지만 두 번째 URL은 이미지 사전 설정을 사용합니다.

![이미지](assets/image-presets/image-preset-2.png)

이미지 사전 설정의 진정한 가치는 회사 관리자가 이미지 사전 설정의 정의를 업데이트하고 웹 코드를 변경하지 않고 해당 형식을 사용하여 모든 이미지에 영향을 줄 수 있다는 것입니다. URL에 대한 캐시가 지워지면 이미지 사전 설정 변경 결과가 표시됩니다.

>[!IMPORTANT]
>
>이미지 크기를 조정할 때 이미지가 왜곡되지 않도록 가로 세로 비율인 가로 세로 비율을 항상 비례적으로 유지해야 합니다.

이미지 사전 설정 이름 양쪽에 달러 기호($)가 있고 물음표(?)를 따릅니다 구분 기호.

>[!TIP]
>
>사이트의 고유한 이미지 크기당 하나의 이미지 사전 설정을 만듭니다. 예를 들어 제품 세부 사항 페이지에 350 X 350 이미지, 검색 페이지에 120 X 120 이미지, 크로스셀/추천 항목에 90 X 90 이미지가 필요한 경우 이미지 500개든 500,000개든 관계없이 세 개의 이미지 사전 설정이 필요합니다.

- 자세히 알아보기 [이미지 사전 설정](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- 방법 알아보기 [이미지 사전 설정 만들기](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## 이미지 사전 설정 및 선명하게 하기

이미지 사전 설정은 일반적으로 이미지 크기를 조정하고 이미지의 크기를 원래 크기에서 조정할 때마다 선명하게 하기를 추가해야 합니다. 크기를 조정하면 많은 픽셀이 병합되어 더 작은 공간으로 혼합되어 이미지가 부드럽고 흐릿하게 보이기 때문입니다. 선명하게 하기를 사용하면 이미지에서 가장자리와 고대비 영역의 대비가 증가합니다.

Dynamic Media Classic에 업로드하는 고해상도 이미지는 전체 크기로 볼 때 확대 시 선명하게 할 필요가 없습니다. 그러나 어떤 작은 크기에서든, 일부 선명하게 하기가 보통 바람직하다.

>[!TIP]
>
>이미지 크기를 조정할 때 항상 선명하게 하기 즉, 모든 이미지 사전 설정(및 나중에 논의할 뷰어 사전 설정)에 선명하게 하기를 추가해야 합니다.
>
>이미지가 좋지 않은 경우 선명하게 해야 하거나 품질이 좋지 않았기 때문일 수 있습니다.

얼마나 선명하게 할지 추가하는 것은 전적으로 주관적입니다. 어떤 사람들은 더 부드러운 이미지를 좋아하는 반면, 다른 사람들은 매우 날카로운 이미지를 좋아한다. 이미지에 선명하게 하기 필터 조합을 실행하여 이미지를 쉽게 향상시킬 수 있습니다. 그러나 이미지를 지나치게 선명하게 하거나 지나치게 선명 효과를 내는 것도 쉽습니다.

다음 그래픽은 세 가지 수준의 선명하게 하기를 보여 줍니다. 오른쪽에서 왼쪽으로 선명 효과 없음, 알맞은 양, 그리고 너무 많습니다.

![이미지](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic에서는 단순 선명하게 하기, 리샘플링 모드 및 언샵 마스크의 세 가지 유형을 사용할 수 있습니다.

자세히 알아보기 [Dynamic Media Classic 선명하게 하기 옵션](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## 추가 리소스

[이미지 사전 설정 안내서](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 이미지 품질 및 로드 속도에 최적화하는 데 사용할 설정입니다.

[Image Is Everything Part 2: 단순한 흐림이 아닙니다. 품질과 속도](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). 고품질의 빠르게 로드되는 이미지를 제공하기 위해 이미지 사전 설정 사용에 대해 논의하는 블로그 게시물.
