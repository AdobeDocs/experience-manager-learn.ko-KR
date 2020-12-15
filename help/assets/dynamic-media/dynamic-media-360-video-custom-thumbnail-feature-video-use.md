---
title: AEM Assets에서 Dynamic Media 360 비디오 및 사용자 정의 비디오 축소판 사용
seo-title: AEM Assets에서 Dynamic Media 360 비디오 및 사용자 정의 비디오 축소판 사용
description: AEM 6.5의 Dynamic Media 뷰어 개선 사항에는 360개 비디오 렌더링 지원, 360개의 미디어 뷰어(video360Social 및 video360VR) 지원 추가, 사용자 정의 비디오 축소판 선택 기능이 포함되어 있습니다.
seo-description: AEM 6.5의 Dynamic Media 뷰어 개선 사항에는 360개 비디오 렌더링 지원, 360개의 미디어 뷰어(video360Social 및 video360VR) 지원 추가, 사용자 정의 비디오 축소판 선택 기능이 포함되어 있습니다.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# AEM Assets에서 Dynamic Media 360 비디오 및 사용자 정의 비디오 축소판 사용

AEM 6.5의 Dynamic Media 뷰어 개선 사항에는 360개 비디오 렌더링 지원, 360개의 미디어 뷰어(video360Social 및 video360VR) 지원 추가, 사용자 정의 비디오 축소판 선택 기능이 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>비디오는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다.  [Dynamic Media을 사용하여 AEM을 설정하는 방법은 여기를 참조하십시오](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 비디오를 업로드할 때 기본적으로 Dynamic Media은 종횡비가 2:1인 경우 푸티지를 360 비디오로 처리합니다. 예를 들어 폭에서 높이 사이의 비율은 2:1입니다.

>[!NOTE]
>
>Dynamic Media 360 미디어 구성 요소는 360 비디오만 지원합니다.

## Dynamic Media 360 비디오

360도 비디오(구형 비디오)는 전방향 카메라나 카메라 컬렉션을 사용하여 촬영한 모든 방향의 보기를 동시에 녹화하는 비디오 녹화입니다. 평면 디스플레이에서 재생하는 동안 사용자는 보기 방향을 제어할 수 있으며 모바일 디바이스에서 재생은 일반적으로 내장된 회전 제어 기능을 사용합니다.  이 기능을 사용하면 사진 한 장의 한계를 뛰어넘을 수 있습니다. 마케터는 360개의 비디오를 활용하여 매력적인 경험을 제공할 수 있습니다.  시작하기 파노라마 이미지 종횡비 기준은 /conf/global/settings/cloudconfigs/dmscene7/jcr:content에서 이중 속성 s7PanoramicAR을 지정하여 회사의 DMS7 구성에서 수정할 수 있습니다.

## Dynamic Media 360 비디오

이제 Dynamic Media 비디오에서 비디오의 사용자 정의 축소판을 선택할 수 있습니다. 사용자는 AEM Assets에서 기존 에셋을 선택하거나 축소판으로 비디오 프레임을 선택할 수 있습니다.

## 동적 360 미디어 뷰어

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR Viewer**</td>
   </tr>
   <tr>
      <td>Dynamic Media 실행 모드</td>
      <td>Dynamic Media Scene7 모드만 해당</td>
      <td>Dynamic Media Scene7 모드만<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>사용 사례</td>
      <td>
         <p>자이로스코프를 지원하지 않는 웹 사이트 및 디바이스</p>
         <p> </p>
      </td>
      <td>
         <p>자이로스코프를 지원하는 장치에 대한 가상 현실 환경을 제공합니다. </p>
      </td>
   </tr>
   <tr>
      <td>오디오 - 스테레오 모드</td>
      <td>아니오</td>
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
            <li>마우스 드래그(데스크톱 시스템에서)</li>
            <li>스와이프(터치 장치)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>마우스 및 드래그 옵션이 비활성화됩니다.</li>
            <li>내장 자이로스코프 사용</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 Player</td>
      <td>예</td>
      <td>예</td>
   </tr>
   <tr>
      <td>소셜 공유 옵션</td>
      <td>예</td>
      <td>아니오</td>
   </tr>
</tbody>
</table>

## 추가 리소스{#additional-resources}

[Scene7 모드에서 Dynamic Media 구성](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
