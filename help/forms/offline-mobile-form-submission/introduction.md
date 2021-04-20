---
title: HTM5 양식 제출 시 AEM 워크플로우 트리거
seo-title: HTML5 양식 제출 시 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 전송하여 AEM 워크플로우를 트리거할 수 있습니다.
seo-description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 전송하여 AEM 워크플로우를 트리거할 수 있습니다.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# 부분적으로 완성된 모바일 양식 다운로드 및 AEM 작업 과정에 제출

일반적인 사용 사례는 데이터 캡처 활동을 위해 XDP를 HTML로 렌더링하는 기능을 갖는 것입니다. 이러한 기능은 간단한 양식을 온라인으로 작성하여 제출할 수 있을 때 유용합니다. 그러나 양식이 복잡하다면 사용자가 온라인으로 양식을 작성할 수 없을 수도 있습니다. Adobe는 양식 작성자가 Acrobat/Reader을 사용하여 오프라인 방식으로 양식의 대화형 버전을 다운로드할 수 있도록 하는 기능을 제공해야 합니다. 양식이 작성되면 사용자는 온라인으로 양식을 제출할 수 있습니다.
이 사용 사례를 수행하려면 다음 단계를 수행해야 합니다.

* 모바일 양식에 입력된 데이터로 인터랙티브한/입력 가능한 PDF 생성
* Acrobat/Reader에서 PDF 제출 처리
* Adobe Experience Manager(AEM) 워크플로우를 트리거하여 제출된 PDF 검토

이 자습서에서는 위의 사용 사례를 수행하는 데 필요한 단계를 안내합니다. 이 자습서와 관련된 샘플 코드 및 자산은 [여기에서 사용할 수 있습니다.](part-four.md)

다음 비디오에서는 사용 사례에 대한 개요를 알 수 있습니다

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

