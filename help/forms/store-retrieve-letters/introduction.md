---
title: 편지 저장 및 다시 시작
description: 초안 문자를 저장하고 검색하는 방법 알아보기
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 소개

대화형 커뮤니케이션을 사용하면 애드혹 응답을 준비하는 에이전트에서 부분적으로 완료된 응답을 저장하고 동일한 응답을 검색하여 계속 작업할 수 있습니다. AEM Forms은 [서비스 공급자 인터페이스](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html)를 제공합니다. 고객은 저장 및 다시 시작 기능을 가져오기 위해 이 인터페이스를 구현해야 합니다.

이 문서에서는 MySQL 데이터베이스를 사용하여 문자 인스턴스의 메타데이터를 저장합니다. 편지 데이터는 파일 시스템에 저장됩니다.

다음 비디오에서는 사용 사례를 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/3441447?quality=12&learn=on&captions=kor)

## 전제 조건

필요에 따라 솔루션을 구현하려면 다음 사항이 필요합니다

* AEM Forms 작업 환경
* AEM Server 6.5(Forms 추가 기능 포함)
* OSGI 번들 빌드에 익숙해야 함
