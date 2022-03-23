---
title: HTM5 양식 제출 소개에서 AEM 워크플로우 트리거
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 오프라인 모드에서 모바일 양식 채우기를 계속 수행하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 부분적으로 완료된 모바일 양식 다운로드 및 AEM 워크플로우에 제출

일반적인 사용 사례는 데이터 캡처 활동에 대한 HTML으로 XDP를 렌더링할 수 있는 기능입니다. 이 기능은 간단한 양식을 온라인으로 작성 및 제출할 수 있을 때 잘 작동합니다. 그러나 양식이 복잡하면 사용자가 온라인으로 양식을 완료하지 못할 수 있습니다. 양식 작성자가 Acrobat/Reader을 사용하여 대화형 양식의 버전을 오프라인 방식으로 채울 수 있도록 허용하는 기능을 제공해야 합니다. 양식을 작성하면 사용자가 온라인 상태여서 양식을 제출할 수 있습니다.
이 사용 사례를 수행하려면 다음 단계를 수행해야 합니다.

* 모바일 양식에 입력한 데이터로 대화형/입력 가능한 PDF 생성 기능
* Acrobat/Reader에서 PDF 제출을 처리합니다
* AEM(Adobe Experience Manager) 워크플로우를 트리거하여 제출된 PDF 검토

이 자습서에서는 위의 사용 사례를 수행하는 데 필요한 단계를 안내합니다. 이 자습서와 관련된 샘플 코드 및 자산은 [여기에서 사용 가능합니다.](part-four.md)

다음 비디오에서는 사용 사례에 대한 개요를 제공합니다

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
