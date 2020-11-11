---
title: 샘플 배포
description: 로컬 AEM Forms 인스턴스에서 사용 사례 실행
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---



# 샘플 배포

시스템에서 이 사용 사례를 사용하려면 다음 지침을 따르십시오.

>[!NOTE]
>4502 포트에서 AEM Forms을 실행하고 있다고 가정합니다.


## 데이터베이스 만들기

이 샘플에서는 MySQL 데이터베이스를 사용하여 응용 양식 데이터를 저장합니다. 스키마 파일을 MySQL Workbench로 가져와서 [데이터베이스 스키마를](assets/data-base-schema.sql) 만들어야 합니다.

## 데이터 소스 만들기

StoreAndRetrieveAfData라는 데이터 **소스를 만들어야 합니다**. OSGi 번들의 코드는 이 데이터 소스 이름을 사용합니다

## 양식 데이터 모델 작성

StoreAndRetrieveAfData라는 이 데이터 소스 **를 기반으로 양식 데이터 모델을 만들어야 합니다**. 이 양식 데이터 모델은 응용 프로그램 ID와 연결된 휴대폰 번호를 가져오는 데 사용됩니다. 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## nexmo로 개발자 계정 만들기

OTP 코드 전송 및 확인을 위해 [Nexmo가](https://dashboard.nexmo.com/) 포함된 개발자 계정을 만듭니다. API 키 및 API 암호 키에 대한 설명을 참조하십시오. 이 서비스에 대해 데이터 소스 및 양식 데이터 모델은 이미 생성되었으며 이전 단계에서 언급한 자산에 포함됩니다.

## 다음 OSGi 번들 배포

데이터베이스에 데이터를 저장하고 가져올 [코드가 있는 번들을 배포합니다.](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)DevelopingWithServiceUser 번들 [을 배포합니다](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## 클라이언트 라이브러리 배포

이 샘플에서는 2개의 클라이언트 라이브러리를 사용합니다. 이러한 [클라이언트 라이브러리를](assets/client-libraries.zip) AEM으로 가져옵니다.

## 사용자 지정 적응형 양식 템플릿 가져오기

이 데모에서 사용되는 샘플 양식은 사용자 지정 템플릿을 기반으로 합니다. AEM으로 [사용자 지정 템플릿 가져오기](assets/custom-template-with-page-component.zip)

## 샘플 적응형 양식 가져오기

이 샘플을 구성하는 2개의 양식을 AEM으로 가져와야 합니다. 샘플 양식은 여기에서 [다운로드할 수 있습니다.](assets/sample-forms.zip)

Open the [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in edit mode. 적응형 양식의 해당 필드에 API 키 및 API 암호 값을 지정합니다.

## 솔루션 테스트

StoreAFWithAttachments [미리 보기](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)국가 코드를 포함한 모바일 번호를 입력하고 사용자 세부 사항을 채우고 일부 첨부 파일을 추가합니다. &quot;저장 및 종료&quot; 버튼을 클릭하여 적응형 양식과 첨부 파일을 저장합니다


## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
