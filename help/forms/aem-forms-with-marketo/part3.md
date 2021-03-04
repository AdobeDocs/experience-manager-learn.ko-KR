---
title: AEM Forms과 Marketing(3부)
seo-title: AEM Forms과 Marketing(3부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketing을 통합하는 자습서입니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketing을 통합하는 자습서입니다.
feature: '"적응형 Forms, 양식 데이터 모델"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# 데이터 소스 구성

AEM Forms 데이터 통합을 사용하여 서로 다른 데이터 소스를 구성하고 연결할 수 있습니다. 기본적으로 지원되는 유형은 다음과 같습니다. 그러나 사용자 정의 기능을 통해 다른 데이터 소스도 통합할 수 있습니다.

1. 관계형 데이터베이스 - MySQL, Microsoft SQL Server, IBM DB2 및 Oracle RDBMS
1. AEM 사용자 프로필
1. RESTful 웹 서비스
1. SOAP 기반의 웹 서비스
1. OData 서비스

AEM Forms과 Marketing의 통합을 위해 RESTful 웹 서비스를 사용할 것입니다. 통합의 첫 번째 단계는 [데이터 소스를 구성하는 것입니다.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 이 튜토리얼의 일부로 제공된 swagger 파일을 사용하십시오. 다음 스크린샷은 데이터 소스를 구성하는 동안 지정해야 하는 중요한 속성을 보여 줍니다.
![datasource](assets/datasource.jfif)

&quot;markto.json&quot;은 swagger 파일이며 이 자습서 에셋의 일부로 사용자에게 제공됩니다.
속성 호스트는 마케팅 인스턴스와 관련이 있습니다.
인증 유형은 사용자 정의이며 인증 구현은 &quot;AemForms With Marketing&quot;과 일치해야 합니다. (코드에서 이를 변경하지 않은 경우).

## 양식 데이터 모델 작성

그런 다음 데이터 소스를 구성한 후 다음 단계는 이전 단계에서 구성된 데이터 소스를 기반으로 양식 데이터 모델을 만드는 것입니다. 양식 데이터 모델을 만들려면 다음 단계를 수행하십시오.

브라우저를 [데이터 통합 페이지로 지정합니다.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) AEM 인스턴스에서 만든 모든 데이터 통합이 나열됩니다.

1. 만들기를 클릭합니다. | 양식 데이터 모델
1. FormsAndMarketing과 같은 의미 있는 제목을 입력하고 다음을 클릭합니다.
1. 이전 단계에서 구성한 데이터 소스를 선택하고 만들기 및 편집을 클릭하여 편집 모드에서 양식 데이터 모델을 엽니다
1. &quot;FormsAndMarketing&quot; 노드를 확장합니다. 서비스 노드 확장
1. 첫 번째 &quot;가져오기&quot; 작업 선택
1. 선택한 항목 추가를 클릭합니다.
1. &quot;연결된 모델 개체 추가&quot; 대화 상자에서 &quot;모두 선택&quot;을 클릭한 다음 추가를 클릭합니다
1. 저장 단추를 클릭하여 양식 데이터 모델을 저장합니다.
1. 탭 - 서비스 탭
1. 나열된 유일한 서비스를 선택하고 Test Service를 클릭합니다.
1. 유효한 leadId를 입력하고 테스트를 클릭합니다. 모든 것이 제대로 작동하면 아래 스크린샷에 나와 있는 대로 리드 세부 정보를 다시 가져와야 합니다
   ![테스트 결과](assets/testresults.jfif)
