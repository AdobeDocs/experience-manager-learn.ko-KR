---
title: OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅
description: AEM SDK의 로컬 빠른 시작에는 로컬 AEM 런타임에서 애플리케이션을 인식하는 방법과 AEM 내의 기능을 이해하는 데 유용한 다양한 정보와 설명을 제공하는 OSGi 웹 콘솔이 있습니다.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: 개발
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 1%

---


# OSGi 웹 콘솔을 사용하여 AEM SDK 디버깅

AEM SDK의 로컬 빠른 시작에는 로컬 AEM 런타임에서 애플리케이션을 인식하는 방법과 AEM 내의 기능을 이해하는 데 유용한 다양한 정보와 설명을 제공하는 OSGi 웹 콘솔이 있습니다.

AEM에서는 많은 OSGi 콘솔을 제공하며, 각 콘솔은 AEM의 다른 측면에 대한 주요 통찰력을 제공합니다. 그러나 다음은 일반적으로 응용 프로그램을 디버깅하는 데 가장 유용합니다.

## 번들

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

번들 콘솔은 OSGi 번들 및 해당 세부 정보가 시작 및 중지할 수 있는 임시 기능과 함께 AEM에 배포되는 OSGi 번들 카탈로그입니다.

번들 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 번들
+ 또는 다음 위치에서 바로 사용할 수 있습니다.[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

각 번들을 클릭하면 애플리케이션 디버깅에 도움이 되는 세부 정보가 제공됩니다.

+ OSGi 번들 확인
+ OSGi 번들이 활성 상태인지 확인
+ OSGi 번들에 불만족된 가져오기로 인해 OSGi 번들이 시작되지 않는지 확인

## 구성 요소

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

구성 요소 콘솔은 AEM에 배포된 모든 OSGi 구성 요소의 카탈로그이며, 정의된 OSGi 구성 요소 수명 주기에서 구성 요소가 참조할 수 있는 OSGi 서비스에 대한 모든 정보를 제공합니다

구성 요소 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > OSGi > 구성 요소
+ 또는 다음 위치에서 바로 사용할 수 있습니다.[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

디버깅 활동에 도움이 되는 주요 측면:

+ OSGi 번들 확인
+ OSGi 번들이 활성 상태인지 확인
+ OSGi 번들에 불만족된 가져오기로 인해 OSGi 번들이 시작되지 않는지 확인
+ Git에서 구성 요소에 대한 OSGi 구성을 만들려면 구성 요소의 PID를 가져옵니다
+ 활성 OSGi 구성에 바인딩된 OSGi 속성 값 식별

## Sling 모델

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling 모델 콘솔은 다음 위치에 있습니다.

+ 도구 > 작업 > 웹 콘솔 > 상태 > Sling 모델
+ 또는 다음 위치에서 바로 사용할 수 있습니다.[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

디버깅 활동에 도움이 되는 주요 측면:

+ Sling 모델 유효성 검사가 적절한 리소스 유형에 등록됩니다
+ Sling 모델의 유효성을 확인하는 것은 올바른 개체(Resource 또는 SlingHttpRequestServlet)에서 적용할 수 있습니다
+ Sling 모델 내보내기 유효성 검사가 올바르게 등록되었습니다
