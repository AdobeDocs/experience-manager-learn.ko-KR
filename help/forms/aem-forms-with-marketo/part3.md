---
title: AEM Forms과 Marketo(3부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 자습서입니다.
feature: 적응형 Forms, 양식 데이터 모델
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# 데이터 소스 구성

AEM Forms 데이터 통합을 사용하면 서로 다른 데이터 소스를 구성하고 연결할 수 있습니다. 기본적으로 지원되는 유형은 다음과 같습니다. 그러나 사용자 지정을 약간 사용하면 다른 데이터 소스와 통합할 수도 있습니다.

1. 관계형 데이터베이스 - MySQL, Microsoft SQL Server, IBM DB2 및 Oracle RDBMS
1. AEM 사용자 프로필
1. RESTful 웹 서비스
1. SOAP 기반 웹 서비스
1. OData 서비스

AEM Forms과 Marketo을 통합하기 위해 RESTful 웹 서비스를 사용할 것입니다. 통합에서 첫 번째 단계는 [데이터 소스를 구성하는 것입니다.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 이 자습서의 일부로 제공된 swagger 파일을 사용하십시오. 다음 스크린샷은 데이터 소스를 구성하는 동안 지정해야 하는 중요한 속성을 보여줍니다.
![데이터 소스](assets/datasource.jfif)

&quot;marketo.json&quot;은 swagger 파일이며 이 자습서 자산의 일부로 제공됩니다.
속성 호스트는 Marketo 인스턴스에만 해당됩니다.
인증 유형은 사용자 지정이며, 인증 구현은 &quot;Aem Forms With Marketo&quot;과 일치해야 합니다. (코드에서 이를 변경하지 않은 경우).

## 양식 데이터 모델 작성

그런 다음 데이터 소스를 구성하는 다음 단계는 이전 단계에서 구성된 데이터 소스를 기반으로 하는 양식 데이터 모델을 만드는 것입니다. 양식 데이터 모델을 만들려면 다음 단계를 수행하십시오.

브라우저를 [데이터 통합 페이지로 이동합니다.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) AEM 인스턴스에서 만든 모든 데이터 통합 목록이 표시됩니다.

1. 만들기 를 클릭합니다 | 양식 데이터 모델
1. FormsAndMarketo와 같은 의미 있는 제목을 제공하고 다음을 클릭합니다
1. 이전 단계에서 구성된 데이터 소스를 선택하고 만들기 및 편집 을 클릭하여 편집 모드에서 양식 데이터 모델을 엽니다
1. &quot;FormsAndMarketo&quot; 노드를 확장합니다. 서비스 노드를 확장합니다.
1. 첫 번째 &quot;가져오기&quot; 작업을 선택합니다
1. 선택 항목 추가를 클릭합니다.
1. 연결된 모델 개체 추가 대화 상자에서 &quot;모두 선택&quot;을 클릭한 다음 추가를 클릭합니다
1. Save 단추를 클릭하여 양식 데이터 모델을 저장합니다.
1. Tab 키를 눌러 서비스 탭을 표시합니다
1. 나열된 유일한 서비스를 선택하고 Test Service를 클릭합니다.
1. 올바른 leadId 를 제공하고 테스트를 클릭합니다. 모든 것이 제대로 작동하면 아래 스크린샷에 표시된 대로 리드 세부 정보를 다시 가져와야 합니다
   ![테스트 결과](assets/testresults.jfif)
