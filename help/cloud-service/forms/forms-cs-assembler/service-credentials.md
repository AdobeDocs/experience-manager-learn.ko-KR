---
title: AEM 서비스 자격 증명
description: AEM 개발자 콘솔에서 서비스 자격 증명을 다운로드합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# 서비스 자격 증명

AEM과의 통합은 AEM에 안전하게 인증할 수 있어야 합니다. AEM 개발자 콘솔은 외부 애플리케이션, 시스템 및 서비스에서 HTTP를 통해 AEM 작성자 또는 게시 서비스와 프로그래밍 방식으로 상호 작용하는 서비스 자격 증명을 생성합니다.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

다운로드한 서비스 자격 증명 파일은 제공된 eclipse에 service_token.json이라는 리소스 파일로 저장됩니다. service_token 파일의 값은 JWT를 생성하고 액세스 토큰에 대해 JWT를 교환하는 데 사용됩니다. 유틸리티 클래스 GetServiceCredentials는 service_token.json 리소스 파일에서 속성 값을 가져오는 데 사용됩니다.
