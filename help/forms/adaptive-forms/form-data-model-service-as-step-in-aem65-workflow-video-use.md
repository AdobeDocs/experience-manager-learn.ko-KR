---
title: AEM 6.5 워크플로우에서 양식 데이터 모델 서비스 사용
seo-title: AEM 6.5 워크플로우에서 양식 데이터 모델 서비스 사용
description: AEM Forms 6.5는 AEM 워크플로우에서 변수를 만드는 기능을 도입했습니다. AEM Workflow에서 "양식 데이터 모델 서비스 호출"을 사용하는 이 새로운 기능이 도입되면서 매우 수월해졌습니다. 다음 비디오에서는 AEM 워크플로우에서 양식 데이터 모델 서비스 호출 관련 단계를 안내합니다.
seo-description: AEM Forms 6.5는 AEM 워크플로우에서 변수를 만드는 기능을 도입했습니다. AEM Workflow에서 "양식 데이터 모델 서비스 호출"을 사용하는 이 새로운 기능이 도입되면서 매우 수월해졌습니다. 다음 비디오에서는 AEM 워크플로우에서 양식 데이터 모델 서비스 호출 관련 단계를 안내합니다.
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# AEM 6.5 작업 과정에서 양식 데이터 모델 서비스 사용 {#using-form-data-model-service-as-step-in-workflow}

이제 AEM Forms 6.4부터 AEM 워크플로우의 일부로 양식 데이터 모델 서비스를 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다

>!![NOTE]이 비디오에서 제시된 기능을 사용하려면 AEM Forms 6.5.1이 필요합니다.


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오

* [여기](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)에 설명된 대로 SampleRest.war 파일이 있는 설정 tomcat. Tomcat에 배포된 전쟁 파일에 신청자의 신용 점수가 반환되는 코드가 있습니다.신용 점수는 200에서 800 사이의 임의 번호입니다

* [ 패키지 관리자를 사용하여 AEM으로 에셋 가져오기](assets/aem65-loanapplication.zip)
* 패키지에는 다음과 같은 내용이 들어 있습니다.

   * FDM 단계를 사용하는 워크플로우 모델입니다.
   * FDM 단계에서 사용되는 양식 데이터 모델입니다.
   * 적응형 양식을 사용하여 제출 시 워크플로우를 트리거합니다.
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 작성하고 제출합니다. 양식 제출 시 [응용 프로그램 워크플로](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)이 트리거됩니다.

![ workflow ](assets/invokefdm651.PNG).
워크플로우는 신용 점수가 500을 초과하는 경우 애플리케이션을 관리자에게 라우팅하기 위해 Or 분할 구성 요소를 사용합니다. 신용 점수가 500점보다 작으면 지원서가 발송됩니다.
