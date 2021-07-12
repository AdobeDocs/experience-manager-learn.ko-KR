---
title: AEM Dynamic Media에서 비디오 플레이어 사용
description: 데스크탑 클라이언트와 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임을 사용하는 AEM Dynamic Media 비디오 플레이어가 플래시 기반 컨텐츠 스트리밍에서 더 적극적이 되었습니다. HLS(Apple의 HTTP Live 스트리밍 비디오 게재 프로토콜)의 도입으로 이제 플래시에 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.
sub-product: dynamic-media
feature: 비디오 프로필
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 9%

---


# AEM Dynamic Media에서 비디오 플레이어 사용{#using-the-video-player-in-aem-dynamic-media}

데스크탑 클라이언트와 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임을 사용하는 AEM Dynamic Media 비디오 플레이어가 플래시 기반 컨텐츠 스트리밍에서 더 적극적이 되었습니다. HLS(Apple의 HTTP Live 스트리밍 비디오 게재 프로토콜)의 도입으로 이제 플래시에 의존하지 않고 컨텐츠를 스트리밍할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 비Flash 비디오 플레이어에 대한 빠른 검색 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS 브라우저 지원은 지원되지 않는 브라우저의 경우 점진적 비디오 게재로 폴백합니다

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
   <td> <p>Chrome(Android 6 이하)</p> </td>
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