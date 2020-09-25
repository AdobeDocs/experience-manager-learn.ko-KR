---
title: AEM Assets Dynamic Media에서 스마트 자르기 사용
seo-title: AEM Assets Dynamic Media에서 스마트 자르기 사용
description: Smart Crop은 Adobe Sensei을 사용하여 반응형 디자인을 위해 컨텐츠를 자르는 데 많은 시간과 비용이 소요되는 작업을 제거합니다.
seo-description: Smart Crop은 Adobe Sensei을 사용하여 반응형 디자인을 위해 컨텐츠를 자르는 데 많은 시간과 비용이 소요되는 작업을 제거합니다.
uuid: 2cb27aa8-644d-4b17-8ffc-f6a99f95cfd2
discoiquuid: e4b8534c-fa64-491f-86ec-4dbe50cd6bf7
sub-product: dynamic-media
feature: smart-crop, image-profiles
topics: images, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---


# AEM Assets Dynamic Media에서 스마트 자르기 사용{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop은 Adobe Sensei을 사용하여 반응형 디자인을 위해 컨텐츠를 자르는 데 많은 시간과 비용이 소요되는 작업을 제거합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>비디오는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다. [Dynamic Media를 사용하여 AEM을 설정하는 방법은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager의 Dynamic Media Smart Crop 기능 포함

* AEM 에셋 관리자는 디바이스 폭과 높이를 기반으로 스마트 자르기를 위한 이미지 프로필을 손쉽게 만들 수 있습니다.
* 개별 자산에 대해 스마트 자르기를 수행하거나 폴더 내의 모든 자산에 대해 수행할 수 있습니다.
* 스마트 자르기 편집 레이아웃의 크기를 조절하여 더 잘 볼 수 있습니다.
* AEM Sites의 Dynamic Media 구성 요소는 스마트 자르기를 지원합니다.
* 스마트 잘림 자산에 대한 게시된 URL은 호스팅된 자산을 허용하는 타사 응용 프로그램에서 사용할 수 있습니다.

>[!NOTE]
>
>스마트 자르기 좌표는 종횡비에 따라 다릅니다. 즉, 이미지 프로필의 다양한 스마트 자르기 설정의 경우 종횡비가 이미지 프로필의 추가된 크기에 대해 동일하면 동일한 종횡비가 Dynamic Media로 전송됩니다. 이로 인해 스마트 자르기 편집기에서도 동일한 자르기 영역이 표시됩니다. 예를 들어 100x100 및 200x200의 자르기 설정이 시스템에서 동일한 스마트 자르기를 생성합니다.
