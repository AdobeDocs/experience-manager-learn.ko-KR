---
title: 샘플 배포
description: 로컬 AEM Forms 인스턴스에서 실행되는 사용 사례 가져오기
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# 샘플 배포

시스템에서 이 사용 사례를 사용하려면 다음 지침을 따르십시오.

>[!NOTE]
>포트 4502에서 AEM Forms을 실행 중이라고 가정합니다.


## 데이터베이스 만들기

이 샘플은 MySQL 데이터베이스를 사용하여 적응형 양식 데이터를 저장합니다. 스키마 파일](assets/data-base-schema.sql)을(를) MySQL Workbench로 가져와서 [데이터베이스 스키마를 만들어야 합니다.

## 데이터 소스 만들기

이전 단계에서 만든 데이터베이스 스키마를 가리키는 **StoreAndRetrieveAfData**&#x200B;이라는 Apache Sling 연결의 풀링된 DataSource를 만들어야 합니다. OSGi 번들의 코드는 이 데이터 소스 이름을 사용합니다.

## Forms 데이터 모델 만들기

양식 데이터 모델은 **StoreAndRetrieveAfData**&#x200B;이라는 이 데이터 원본을 기반으로 만들어야 합니다. 이 양식 데이터 모델은 애플리케이션 ID와 연결된 휴대폰 번호를 가져오는 데 사용됩니다. 양식 데이터 모델은 [여기에서 다운로드할 수 있습니다.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Nexmo로 개발자 계정 만들기

OTP 코드를 보내고 확인하기 위해 [Nexmo](https://dashboard.nexmo.com/)&#x200B;(으)로 개발자 계정을 만드십시오. API 키 및 API 암호 키를 기록합니다. 데이터 소스 및 양식 데이터 모델은 이 서비스에 대해 이미 만들어졌으며 이전 단계에서 언급한 자산에 포함됩니다.

## 다음 OSGi 번들 배포

데이터베이스에서 데이터를 저장하고 가져오기 위해 [코드가 있는 번들을 배포합니다](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
[developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)을(를) 다운로드하고 압축 해제합니다.
Felix 웹 콘솔을 사용하여 DevelopingWithServiceUser.jar 파일을 배포합니다.

## 클라이언트 라이브러리 배포

이 샘플은 2개의 클라이언트 라이브러리를 사용합니다. 이 [클라이언트 라이브러리](assets/store-af-with-attachments-client-lib.zip)를 AEM으로 가져옵니다.

## 사용자 지정 적응형 양식 템플릿 가져오기

이 데모에 사용된 샘플 양식은 사용자 지정 템플릿을 기반으로 합니다. [사용자 지정 템플릿을 AEM으로 가져오기](assets/custom-template-with-page-component.zip)

## 샘플 적응형 양식 가져오기

이 샘플을 구성하는 2가지 양식을 AEM으로 가져와야 합니다. 샘플 양식은 [여기에서 다운로드](assets/sample-forms.zip)할 수 있습니다.

편집 모드에서 [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)을 엽니다. 적응형 양식의 해당 필드에 Vonage API 키 및 API 암호 값을 지정합니다.

## 솔루션 테스트

[StoreAFWithAttachments를 미리 봅니다](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
국가 번호를 포함한 모바일 번호를 입력하고 사용자 세부 정보를 입력한 다음 일부 첨부 파일을 추가합니다. &quot;저장 및 종료&quot; 버튼을 클릭하여 적응형 양식과 해당 첨부 파일을 저장합니다


## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
