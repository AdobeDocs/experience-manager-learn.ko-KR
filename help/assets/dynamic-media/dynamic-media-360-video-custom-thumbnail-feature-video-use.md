---
title: AEM Assets에서 Dynamic Media 360 비디오 및 사용자 지정 비디오 썸네일 사용
description: AEM 6.5의 Dynamic Media 뷰어 개선 사항에는 360 비디오 렌더링, 360 미디어 뷰어(video360Social 및 video360VR)에 대한 지원 추가 및 사용자 지정 비디오 썸네일을 선택하는 기능이 포함됩니다.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# AEM Assets에서 Dynamic Media 360 비디오 및 사용자 지정 비디오 썸네일 사용

AEM 6.5의 Dynamic Media 뷰어 개선 사항에는 360 비디오 렌더링, 360 미디어 뷰어(video360Social 및 video360VR)에 대한 지원 추가 및 사용자 지정 비디오 썸네일을 선택하는 기능이 포함됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>이 비디오에서는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다.  [Dynamic Media으로 AEM 설정에 대한 지침은 여기에서 확인할 수 있습니다](https://helpx.adobe.com/kr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 비디오를 업로드할 때 기본적으로 Dynamic Media은 종횡비가 2:1인 경우 푸티지를 360 비디오로 처리합니다. 즉, 너비 대 높이의 비율은 2:1입니다.

>[!NOTE]
>
>Dynamic Media 360 미디어 구성 요소는 360 비디오만 지원합니다.

## Dynamic Media 360 비디오

구면 영상이라고도 하는 360도 영상은 전방위 카메라를 이용하거나 카메라 집합을 이용해 모든 방향의 시야를 동시에 기록하는 영상 녹화물이다. 평면 디스플레이에서 재생하는 동안 사용자는 시청 방향을 제어하고 모바일 장치에서 재생은 일반적으로 내장된 자이로스코프 제어를 활용합니다.  단일 사진의 한계를 뛰어넘을 수 있도록 해줍니다. 마케터는 360개의 비디오를 통해 사용자에게 매력적인 경험을 제공할 수 있습니다.  시작하겠습니다. 파노라마 이미지 종횡비 기준은 회사의 DMS7 구성에서 /conf/global/settings/cloudconfigs/dmscene7/jcr:content에 이중 속성 s7PanoramicAR을 지정하여 수정할 수 있습니다.

## Dynamic Media 360 비디오

이제 Dynamic Media 비디오에서는 비디오에 대한 사용자 지정 썸네일을 선택하는 기능을 지원합니다. 사용자는 AEM Assets에서 기존 에셋을 선택하거나 비디오 프레임을 썸네일로 선택할 수 있습니다.

## Dynamic 360 Media 뷰어

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social 뷰어**</td>
      <td>**Video360VR 뷰어**</td>
   </tr>
   <tr>
      <td>Dynamic Media 실행 모드</td>
      <td>Dynamic Media Scene7 모드만 해당</td>
      <td>Dynamic Media Scene7 모드만 해당<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>사용 사례</td>
      <td>
         <p>자이로스코프를 지원하지 않는 웹 사이트 및 장치의 경우</p>
         <p> </p>
      </td>
      <td>
         <p>자이로스코프를 지원하는 장치에 가상 현실 환경을 제공합니다. </p>
      </td>
   </tr>
   <tr>
      <td>오디오 - 스테레오 모드</td>
      <td>아니요</td>
      <td>예</td>
   </tr>
   <tr>
      <td>비디오 재생</td>
      <td>예</td>
      <td>예</td>
   </tr>
   <tr>
      <td>POV 탐색</td>
      <td>
         <ul>
            <li>마우스 끌기(데스크탑 시스템)</li>
            <li>스와이프(터치 장치)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>마우스 및 끌기 옵션이 비활성화됨</li>
            <li>내장 자이로스코프 사용</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 플레이어</td>
      <td>예</td>
      <td>예</td>
   </tr>
   <tr>
      <td>소셜 공유 옵션</td>
      <td>예</td>
      <td>아니요</td>
   </tr>
</tbody>
</table>

## 추가 리소스{#additional-resources}

[Scene7 모드에서 Dynamic Media 구성](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
