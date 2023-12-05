---
title: OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅
description: AEM SDK의 로컬 빠른 시작에는 애플리케이션이 AEM에서 인식되고 작동하는 방식을 이해하는 데 유용한 로컬 AEM 런타임에 대한 다양한 정보 및 자체 검사를 제공하는 OSGi 웹 콘솔이 있습니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 516
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅

AEM SDK의 로컬 빠른 시작에는 애플리케이션이 AEM에서 인식되고 작동하는 방식을 이해하는 데 유용한 로컬 AEM 런타임에 대한 다양한 정보 및 자체 검사를 제공하는 OSGi 웹 콘솔이 있습니다.

AEM은 여러 OSGi 콘솔을 제공하며 각각은 AEM의 다양한 측면에 대한 주요 통찰력을 제공합니다. 하지만 일반적으로 다음 기능은 애플리케이션을 디버깅하는 데 가장 유용합니다.

## 번들

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

번들 콘솔은 시작 및 중지를 수행하는 임시 기능과 함께 AEM에 배포된 OSGi 번들 및 세부 정보의 카탈로그입니다.

번들 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 번들
+ 또는 다음 위치에서 바로 사용할 수 있습니다. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

각 번들을 클릭하면 응용 프로그램 디버깅에 도움이 되는 세부 정보가 제공됩니다.

+ OSGi 번들이 존재하는지 확인
+ OSGi 번들이 활성 상태인지 확인하는 중
+ OSGi 번들이 충족되지 않은 가져오기로 인해 시작되지 않았는지 확인

## 구성 요소

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

구성 요소 콘솔은 AEM에 배포된 모든 OSGi 구성 요소의 카탈로그이며 정의된 OSGi 구성 요소 수명 주기에서 참조할 수 있는 OSGi 서비스에 이르기까지 구성 요소에 대한 모든 정보를 제공합니다

구성 요소 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 구성 요소
+ 또는 다음 위치에서 바로 사용할 수 있습니다. [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

디버깅 활동에 도움이 되는 주요 측면:

+ OSGi 번들이 존재하는지 확인
+ OSGi 번들이 활성 상태인지 확인하는 중
+ OSGi 번들이 충족되지 않은 가져오기로 인해 시작되지 않았는지 확인
+ Git에서 구성 요소에 대한 OSGi 구성을 생성하기 위해 구성 요소의 PID 가져오기
+ 활성 OSGi 구성에 바인딩된 OSGi 속성 값 식별

## Sling 모델

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Sling 모델 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > 상태 > Sling 모델
+ 또는 다음 위치에서 바로 사용할 수 있습니다. [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

디버깅 활동에 도움이 되는 주요 측면:

+ Sling 모델이 적절한 리소스 유형에 등록되어 있는지 확인합니다.
+ Sling 모델 유효성 검사는 올바른 개체(리소스 또는 SlingHttpRequestServlet)에서 조정할 수 있습니다.
+ Sling 모델 내보내기가 올바르게 등록되었는지 확인
