---
title: AEM Assets Dynamic Media에서 파노라마 및 세로 이미지 뷰어 사용
description: AEM 6.4의 Dynamic Media 뷰어 개선 사항에는 파노라마 이미지 뷰어, 파노라마 Virtual Reality 이미지 뷰어 및 수직 이미지 뷰어가 포함됩니다. Panoramic Viewer는 사용자 정의 개발 없이도 실내, 숙소, 위치 또는 환경에 대한 매력적이고 몰입 경험을 제공하는 손쉬운 방법을 제공합니다.
feature: Video Profiles, Video Profiles, 360 VR Video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 535
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# AEM Assets Dynamic Media에서 파노라마 및 세로 이미지 뷰어 사용{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4의 Dynamic Media 뷰어 개선 사항에는 파노라마 이미지 뷰어, 파노라마 Virtual Reality 이미지 뷰어 및 수직 이미지 뷰어가 포함됩니다. Panoramic Viewer는 사용자 정의 개발 없이도 실내, 숙소, 위치 또는 환경에 대한 매력적이고 몰입 경험을 제공하는 손쉬운 방법을 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/35631?quality=12&learn=on&captions=kor)

>[!NOTE]
>
>이 비디오에서는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다. [Dynamic Media를 사용하여 AEM을 설정하는 방법에 대한 지침은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/kr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 파노라마 및 파노라마 VR 뷰어

이미지는 종횡비나 키워드에 따라 파노라마로 간주됩니다. 기본적으로 종횡비가 2인 이미지는 파노라마 이미지로 간주됩니다. 위의 기준을 충족하면 파노라마 이미지 뷰어 사전 설정을 이미지 미리 보기에 사용할 수 있습니다. 파노라마 이미지 종횡비 기준은 회사의 DMS7 구성에서 /conf/global/settings/cloudconfigs/dmscene7/jcr:content에 이중 속성 s7PanoramicAR을 지정하여 수정할 수 있습니다. 키워드는 에셋 메타데이터 노드의 dc:keyword 속성에 저장됩니다. 키워드에 다음 조합 중 하나가 포함된 경우:

* 등각형이고,
* 구형 + 파노라마,
* 구형 + 파노라마,

종횡비에 상관없이 파노라마 이미지 자산으로 간주됩니다.

## 세로 이미지 뷰어

가로 색상 견본의 경우 소비자의 데스크탑 화면 크기에 따라 사용자가 페이지를 아래로 스크롤할 때까지 색상 견본이 표시되지 않는 경우가 있습니다. 세로 이미지 뷰어를 사용하고 세로 색상 견본을 배치하면 화면 크기와 상관없이 색상 견본이 표시됩니다. 또한 기본 이미지 크기를 최대화합니다. 가로 색상 견본을 사용하면 페이지가 표시될 가능성이 높고 기본 이미지의 크기가 줄어들게 하려면 페이지에서 공간을 예약해야 합니다. 수직 레이아웃에서는 이 공간을 할당할 필요가 없으므로 기본 이미지 크기를 최대화할 수 있습니다.

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
   <td><p>파노라마 뷰어 및 가상 현실 뷰어는 사용자에게 보다 매력적인 경험을 제공한다. 예약하기 전이라도 호텔 객실을 체크아웃할 수 있고, 예약하지 않아도 임대 부동산을 체크아웃할 수 있다. 사용자는 위치와 더 많은 가능성을 확인할 수 있습니다. 여기에서 주요 초점은 소비자가 웹 사이트를 방문할 때 더 나은 경험을 제공하고 결국 전환율을 높이는 것입니다.</p> <p> </p> </td> 
   <td><p>세로 이미지 뷰어는 제품 이미지 보기 경험을 극대화하여 소비자에게 제품을 가장 잘 표현할 수 있도록 해 줌으로써 전환을 유도하고 수익을 최소화할 수 있습니다.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>사용 가능 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Scene7 모드에서 Dynamic Media 구성](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/config-dms7.html)

[하이브리드 모드에서 Dynamic Media 구성](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>파노라마 뷰어는 파노라마 이미지와 함께 작동하며 일반 이미지와 함께 사용할 수 없습니다.
