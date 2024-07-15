---
title: AEM Dynamic Media에서 비디오 플레이어 사용
description: 데스크탑 클라이언트와 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임에 의존하던 AEM Dynamic Media video player가 플래시 기반 콘텐츠 스트리밍에 대한 공격적인 태도를 취하게 되었습니다. HLS(Apple의 HTTP 라이브 스트리밍 비디오 전송 프로토콜)가 도입됨에 따라 이제 플래시에 의존하지 않고 콘텐츠를 스트리밍할 수 있습니다.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 568
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# AEM Dynamic Media에서 비디오 플레이어 사용{#using-the-video-player-in-aem-dynamic-media}

데스크탑 클라이언트와 브라우저에서 적응형 비디오 스트리밍을 지원하기 위해 Flash 런타임에 의존하던 AEM Dynamic Media video player가 플래시 기반 콘텐츠 스트리밍에 대한 공격적인 태도를 취하게 되었습니다. HLS(Apple의 HTTP 라이브 스트리밍 비디오 전송 프로토콜)가 도입됨에 따라 이제 플래시에 의존하지 않고 콘텐츠를 스트리밍할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## 비 Flash 비디오 플레이어에 대한 빠른 살펴보기 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

지원되지 않는 브라우저에 대해 HLS 브라우저 지원은 다음과 같습니다. 점진적 비디오 제공으로 돌아갑니다

>[!NOTE]
>
> Dynamic Media Hybrid는 2022년 3월 15일부터 Internet Explorer 11에서 비디오 스트리밍을 지원하지 않습니다. IE 11의 점진적 재생으로 폴백하려면 6.5.12 이상으로 업그레이드하십시오.

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
   <td> <p>데스크탑</p> </td>
   <td> <p>Internet Explorer 9 및 10</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr>
   <td> <p>데스크탑</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Scene 7 모드: HLS 비디오 스트리밍</p> 
        <p>Dynamic Media - 하이브리드 모드: 점진적 다운로드</p>
   </td>
  </tr>
  <tr>
   <td> <p>데스크탑</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>점진적 다운로드</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크탑</p> </td>
   <td> <p>Firefox 45 이상</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크탑</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS 비디오 스트리밍</p> </td>
  </tr>
  <tr> 
   <td> <p>데스크탑</p> </td>
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
