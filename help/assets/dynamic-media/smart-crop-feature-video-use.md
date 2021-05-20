---
title: AEM Assets Dynamic Media에서 스마트 자르기 사용
description: 스마트 자르기는 Adobe Sensei을 사용하여 반응형 디자인을 위해 콘텐츠를 자르는데 시간이 오래 걸리고 비용이 많이 드는 작업을 제거합니다.
sub-product: dynamic-media
feature: 스마트 자르기, 이미지 프로필
version: 6.4, 6.5
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 3%

---


# AEM Assets Dynamic Media에서 스마트 자르기 사용{#using-smart-crop-with-aem-assets-dynamic-media}

스마트 자르기는 Adobe Sensei을 사용하여 반응형 디자인을 위해 콘텐츠를 자르는데 시간이 오래 걸리고 비용이 많이 드는 작업을 제거합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>비디오에서는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다. [Dynamic Media을 사용하여 AEM을 설정하는 방법은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/kr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager의 Dynamic Media 스마트 자르기 기능은 다음을 포함합니다

* AEM Asset 관리자는 장치 너비와 높이를 기반으로 스마트 자르기용 이미지 프로필을 쉽게 만들 수 있습니다.
* 스마트 자르기는 개별 자산에 대해 수행하거나 폴더 내의 모든 자산에 대해 수행할 수 있습니다.
* 스마트 자르기 편집 레이아웃 크기를 조정하여 더 잘 보이도록 할 수 있습니다.
* AEM Sites의 Dynamic Media 구성 요소는 스마트 자르기를 지원합니다.
* 스마트 잘림 자산에 대해 게시된 URL은 호스팅된 자산을 허용하는 타사 애플리케이션과 함께 사용할 수 있습니다.

>[!NOTE]
>
>스마트 자르기 좌표는 종횡비에 따라 다릅니다. 즉, 이미지 프로필의 다양한 스마트 자르기 설정에 대해 이미지 프로필에 추가된 차원에 대해 종횡비가 동일하면 동일한 종횡비가 Dynamic Media로 전송됩니다. 따라서 스마트 자르기 편집기에서는 동일한 자르기 영역을 추천합니다. 예를 들어 100x100 및 200x200의 자르기 설정을 사용하면 시스템에서 동일한 스마트 자르기가 생성됩니다.
