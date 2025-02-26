---
title: 클라우드 서비스 구성 만들기
description: OAuth 자격 증명을 사용하여 Salesforce에 연결할 데이터 소스 만들기
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: ce22dd482417a54d222165deaf485ff69c2856b7
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# 데이터 Source 만들기

이전 단계에서 만든 Swagger 파일을 사용하여 REST 지원 데이터 소스를 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| 설정 | 값 |
|---------------------|-----------------------------------------------------------------|
| OAuth URL | https://login.salesforce.com/services/oauth2/authorize |
| 인증 범위 | api chatter_api 전체 id openid refresh_token visualforce 웹 |
| 새로 고침 토큰 URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| 토큰 URL 액세스 | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**새로 고침 및 액세스 토큰 URL의 도메인 이름이 Salesforce 계정 설정과 일치하도록 변경해야 합니다**