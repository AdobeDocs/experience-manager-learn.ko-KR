---
title: AEM Forms과 Marketo(3부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 방법에 대한 자습서입니다.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 108
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 1%

---

# 데이터 소스 구성

AEM Forms 데이터 통합을 사용하면 서로 다른 데이터 소스를 구성하고 연결할 수 있습니다. 기본적으로 지원되는 유형은 다음과 같습니다. 그러나 약간의 맞춤화를 통해 다른 데이터 소스와도 통합할 수 있습니다.

1. 관계형 데이터베이스 - MySQL, Microsoft SQL Server, IBM DB2 및 Oracle RDBMS
1. AEM 사용자 프로필
1. RESTful 웹 서비스
1. SOAP 기반 웹 서비스
1. OData 서비스

AEM Forms과 Marketo의 통합을 위해 RESTful 웹 서비스를 사용하고 있습니다. 통합의 첫 번째 단계는 를 구성하는 것입니다. [데이터 소스.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 이 자습서의 일부로 제공된 swagger 파일을 사용하십시오. 다음 스크린샷은 데이터 소스를 구성하는 동안 지정해야 하는 중요한 속성을 보여 줍니다.
![데이터 소스](assets/datasource.jfif)

&quot;marketo.json&quot;은 Swagger 파일이며 이 자습서 자산의 일부로 제공됩니다.
속성 호스트는 Marketo 인스턴스에만 적용됩니다.
인증 유형은 사용자 정의이며 인증 구현은 &quot;Marketo이 포함된 AemForms&quot;와 일치해야 합니다. (코드에서 이를 변경하지 않은 경우).

## 양식 데이터 모델 만들기

그런 다음 데이터 소스를 구성한 다음 단계는 이전 단계에서 구성한 데이터 소스를 기반으로 하는 양식 데이터 모델을 만드는 것입니다. 양식 데이터 모델을 만들려면 다음 단계를 수행하십시오.

브라우저를 가리켜 [데이터 통합 페이지.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) AEM 인스턴스에 생성된 모든 데이터 통합 목록이 표시됩니다.

1. 만들기 를 클릭합니다 | 양식 데이터 모델
1. FormsAndMarketo와 같은 의미 있는 제목을 입력하고 다음 을 클릭합니다
1. 이전 단계에서 구성한 데이터 소스를 선택하고 만들기 및 편집 을 클릭하여 편집 모드에서 양식 데이터 모델 을 엽니다
1. &quot;FormsAndMarketo&quot; 노드를 확장합니다. 서비스 노드를 확장합니다.
1. 첫 번째 &quot;가져오기&quot; 작업 선택
1. 선택 항목 추가를 클릭합니다
1. &quot;연결된 모델 개체 추가&quot; 대화 상자에서 &quot;모두 선택&quot;을 클릭한 다음 추가를 클릭합니다
1. 저장 단추를 클릭하여 양식 데이터 모델을 저장합니다.
1. 서비스 탭 탭
1. 나열된 유일한 서비스를 선택하고 테스트 서비스 를 클릭합니다.
1. 유효한 leadId를 입력하고 Test 를 클릭합니다. 모든 것이 잘 진행되면 아래 스크린샷과 같이 리드 세부 정보를 다시 가져와야 합니다
   ![testresults](assets/testresults.jfif)

## 다음 단계

[테스트를 위해 모든 구성 요소 통합](./part4.md)
