---
title: 양식 데이터 모델 서비스를 워크플로우의 단계로 사용
seo-title: 양식 데이터 모델 서비스를 워크플로우의 단계로 사용
description: 이제 AEM Forms 6.4부터 AEM 워크플로우의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다.
seo-description: 이제 AEM Forms 6.4부터 AEM 워크플로우의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# 양식 데이터 모델 서비스를 워크플로우의 단계로 사용 {#using-form-data-model-service-as-step-in-workflow}

이제 AEM Forms 6.4부터 AEM 워크플로우의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오
* [setvalue 번들을 다운로드하고 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 메타데이터 속성을 설정하는 사용자 정의 OSGI 번들입니다.
>!![NOTE]AEM Forms 6.5 이상에서 이 기능은 여기에서  [설명하는 대로 즉시 사용할 수 있습니다.](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* [여기](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)에 설명된 대로 SampleRest.war 파일이 있는 설정 tomcat. Tomcat에 배포된 전쟁 파일은 신청자의 신용 점수를 반환하는 코드를 가지고 있습니다. 신용 점수는 200에서 800 사이의 무작위 숫자입니다.

* [패키지 관리자를 사용하여 AEM으로 에셋을 가져옵니다](assets/invoke-fdm-as-service-step.zip). 패키지에 다음이 포함됩니다.

   * FDM 단계를 사용하는 워크플로우 모델입니다.
   * FDM 단계에서 사용되는 양식 데이터 모델입니다.
   * 적응형 양식을 사용하여 제출 시 워크플로우를 트리거합니다.
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 작성하고 제출합니다. 양식 제출 시 [응용 프로그램 워크플로](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)이 트리거됩니다.

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
워크플로우는 신용 점수가 500을 초과하는 경우 애플리케이션을 관리자에게 라우팅하기 위해 Or 분할 구성 요소를 사용합니다. 신용 점수가 500점보다 작으면 응용 프로그램이 회신을 위해 라우팅됩니다
