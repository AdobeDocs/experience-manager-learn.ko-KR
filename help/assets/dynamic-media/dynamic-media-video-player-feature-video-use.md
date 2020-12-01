---
title: AEM Dynamic Media에서 비디오 플레이어 사용
seo-title: AEM Dynamic Media에서 비디오 플레이어 사용
description: 데스크탑 클라이언트 및 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임 기반의 AEM Dynamic Media 비디오 플레이어가 Flash 기반의 컨텐츠 스트리밍을 더욱 활성화했습니다. HLS(Apple의 HTTP Live Streaming 비디오 전달 프로토콜)가 도입됨에 따라 이제 flash에 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.
seo-description: 데스크탑 클라이언트 및 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임 기반의 AEM Dynamic Media 비디오 플레이어가 Flash 기반의 컨텐츠 스트리밍을 더욱 활성화했습니다. HLS(Apple의 HTTP Live Streaming 비디오 전달 프로토콜)가 도입됨에 따라 이제 flash에 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# AEM Dynamic Media에서 비디오 플레이어 사용{#using-the-video-player-in-aem-dynamic-media}

데스크탑 클라이언트 및 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임 기반의 AEM Dynamic Media 비디오 플레이어가 Flash 기반의 컨텐츠 스트리밍을 더욱 활성화했습니다. HLS(Apple의 HTTP Live Streaming 비디오 전달 프로토콜)가 도입됨에 따라 이제 flash에 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Flash이 아닌 비디오 플레이어 {#quick-look-into-non-flash-video-player}에 대한 빠른 검토

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

지원되지 않는 브라우저의 경우 점진적인 비디오 전달으로 폴백하는 HLS 브라우저 지원은 다음과 같습니다

<table> 
 <thead> 
  <tr> 
   <th> <p>장치</p> </th>
   <th> <p>브라우저</p> </th>
   <th > <p>비디오 재생 모드</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>데스크톱</p> </td>
   <td> <p>Internet Explorer 9 및 10</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr>
   <td> <p>데스크톱</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr>
   <td> <p>데스크톱</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크톱</p> </td>
   <td> <p>Firefox 45 이상</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크톱</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크톱</p> </td>
   <td> <p>Safari(Mac)</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Chrome(Android 6 또는 이전 버전)</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Chrome(Android 7 이상)</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Android(기본 브라우저)</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Safari(iOS)</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Chrome(iOS)</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>모바일</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
 </tbody>
</table>