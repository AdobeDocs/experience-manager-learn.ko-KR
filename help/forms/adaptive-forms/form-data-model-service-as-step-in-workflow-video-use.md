---
title: 양식 데이터 모델 서비스를 워크플로우의 단계로 사용
seo-title: 양식 데이터 모델 서비스를 워크플로우의 단계로 사용
description: AEM Forms 6.4부터 이제 AEM Workflow의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계에 따라 안내합니다.
seo-description: AEM Forms 6.4부터 이제 AEM Workflow의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계에 따라 안내합니다.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: 워크플로우
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: 개발
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# 양식 데이터 모델 서비스를 워크플로 {#using-form-data-model-service-as-step-in-workflow}의 단계로 사용

AEM Forms 6.4부터 이제 AEM Workflow의 일부로 양식 데이터 모델을 사용할 수 있습니다. 다음 비디오는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계에 따라 안내합니다


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오
* [setvalue 번들을 다운로드하고 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 메타데이터 속성을 설정하는 사용자 정의 OSGI 번들입니다.
>!![NOTE]AEM Forms 6.5 이상에서 이 기능은 여기에서  [설명하는 대로 즉시 사용할 수 있습니다.](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* [여기](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)에 설명된 대로 SampleRest.war 파일로 tomcat을 설정합니다. Tomcat에 배포된 전쟁 파일은 신청자의 신용 점수를 반환하기 위한 코드를 가집니다. 신용 점수는 200에서 800 사이의 무작위 숫자입니다.

* [패키지 관리자를 사용하여 자산을 AEM으로 가져옵니다](assets/invoke-fdm-as-service-step.zip). 패키지에는 다음이 포함됩니다.

   * FDM 단계를 사용하는 워크플로우 모델입니다.
   * FDM 단계에서 사용되는 양식 데이터 모델.
   * 제출 시 워크플로우를 트리거하는 적응형 양식입니다.
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 입력하고 제출합니다. 양식 제출 시 [응용 프로그램 워크플로](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)이(가) 트리거됩니다.

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
워크플로우는 신용 점수가 500점 이상인 경우 Or Split 구성 요소를 사용하여 애플리케이션을 관리자에게 전달합니다. 신용 점수가 500점 미만인 경우, 응용 프로그램은 대수로 라우팅됩니다
