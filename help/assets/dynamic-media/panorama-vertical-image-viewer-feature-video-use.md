---
title: AEM Assets Dynamic Media에서 파노라마 및 세로 이미지 뷰어 사용
description: AEM 6.4의 Dynamic Media 뷰어 개선 사항에는 파노라마 이미지 뷰어, 파노라마 가상 실제 이미지 뷰어 및 세로 이미지 뷰어가 추가되었습니다. 파노라마 뷰어를 사용하면 사용자 정의 개발 없이 공간, 속성, 위치 또는 조경에 대한 매력적인 경험을 손쉽게 전달할 수 있습니다.
sub-product: dynamic-media
feature: 비디오 프로필, 비디오 프로필, 360 VR 비디오
version: 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 3%

---


# AEM Assets Dynamic Media에서 파노라마 및 세로 이미지 뷰어 사용{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4의 Dynamic Media 뷰어 개선 사항에는 파노라마 이미지 뷰어, 파노라마 가상 실제 이미지 뷰어 및 세로 이미지 뷰어가 추가되었습니다. 파노라마 뷰어를 사용하면 사용자 정의 개발 없이 공간, 속성, 위치 또는 조경에 대한 매력적인 경험을 손쉽게 전달할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>비디오에서는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다. [Dynamic Media을 사용하여 AEM을 설정하는 방법은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/kr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 파노라마 및 파노라마 VR 뷰어

이미지는 종횡비나 키워드에 따라 파노라마로 간주됩니다. 기본적으로 종횡비가 2인 이미지는 파노라마 이미지로 간주됩니다. 위의 기준을 충족하면 이미지 미리 보기에 파노라마 이미지 뷰어 사전 설정을 사용할 수 있습니다. 파노라마 이미지 종횡비 기준은 /conf/global/settings/cloudconfigs/dmscene7/jcr:content에서 이중 속성 s7PanasonicAR을 지정하여 회사의 DMS7 구성에서 수정할 수 있습니다. 키워드는 자산 메타데이터 노드의 dc:keyword 속성에 저장됩니다. 키워드에 다음 조합 중 하나가 포함되어 있는 경우:

* 정사각형,
* 구형 + 파노라마
* 구형 + 파노라마,

종횡비에 관계없이 파노라마 이미지 자산으로 간주됩니다.

## 세로 이미지 뷰어

가로 색상 견본을 사용하면 소비자의 데스크탑 화면 크기에 따라 사용자가 페이지를 아래로 스크롤할 때까지 색상 견본이 표시되지 않는 경우가 있습니다. 세로 이미지 뷰어를 사용하고 세로 색상 견본을 배치하면 화면 크기에 상관없이 색상 견본이 표시되도록 합니다. 또한 기본 이미지 크기를 극대화합니다. 가로 색상 견본을 사용하면 볼 가능성이 높고 기본 이미지 크기를 줄이기 위해 페이지의 공간을 예약해야 했습니다. 세로 레이아웃이 있으면 이 공간을 할당하는 것에 대해 걱정할 필요가 없으므로 기본 이미지 크기를 최대화할 수 있습니다.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>파노라마 및 VR 뷰어</td>
   <td>세로 이미지 뷰어</td>
  </tr>
  <tr>
   <td>Dynamic Media 실행 모드</td>
   <td>Dynamic Media Scene7 모드만</td>
   <td>DMS7 및 Dynamic Media</td>
  </tr>
  <tr>
   <td>사용 사례</td>
   <td><p>파노라마 뷰어와 가상 현실 뷰어는 사용자에게 더 매력적인 경험을 제공합니다. 예약을 하기 전에도 호텔방을 체크아웃하거나, 예약을 하지 않고도 대여 서비스를 이용할 수 있다. 사용자는 위치 및 더 많은 가능성을 확인할 수 있습니다. 여기에서 가장 중요한 것은 소비자가 웹 사이트를 방문하여 사용 중인 전환율을 높일 때 더 나은 경험을 제공하려는 것입니다.</p> <p> </p> </td> 
   <td><p>수직 이미지 뷰어는 제품 이미지 보기 환경을 최대화하여 소비자에게 제품을 가장 잘 표현할 수 있도록 함으로써 전환을 유도하고 반품을 최소화할 수 있습니다.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>사용 가능 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Scene7 모드에서 Dynamic Media 구성](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[하이브리드 모드에서 Dynamic Media 구성](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>파노라마 뷰어는 파노라마 이미지와 함께 작동하며 일반 이미지와 함께 사용되지는 않습니다.
