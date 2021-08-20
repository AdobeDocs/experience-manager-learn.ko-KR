---
title: 양식 데이터 모델 서비스를 워크플로우의 단계로 사용
description: AEM Forms 6.4부터 이제 양식 데이터 모델을 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오에서는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다.
feature: 워크플로우
type: Tutorial
version: 6.4,6.5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 양식 데이터 모델 서비스를 워크플로우의 단계로 사용 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4부터 이제 양식 데이터 모델을 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오에서는 AEM Workflow에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오
* [setvalue 번들을 다운로드하여 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 메타데이터 속성을 설정하는 사용자 지정 OSGI 번들입니다.
>!![NOTE]AEM Forms 6.5 이상에서는 여기에  [설명된 대로 이 기능을 즉시 사용할 수 있습니다](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* [여기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)에 설명된 대로 SampleRest.war 파일을 사용하여 tomcat을 설정합니다. Tomcat에 배포된 전쟁 파일에는 지원자의 신용 점수를 반환하는 코드가 있습니다. 신용 점수는 200에서 800 사이의 난수입니다

* [패키지 관리자](assets/invoke-fdm-as-service-step.zip)를 사용하여 자산을 AEM에 가져옵니다. 패키지에는 다음이 포함되어 있습니다.

   * FDM 단계를 사용하는 워크플로우 모델.
   * FDM 단계에서 사용되는 양식 데이터 모델.
   * 제출 시 워크플로우를 트리거하는 적응형 양식입니다.
* [ModerationApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 입력하고 제출하십시오. 양식 제출 시 [응용 프로그램 로드 워크플로우](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)가 트리거됩니다.

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
워크플로우는 신용 점수가 500을 초과하는 경우 애플리케이션을 관리자에게 라우팅하기 위해 또는 분할 구성 요소를 사용합니다. 신용 점수가 500점 미만인 경우, 응용 프로그램이 게시에 라우팅됩니다
