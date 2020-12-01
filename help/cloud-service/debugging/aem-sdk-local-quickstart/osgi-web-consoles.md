---
title: OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅
description: AEM SDK의 로컬 quickstart에는 애플리케이션이 어떻게 인식되는지, 그리고 AEM 내에서 어떻게 작동하는지 이해하는 데 유용한 다양한 정보와 설명을 로컬 AEM 런타임에 제공하는 OSGi 웹 콘솔이 있습니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅

AEM SDK의 로컬 quickstart에는 애플리케이션이 어떻게 인식되는지, 그리고 AEM 내에서 어떻게 작동하는지 이해하는 데 유용한 다양한 정보와 설명을 로컬 AEM 런타임에 제공하는 OSGi 웹 콘솔이 있습니다.

AEM은 다양한 OSGi 콘솔을 제공하며, 각 콘솔은 AEM의 다양한 측면에 대한 주요 통찰력을 제공하지만, 일반적으로 응용 프로그램을 디버깅하는 데 가장 유용합니다.

## 번들

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

번들 콘솔은 OSGi 번들 및 세부 사항을 시작 및 중지할 수 있는 고급 기능과 함께 AEM에 배포한 OSGi 번들 카탈로그입니다.

번들 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 번들
+ 또는[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

각 번들을 클릭하면 애플리케이션 디버깅에 도움이 되는 세부 정보가 제공됩니다.

+ OSGi 번들의 유효성을 검사하는 중입니다.
+ OSGi 번들이 활성 상태인지 확인
+ OSGi 번들의 가져오기가 만족스러워서 시작할 수 없는지 확인

## 구성 요소

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

구성 요소 콘솔은 AEM에 배포된 모든 OSGi 구성 요소의 카탈로그이며, 정의된 OSGi 구성 요소 수명 주기에서부터 구성 요소가 참조할 수 있는 OSGi 서비스에 대한 모든 정보를 제공합니다

구성 요소 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 구성 요소
+ 또는[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

디버깅 활동에 도움이 되는 주요 측면:

+ OSGi 번들의 유효성을 검사하는 중입니다.
+ OSGi 번들이 활성 상태인지 확인
+ OSGi 번들의 가져오기가 만족스러워서 시작할 수 없는지 확인
+ 구성 요소의 PID를 획득하여 Git에서 구성 요소에 대한 OSGi 구성을 생성합니다.
+ 활성 OSGi 구성에 바인딩된 OSGi 속성 값 식별

## Sling Models

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Models 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > 상태 > 모델 정렬
+ 또는[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

디버깅 활동에 도움이 되는 주요 측면:

+ Sling 모델의 유효성을 검사하는 것이 적절한 리소스 유형에 등록되었습니다.
+ Sling 모델의 유효성을 검사하는 것은 올바른 객체(Resource 또는 SlingHttpRequestServlet)에서 사용할 수 있습니다.
+ 슬링 모델 내보내기 유효성 검사가 올바르게 등록되었습니다.
