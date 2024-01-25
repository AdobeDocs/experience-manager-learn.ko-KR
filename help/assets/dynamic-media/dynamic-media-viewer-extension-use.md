---
title: Adobe Analytics 및 Adobe Launch에서 Dynamic Media Viewer 사용
description: Dynamic Media Viewers 5.13 릴리스와 함께 Adobe Launch용 Dynamic Media Viewers 확장을 사용하면 Dynamic Media, Adobe Analytics 및 Adobe Launch 고객은 Adobe Launch 구성에서 Dynamic Media Viewer에 고유한 이벤트 및 데이터를 사용할 수 있습니다.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 637
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 17%

---

# Adobe Analytics 및 Adobe Launch에서 Dynamic Media Viewer 사용{#using-dynamic-media-viewers-adobe-analytics-launch}

Dynamic Media 및 Adobe Analytics을 사용하는 고객의 경우 이제 Dynamic Media Viewer Extension을 사용하여 웹 사이트에서 Dynamic Media Viewer 사용을 추적할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> 이 기능을 사용하려면 Dynamic Media Scene7 모드에서 Adobe Experience Manager을 실행하십시오. 또한 다음을 수행해야 합니다 [AEM 인스턴스와 Adobe Experience Platform Launch 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html).

Dynamic Media 뷰어 확장 기능 도입으로 이제 Adobe Experience Manager은 Dynamic Media 뷰어(5.13)와 함께 제공된 자산에 대한 고급 분석 지원을 제공하여 Dynamic Media 뷰어를 Sites 페이지에서 사용할 때 이벤트 추적을 보다 세밀하게 제어할 수 있습니다.

AEM Assets 및 Sites가 이미 있는 경우 Launch 속성을 AEM 작성자 인스턴스와 통합할 수 있습니다. Launch 통합이 웹 사이트와 연결되면 뷰어에 대한 이벤트 추적을 활성화하여 다이내믹 미디어 구성 요소를 페이지에 추가할 수 있습니다.

AEM Assets 전용 고객 또는 Dynamic Media Classic 고객의 경우 사용자는 뷰어에 대한 포함 코드를 가져와 페이지에 추가할 수 있습니다. 그런 다음 Launch 스크립트 라이브러리를 뷰어 이벤트 추적을 위해 페이지에 수동으로 추가할 수 있습니다.

다음 표에는 Dynamic Media 뷰어 이벤트와 지원되는 인수가 나와 있습니다.

<table>
   <tbody>
      <tr>
         <td>뷰어 이벤트 이름</td>
         <td>인수 참조</td>
      </tr>
      <tr>
         <td> 공통 </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> 배너 <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> 항목 </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> 로드 </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> 메타데이터 </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> 마일스톤 </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> 페이지 </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> 일시 중지 </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> 재생 </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> 회전 </td>
         <td> %event.detail.dm.SPIN.frameenumber% </td>
      </tr>
      <tr>
         <td> 중지 </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> 교체 </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> 견본 </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> 타르크 </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> 확대/축소 </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## 추가 리소스{#additional-resources}

* [Adobe Experience Manager과 Adobe Launch 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [Dynamic Media Scene7 모드에서 Adobe Experience Manager 실행](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [Adobe Analytics 및 Adobe Launch와 Dynamic Media Viewer 통합](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
