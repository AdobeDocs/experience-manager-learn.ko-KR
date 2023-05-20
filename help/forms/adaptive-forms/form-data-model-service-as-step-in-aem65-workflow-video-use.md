---
title: AEM 6.5 워크플로에서 양식 데이터 모델 서비스를 단계로 사용
description: AEM Forms 6.5에는 AEM Workflow에 변수를 만드는 기능이 도입되었습니다. AEM Workflow의 "양식 데이터 모델 서비스 호출"을 사용하는 이 새로운 기능은 매우 쉬워졌습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 서비스 호출 사용과 관련된 단계를 설명합니다.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# AEM 6.5 워크플로에서 양식 데이터 모델 서비스를 단계로 사용 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4부터 이제 양식 데이터 모델 서비스를 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다

>!![NOTE]이 비디오에서 설명하는 기능을 사용하려면 AEM Forms 6.5.1이 필요합니다


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오

* 설명된 대로 SampleRest.war 파일로 tomcat 설정 [여기](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Tomcat에 배포된 war 파일에는 지원자의 크레딧 점수를 반환하는 코드가 있습니다.크레딧 점수는 200에서 800 사이의 임의의 숫자입니다

* [ 패키지 관리자를 사용하여 AEM에 에셋 가져오기](assets/aem65-loanapplication.zip)
* 패키지에는 다음 항목이 포함되어 있습니다.

   * FDM 단계를 사용하는 워크플로우 모델.
   * FDM 단계에서 사용되는 양식 데이터 모델
   * 제출 시 워크플로우를 트리거하는 적응형 양식입니다.
* 를 엽니다. [모기지 애플리케이션 양식](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 세부 사항을 입력하고 제출하십시오. 양식 제출 시 [응용 프로그램 워크플로 대여](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 가 트리거됩니다.

![ 워크플로우 ](assets/invokefdm651.PNG).
크레딧 점수가 500점 이상인 경우 워크플로우는 Or 분할 구성 요소를 사용하여 애플리케이션을 관리자로 라우팅합니다. 신용 점수가 500점 미만인 경우 지원서가 cavery로 라우팅됩니다.
