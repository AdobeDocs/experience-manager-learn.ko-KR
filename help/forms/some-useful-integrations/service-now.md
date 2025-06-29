---
title: ' [!DNL ServiceNow]과 통합'
description: 양식 데이터 모델을 사용하여 모든 인시던트를 만들고 표시합니다.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# [!DNL ServiceNow]과(와) AEM Forms 통합

AEM Forms의 양식 데이터 모델을 사용하여 [!DNL ServiceNow]에서 인시던트를 만들고 표시합니다.

## 사전 요구 사항

* [!DNL ServiceNow] 계정입니다.
* [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ko)에 익숙함
* [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ko)에 익숙함

## 샘플 Assets

이 문서와 함께 제공되는 샘플 에셋에는 다음 항목이 포함됩니다

* 클라우드 서비스 구성
* 장애를 만들고 모두 가져오기 위한 Swagger 파일   문제
* Swagger 파일을 기반으로 한 양식 데이터 모델
* [!DNL ServiceNow]개의 인시던트를 만들고 나열할 적응형 양식

## 서버에 자산 배포

* [샘플 자산](assets/service-now.zip) 다운로드
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 AEM에 자산 가져오기
* 이 통합에 사용된 Swagger 파일이 crx 저장소의 ```/conf/9957/settings/cloudconfigs/fdm``` 폴더 아래에 있습니다.
* ServiceNow 인스턴스와 일치하도록 [CreateIncident 클라우드 서비스 구성](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)을(를) 편집합니다.
* ServiceNow 인스턴스와 일치하도록 [GetAllIncidents 클라우드 서비스 구성](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents)을(를) 편집합니다. ServiceNow 인스턴스 자격 증명과 일치하도록 호스트, 사용자 이름 및 암호를 변경해야 합니다.

## ServiceNow 인스턴스 자격 증명에 액세스

* 사용자 프로필 클릭
  ![사용자 프로필 클릭](assets/snow-1.png)

* 인스턴스 암호 관리 를 클릭합니다.
* 인스턴스 세부 사항은 아래와 같이 표시됩니다
  ![인스턴스 세부 정보](assets/snow-3.png)

## 통합 테스트

* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 설명 및 설명 필드에 일부 값을 입력하고 문제 생성 버튼을 클릭합니다
* 새로 만든 인시던트의 인시던트 ID가 텍스트 필드에 채워지고 아래 표에는 모든 인시던트가 나열되어야 합니다.
