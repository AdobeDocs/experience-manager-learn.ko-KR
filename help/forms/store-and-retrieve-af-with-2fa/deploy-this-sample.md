---
title: 샘플 배포
description: 로컬 AEM Forms 인스턴스에서 실행되는 사용 사례 가져오기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# 샘플 배포

시스템에서 이 사용 사례를 사용하려면 다음 지침을 따르십시오.

>[!NOTE]
>포트 4502에서 AEM Forms을 실행 중이라고 가정합니다.


## 데이터베이스 만들기

이 샘플은 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장합니다. 다음을 만들어야 합니다. [스키마 파일을 가져와서 데이터베이스 스키마](assets/data-base-schema.sql) MySQL Workbench로 이동합니다.

## 데이터 소스 만들기

라는 Apache Sling 연결의 풀링된 데이터 소스를 만들어야 합니다 **StoreAndRetrieveAfData** 이전 단계에서 생성된 데이터베이스 스키마를 가리킵니다. OSGi 번들의 코드는 이 데이터 소스 이름을 사용합니다.

## 양식 데이터 모델 만들기

양식 데이터 모델은 이라는 이 데이터 소스를 기반으로 생성해야 합니다. **StoreAndRetrieveAfData**. 이 양식 데이터 모델은 애플리케이션 ID와 연결된 휴대폰 번호를 가져오는 데 사용됩니다. 양식 데이터 모델은 다음과 같을 수 있습니다 [여기에서 다운로드했습니다.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Nexmo로 개발자 계정 만들기

다음을 사용하여 개발자 계정 만들기 [Nexmo](https://dashboard.nexmo.com/) OTP 코드 전송 및 확인. API 키 및 API 암호 키를 기록합니다. 데이터 소스 및 양식 데이터 모델은 이 서비스에 대해 이미 만들어졌으며 이전 단계에서 언급한 자산에 포함됩니다.

## 다음 OSGi 번들 배포

이 포함된 번들 배포 [데이터베이스에서 데이터를 저장하고 가져오는 코드](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
을(를) 다운로드하고 압축 해제합니다. [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Felix 웹 콘솔을 사용하여 DevelopingWithServiceUser.jar 파일을 배포합니다.

## 클라이언트 라이브러리 배포

이 샘플은 2개의 클라이언트 라이브러리를 사용합니다. 가져오기 [클라이언트 라이브러리](assets/store-af-with-attachments-client-lib.zip) AEM으로.

## 사용자 지정 적응형 양식 템플릿 가져오기

이 데모에 사용된 샘플 양식은 사용자 지정 템플릿을 기반으로 합니다. 가져오기 [사용자 지정 템플릿을 AEM으로](assets/custom-template-with-page-component.zip)

## 샘플 적응형 양식 가져오기

이 샘플을 구성하는 2가지 양식을 AEM으로 가져와야 합니다. 샘플 양식은 다음과 같습니다. [여기에서 다운로드됨](assets/sample-forms.zip)

를 엽니다. [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 편집 모드로 전환됩니다. 적응형 양식의 해당 필드에 Vonage API 키 및 API 암호 값을 지정합니다.

## 솔루션 테스트

미리 보기 [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
국가 번호를 포함한 모바일 번호를 입력하고 사용자 세부 정보를 입력한 다음 일부 첨부 파일을 추가합니다. &quot;저장 및 종료&quot; 버튼을 클릭하여 적응형 양식과 해당 첨부 파일을 저장합니다


## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
