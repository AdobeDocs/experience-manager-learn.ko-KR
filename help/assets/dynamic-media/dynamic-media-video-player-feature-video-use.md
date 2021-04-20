---
title: AEM Dynamic Media에서 비디오 플레이어 사용
description: 데스크탑 클라이언트 및 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임을 사용하는 AEM Dynamic Media 비디오 플레이어가 더욱 공격적으로 flash 기반의 컨텐츠 스트리밍을 사용하고 있습니다. HLS(Apple의 HTTP Live 스트리밍 비디오 전달 프로토콜)가 도입됨에 따라 이제 flash를 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 9%

---


# AEM Dynamic Media에서 비디오 플레이어 사용{#using-the-video-player-in-aem-dynamic-media}

데스크탑 클라이언트 및 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임을 사용하는 AEM Dynamic Media 비디오 플레이어가 더욱 공격적으로 flash 기반의 컨텐츠 스트리밍을 사용하고 있습니다. HLS(Apple의 HTTP Live 스트리밍 비디오 전달 프로토콜)가 도입됨에 따라 이제 flash를 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Flash이 아닌 비디오 플레이어 {#quick-look-into-non-flash-video-player}에 대한 빠른 검토

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

지원되지 않는 브라우저의 경우 점진적 비디오 전달으로 폴백하는 HLS 브라우저 지원은 다음과 같습니다.

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