---
title: AEM Assets에서 Dynamic Media 360 비디오 및 사용자 정의 비디오 축소판 사용
description: AEM 6.5의 Dynamic Media 뷰어 개선 기능에는 360개의 비디오 렌더링 지원, 360개의 미디어 뷰어(video360Social 및 video360VR) 지원 추가, 사용자 정의 비디오 축소판 선택 기능이 포함되어 있습니다.
sub-product: dynamic-media
feature: 비디오 프로필
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 5%

---


# AEM Assets에서 Dynamic Media 360 비디오 및 사용자 정의 비디오 축소판 사용

AEM 6.5의 Dynamic Media 뷰어 개선 기능에는 360개의 비디오 렌더링 지원, 360개의 미디어 뷰어(video360Social 및 video360VR) 지원 추가, 사용자 정의 비디오 축소판 선택 기능이 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>비디오에서는 AEM 인스턴스가 Dynamic Media S7 모드에서 실행 중이라고 가정합니다.  [Dynamic Media을 사용하여 AEM을 설정하는 방법은 여기에서 확인할 수 있습니다](https://helpx.adobe.com/kr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 비디오를 업로드하면 기본적으로 Dynamic Media에서 2:1의 종횡비가 있는 경우 푸티지를 360 비디오로 처리합니다. 즉, 폭과 높이 비율은 2:1입니다.

>[!NOTE]
>
>Dynamic Media 360 미디어 구성 요소는 360 비디오만 지원합니다.

## Dynamic Media 360 비디오

구면 비디오라고도 하는 360도 동영상은 모든 방향성 카메라나 카메라 컬렉션을 사용하여 촬영하면서 모든 방향으로의 뷰를 동시에 기록하는 비디오 녹화입니다. 평면 디스플레이에서 재생하는 동안 사용자는 보기 방향을 제어할 수 있으며 모바일 장치에서 재생은 일반적으로 내장된 자이로스코프 제어를 활용합니다.  이것은 단일 사진의 한계를 넘어 확장시킬 수 있습니다. 마케터는 사용자에게 360개의 비디오의 도움을 받아 매력적인 경험을 제공할 수 있습니다.  시작해 보겠습니다. 파노라마 이미지 종횡비 기준은 /conf/global/settings/cloudconfigs/dmscene7/jcr:content에서 이중 속성 s7PanasonicAR을 지정하여 회사의 DMS7 구성에서 수정할 수 있습니다.

## Dynamic Media 360 비디오

Dynamic Media 비디오에서는 이제 비디오에 대한 사용자 지정 축소판 그림을 선택하는 기능을 지원합니다. 사용자는 AEM Assets에서 기존 자산을 선택하거나 비디오 프레임을 축소판으로 선택할 수 있습니다.

## Dynamic 360 Media Viewer

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR 뷰어**</td>
   </tr>
   <tr>
      <td>Dynamic Media 실행 모드</td>
      <td>Dynamic Media Scene7 모드만</td>
      <td>Dynamic Media Scene7 모드만<br>
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
            <li>밀기(터치 장치)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>마우스 및 드래그 옵션이 비활성화됩니다</li>
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
