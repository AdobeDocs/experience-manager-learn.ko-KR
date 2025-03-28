---
title: PDF 양식 제출에서 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식을 계속 채우고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# 부분적으로 완료된 모바일 양식을 다운로드하고 AEM 워크플로우를 트리거하기 위해 제출

일반적인 사용 사례는 데이터 캡처 활동을 위해 XDP를 HTML으로 렌더링하는 기능입니다. 양식이 단순하고 온라인에서 작성 및 제출이 가능한 경우 잘 작동합니다. 그러나 양식이 복잡한 경우 사용자가 온라인에서 양식을 작성하지 못할 수 있으므로 양식 작성기가 양식 작성기를 다운로드하여 Acrobat/Reader을 오프라인 방식으로 사용하여 채울 대화식 버전을 다운로드할 수 있도록 해야 합니다. 양식이 작성되면 사용자는 온라인으로 양식을 제출할 수 있습니다.
이 사용 사례를 달성하려면 다음 단계를 수행해야 합니다.

* 모바일 양식에 입력된 데이터로 대화형/입력 가능한 PDF 생성 기능
* Acrobat/Reader에서 PDF 제출 처리
* Adobe Experience Manager(AEM) 워크플로우를 트리거하여 제출된 PDF 검토

이 튜토리얼에서는 위의 사용 사례를 완수하는 데 필요한 단계를 안내합니다. 이 자습서와 관련된 샘플 코드 및 자산은 [여기에서 사용할 수 있습니다.](./deploy-assets.md)

다음 비디오에서는 사용 사례에 대한 개요를 제공합니다

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## 다음 단계

[사용자 지정 프로필 만들기](./custom-profile.md)