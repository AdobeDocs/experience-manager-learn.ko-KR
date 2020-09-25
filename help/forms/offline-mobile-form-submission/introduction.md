---
title: HTM5 양식 제출 시 AEM 워크플로우 트리거
seo-title: HTML5 양식 제출 시 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
seo-description: 오프라인 모드에서 모바일 양식 입력을 계속하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# 부분적으로 완성된 모바일 양식 다운로드 및 AEM 워크플로우에 제출

일반적으로 XDP를 데이터 캡처 활동을 위해 HTML로 렌더링하는 기능을 사용하는 경우가 있습니다. 이러한 기능은 간단한 양식을 온라인으로 작성 및 제출할 수 있는 경우에 적합합니다. 그러나 양식이 복잡하다면 사용자가 온라인으로 양식을 작성할 수 없을 수도 있습니다. 오프라인 상태에서 Acrobat/Reader을 사용하여 양식 작성자가 대화형 버전의 양식을 채울 수 있도록 허용해야 합니다. 양식이 작성되면 사용자는 온라인 상태에서 양식을 제출할 수 있습니다.
이 사용 사례를 수행하려면 다음 단계를 수행해야 합니다.

* 모바일 양식에 입력된 데이터로 인터랙티브한/입력 가능한 PDF 생성
* Acrobat/Reader에서 PDF 제출 처리
* Adobe Experience Manager(AEM) 워크플로우를 트리거하여 제출된 PDF 검토

이 자습서에서는 위의 사용 사례를 수행하는 데 필요한 단계를 안내합니다. 이 자습서와 관련된 샘플 코드 및 자산은 여기에서 [사용할 수 있습니다.](part-four.md)

다음 비디오에서는 사용 사례에 대한 개요를 제공합니다

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

