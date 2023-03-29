---
title: 태그 구현 디버깅
description: 태그 구현을 디버깅하는 몇 가지 일반적인 도구와 기술을 소개합니다. 브라우저의 개발자 콘솔 및 Experience Platform 디버거 확장 기능을 사용하여 태그 구현의 주요 측면을 식별하고 해결하는 방법을 알아봅니다.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# 태그 구현 디버깅 {#debug-tags-implementation}

태그 구현을 디버깅하는 데 사용되는 일반적인 도구와 기술을 소개합니다. 브라우저의 개발자 콘솔 및 Experience Platform 디버거 확장 기능을 사용하여 태그 구현의 주요 측면을 식별하고 해결하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Satellite 개체를 통한 클라이언트 측 디버깅

클라이언트측 디버깅은 태그 속성 규칙 로드 또는 실행 순서를 확인하는 데 유용합니다. 웹 사이트에 Tag 속성을 추가할 때마다 `_satellite` 클라이언트측 이벤트 및 데이터 추적을 용이하게 하기 위해 브라우저에 JavaScript 개체가 있습니다.

클라이언트측 디버깅을 활성화하려면 `setDebug(true)` 메서드 `_satellite` 개체.

1. 브라우저 콘솔을 열고 아래의 명령을 실행합니다.

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEM 사이트 페이지를 다시 로드하고 콘솔 로그가 표시되는지 확인합니다 _규칙이 실행됨_ 아래와 같은 메시지.

   ![작성자 및 게시 페이지의 태그 속성](assets/satellite-object-debugging.png)

## Adobe Experience Platform Debugger를 통한 디버깅

Adobe 제공 Adobe Experience Platform Debugger [Chrome 확장 프로그램](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 및 [Firefox 추가 기능](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 통합을 디버그, 이해 및 통찰력을 얻을 수 있습니다.

1. Adobe Experience Platform Debugger 확장을 열고 게시 인스턴스에서 사이트 페이지를 엽니다.

1. 에서 **Adobe Experience Platform Debugger > 요약 > Adobe Experience Platform 태그** 섹션에서 이름, 버전, 빌드 날짜, 환경 및 확장과 같은 태그 속성 세부 사항을 확인합니다.

   ![Adobe Experience Platform Debugger 및 태그 속성 세부 사항](assets/tag-property-details.png)

## 추가 리소스 {#additional-resources}

+ [Adobe Experience Platform Debugger 소개](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [위성 개체 참조](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
