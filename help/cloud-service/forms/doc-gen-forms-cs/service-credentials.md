---
title: AEM Forms 서비스 자격 증명
description: AEM의 Developer Console에서 서비스 자격 증명을 다운로드합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# AEM Forms 서비스 자격 증명

AEM as a Cloud Service과의 통합은 AEM에 안전하게 인증할 수 있어야 합니다. AEM의 Developer Console은 외부 애플리케이션, 시스템 및 서비스에서 HTTP를 통해 AEM Author 또는 Publish 서비스와 프로그래밍 방식으로 상호 작용하는 데 사용되는 서비스 자격 증명을 생성합니다.

>[!VIDEO](https://video.tv.adobe.com/v/344048?quality=12&learn=on&captions=kor)

다운로드한 서비스 자격 증명 파일은 제공된 eclipse에 service_token.json이라는 리소스 파일로 저장됩니다. service_token 파일의 값은 JWT를 생성하고 JWT를 액세스 토큰으로 교환하는 데 사용됩니다. 유틸리티 클래스 GetServiceCredentials는 service_token.json 리소스 파일에서 속성 값을 가져오는 데 사용됩니다.
