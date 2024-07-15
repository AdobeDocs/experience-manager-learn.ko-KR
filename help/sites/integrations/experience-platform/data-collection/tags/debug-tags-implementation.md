---
title: 태그 구현 디버깅
description: Tags 구현을 디버깅하는 몇 가지 일반적인 도구와 기술을 소개합니다. 브라우저의 개발자 콘솔 및 Experience Platform 디버거 확장을 사용하여 Tags 구현의 주요 측면을 식별하고 해결하는 방법을 알아봅니다.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# 태그 구현 디버깅 {#debug-tags-implementation}

Tags 구현을 디버깅하는 데 사용되는 일반적인 도구 및 기술에 대한 소개입니다. 브라우저의 개발자 콘솔 및 Experience Platform 디버거 확장을 사용하여 Tags 구현의 주요 측면을 식별하고 해결하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Satellite 개체를 통한 클라이언트측 디버깅

클라이언트측 디버깅은 태그 속성 규칙 로드 또는 실행 순서를 확인하는 데 유용합니다. 웹 사이트에 태그 속성이 추가될 때마다 `_satellite` JavaScript 개체가 브라우저에 표시되어 클라이언트측 이벤트와 데이터 추적을 용이하게 합니다.

클라이언트측 디버깅을 사용하려면 `_satellite` 개체에서 `setDebug(true)` 메서드를 호출하십시오.

1. 브라우저 콘솔을 열고 아래 명령을 실행합니다.

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEM 사이트 페이지를 다시 로드하고 콘솔 로그에 아래와 같이 _규칙 실행_ 메시지가 표시되는지 확인하십시오.

   ![작성자 및 Publish 페이지의 태그 속성](assets/satellite-object-debugging.png)

## Adobe Experience Platform Debugger을 통한 디버깅

Adobe은 Adobe Experience Platform Debugger [Chrome 확장](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)을(를) 제공하여 통합을 디버깅하고, 이해하고, 통합에 대한 통찰력을 얻을 수 있습니다.

1. Adobe Experience Platform Debugger 확장을 열고 Publish 인스턴스에서 사이트 페이지를 엽니다

2. **Adobe Experience Platform Debugger > 요약 > Adobe Experience Platform 태그** 섹션에서 이름, 버전, 빌드 날짜, 환경 및 확장과 같은 태그 속성 세부 정보를 확인합니다.

   ![Adobe Experience Platform Debugger 및 태그 속성 세부 정보](assets/tag-property-details.png)

## 추가 리소스 {#additional-resources}

+ [Adobe Experience Platform Debugger 소개](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [위성 개체 참조](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
