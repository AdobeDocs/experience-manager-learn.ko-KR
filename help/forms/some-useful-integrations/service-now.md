---
title: 통합 [!DNL ServiceNow]
description: 양식 데이터 모델을 사용하여 모든 인시던트를 만들고 표시합니다.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# AEM Forms과 통합 [!DNL ServiceNow]

인시던트 만들기 및 표시 [!DNL ServiceNow] AEM Forms에서 양식 데이터 모델 사용.

## 전제 조건

* [!DNL ServiceNow] 계정이 필요합니다.
* 친숙한 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 친숙한 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 샘플 자산

이 문서와 함께 제공되는 샘플 자산은 다음과 같습니다
* 클라우드 서비스 구성
* Swagger 파일을 사용하여 인시던트 만들기 및 모든 인시던트 가져오기
* Swagger 파일을 기반으로 하는 양식 데이터 모델
* 만들고 나열하는 적응형 양식 [!DNL ServiceNow] 문제

## 서버에 자산 배포

* 다운로드 [샘플 자산](assets/service-now.zip)
* 을 사용하여 자산을 AEM에 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 편집 [CreateIncident cloud service 구성](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)를 입력하여 ServiceNow 인스턴스와 일치시킬 수 있습니다.
* 편집 [GetAllInsights 클라우드 서비스 구성](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) ServiceNow 인스턴스와 일치시키려면

## 통합 테스트

* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 설명 및 설명 필드에 일부 값을 입력하고 인시던트 생성 단추를 누릅니다
* 새로 만든 인시던트의 인시던트 ID를 텍스트 필드에 채워야 하며 아래 표에는 모든 인시던트가 나열되어 있어야 합니다.
