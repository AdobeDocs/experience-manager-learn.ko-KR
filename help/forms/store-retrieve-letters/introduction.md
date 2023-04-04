---
title: 문자 저장 및 다시 시작
seo-title: Save and resume letters
description: 초안 문자를 저장하고 검색하는 방법을 알아봅니다
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 소개

대화형 커뮤니케이션을 사용하면 임시 통신을 준비하는 에이전트가 부분적으로 완료된 서신을 저장하고 이를 검색하여 계속 작업할 수 있습니다. AEM Forms에서는 다음을 제공합니다 [서비스 공급자 인터페이스](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). 고객은 저장 및 재개 기능을 얻기 위해 이 인터페이스를 구현해야 합니다.

이 문서에서는 MySQL 데이터베이스를 사용하여 편지 인스턴스의 메타데이터를 저장합니다. 편지 데이터는 파일 시스템에 저장됩니다.

다음 비디오에서는 이 사용 사례를 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 사전 요구 사항

필요에 따라 솔루션을 구현하려면 다음 사항이 필요합니다

* AEM Forms 작업 경험
* Forms 추가 기능이 있는 AEM Server 6.5
* OSGI 번들 빌드에는 익숙해야 합니다
