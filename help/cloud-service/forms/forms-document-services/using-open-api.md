---
title: AEM Forms 커뮤니케이션 API 구성
description: 서버 간 인증을 위한 OpenAPI 기반 AEM Forms Communication API 구성
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# AEM Forms as a Cloud Service에서 OpenAPI 기반 AEM Forms 통신 API 구성

## 사전 요구 사항

* AEM Forms as a Cloud Service의 최신 인스턴스.
* 필요한 모든 [제품 프로필이 환경에 추가됩니다.](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* 아래와 같이 제품 프로필에 AEM API 액세스 권한을 부여합니다
  ![product_profile1](assets/product-profiles1.png)
  ![product_profile](assets/product-profiles.png)

## Adobe Developer Console 프로젝트 만들기

Adobe ID을 사용하여 [Adobe Developer Console](https://developer.adobe.com/console/)에 로그인합니다.
해당 아이콘을 클릭하여 새 프로젝트를 만듭니다
![새 프로젝트](assets/new-project.png)

프로젝트에 의미 있는 이름을 지정하고 API 추가 아이콘을 클릭합니다.
![새 프로젝트](assets/new-project2.png)

Experience Cloud 선택
![new-project3](assets/new-project3.png)
AEM Forms Communications API 를 선택하고 다음 을 클릭합니다
![new-project4](assets/new-project4.png)

서버 간 인증을 선택했는지 확인하고 다음 을 클릭합니다
![new-project5](assets/new-project5.png)
프로필을 선택하고 구성된 API 저장 버튼을 클릭하여 설정을 저장합니다.
![new-project6](assets/new-project6.png)
OAuth 서버 간 클릭
![new-project7](assets/new-project7.png)
클라이언트 ID, 클라이언트 암호 및 범위 복사
![new-project8](assets/new-project8.png)

## ADC 프로젝트 통신을 사용하도록 AEM 인스턴스 구성

이미 AEM Forms 프로젝트가 있는 경우 [다음 지침에 따라](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)Adobe Developer Console 프로젝트의 OAuth 서버 간 자격 증명 ClientID를 사용하여 AEM 인스턴스와 통신할 수 있습니다

AEM Forms 프로젝트가 없는 경우 이 설명서에 따라 [AEM Forms 프로젝트를 만드십시오.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started)을(를) 만든 다음 이 설명서를 사용하여 Adobe Developer Console 프로젝트의 OAuth 서버 간 자격 증명 ClientID를 사용하여 AEM 인스턴스 [과(와) 통신할 수 있습니다.](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## 다음 단계

[액세스 토큰 생성](./generate-access-token.md)
