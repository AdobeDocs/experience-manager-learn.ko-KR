---
title: 샘플 배포
description: 로컬 AEM Forms 인스턴스에서 실행 중인 사용 사례 가져오기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# 샘플 배포

시스템에서 이 사용 사례를 사용하려면 다음 지침을 따르십시오.

>[!NOTE]
>포트 4502에서 AEM Forms을 실행 중이라고 가정합니다.


## 데이터베이스 만들기

이 샘플은 MySQL 데이터베이스를 사용하여 응용 양식 데이터를 저장합니다. 을(를) 만들어야 합니다 [스키마 파일을 가져와 데이터베이스 스키마](assets/data-base-schema.sql) MySQL Workbench로 이동합니다.

## 데이터 소스 만들기

데이터 소스를 만들어야 합니다. **StoreAndRetrieveAfData**. OSGi 번들의 코드는 이 데이터 소스 이름을 사용합니다

## 양식 데이터 모델 만들기

양식 데이터 모델은 **StoreAndRetrieveAfData**. 이 양식 데이터 모델은 애플리케이션 ID와 연결된 휴대폰 번호를 가져오는 데 사용됩니다. 양식 데이터 모델은 [여기에서 다운로드했습니다.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 다음 아이콘을 사용하여 개발자 계정 만들기

다음 아이콘을 사용하여 개발자 계정 만들기 [넥스모](https://dashboard.nexmo.com/) 참조하십시오. API 키 및 API 암호 키를 기록합니다. 이 서비스에 대해 데이터 소스 및 양식 데이터 모델이 이미 생성되었으며, 이전 단계에서 언급한 자산에 포함됩니다.

## 다음 OSGi 번들 배포

가 있는 번들 배포 [데이터베이스에서 데이터를 저장하고 가져오는 코드](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
다운로드 및 압축 해제 [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Felix 웹 콘솔을 사용하여 DevelopingWithServiceUser.jar 파일을 배포합니다.

## 클라이언트 라이브러리 배포

이 샘플은 2개의 클라이언트 라이브러리를 사용합니다. 다음 항목 가져오기 [클라이언트 라이브러리](assets/client-libraries.zip) AEM으로 전환합니다.

## 사용자 지정 적응형 양식 템플릿 가져오기

이 데모에서 사용되는 샘플 양식은 사용자 지정 템플릿을 기반으로 합니다. 가져오기 [AEM에 사용자 지정 템플릿](assets/custom-template-with-page-component.zip)

## 샘플 적응형 양식 가져오기

이 샘플을 구성하는 두 양식을 AEM으로 가져와야 합니다. 샘플 양식은 [여기에서 다운로드](assets/sample-forms.zip)

를 엽니다. [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 편집 모드. 적응형 양식의 해당 필드에 API 키 및 API 암호 값을 지정합니다.

## 솔루션 테스트

미리 보기 [StoreAFWwithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
국가 코드를 포함한 모바일 번호를 입력하고 사용자 세부 사항을 입력하고 첨부 파일을 추가합니다. &quot;저장 및 종료&quot; 단추를 클릭하여 적응형 양식과 첨부 파일을 저장합니다


## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
