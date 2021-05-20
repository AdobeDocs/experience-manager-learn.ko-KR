---
title: AEM 6.5 워크플로우에서 양식 데이터 모델 서비스를 단계로 사용
seo-title: AEM 6.5 워크플로우에서 양식 데이터 모델 서비스를 단계로 사용
description: AEM Forms 6.5에서는 AEM 워크플로우에서 변수를 만드는 기능을 도입했습니다. AEM Workflow에서 "양식 데이터 모델 서비스 호출"을 사용하는 이 새로운 기능을 사용하면 매우 쉽게 작업할 수 있습니다. 다음 비디오에서는 AEM Workflow에서 양식 데이터 모델 서비스 호출을 사용하는 단계를 설명합니다.
seo-description: AEM Forms 6.5에서는 AEM 워크플로우에서 변수를 만드는 기능을 도입했습니다. AEM Workflow에서 "양식 데이터 모델 서비스 호출"을 사용하는 이 새로운 기능을 사용하면 매우 쉽게 작업할 수 있습니다. 다음 비디오에서는 AEM Workflow에서 양식 데이터 모델 서비스 호출을 사용하는 단계를 설명합니다.
feature: 워크플로우
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# AEM 6.5 워크플로우 {#using-form-data-model-service-as-step-in-workflow}에서 양식 데이터 모델 서비스를 단계로 사용

AEM Forms 6.4부터 이제 양식 데이터 모델 서비스를 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오에서는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다

>!![NOTE]이 비디오에 나와 있는 기능을 사용하려면 AEM Forms 6.5.1이 필요합니다


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오

* [여기](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)에 설명된 대로 SampleRest.war 파일을 사용하여 tomcat을 설정합니다.Tomcat에 배포된 전쟁 파일에는 지원자의 신용 점수를 반환하는 코드가 있습니다.신용 점수는 200에서 800 사이의 난수입니다

* [ 패키지 관리자를 사용하여 AEM에 자산 가져오기](assets/aem65-loanapplication.zip)
* 패키지에는 다음 항목이 들어 있습니다.

   * FDM 단계를 사용하는 워크플로우 모델.
   * FDM 단계에서 사용되는 양식 데이터 모델.
   * 제출 시 워크플로우를 트리거하는 적응형 양식입니다.
* [ModerationApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 입력하고 제출하십시오. 양식 제출 시 [응용 프로그램 로드 워크플로우](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)가 트리거됩니다.

![ workflow ](assets/invokefdm651.PNG).
워크플로우는 신용 점수가 500을 초과하는 경우 애플리케이션을 관리자에게 라우팅하기 위해 또는 분할 구성 요소를 사용합니다. 신용 점수가 500점 미만인 경우, 응용 프로그램이 게시에 라우팅됩니다.
