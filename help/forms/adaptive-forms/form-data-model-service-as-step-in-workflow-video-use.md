---
title: 워크플로우에서 단계로 양식 데이터 모델 서비스 사용
description: 이제 AEM Forms 6.4부터 양식 데이터 모델을 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오는 AEM 워크플로에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다.
feature: Workflow
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 워크플로우에서 단계로 양식 데이터 모델 서비스 사용 {#using-form-data-model-service-as-step-in-workflow}

이제 AEM Forms 6.4부터 양식 데이터 모델을 AEM Workflow의 일부로 사용할 수 있습니다. 다음 비디오는 AEM 워크플로에서 양식 데이터 모델 단계를 구성하는 데 필요한 단계를 안내합니다


>[!VIDEO](https://video.tv.adobe.com/v/33260?captions=kor&quality=12&learn=on)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오

* [setvalue 번들을 다운로드하여 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 메타데이터 속성을 설정하는 사용자 지정 OSGI 번들입니다.

  >[!NOTE]
  >
  >AEM Forms 6.5 이상에서는 이 기능을 [여기에서 설명](form-data-model-service-as-step-in-aem65-workflow-video-use.md)과 같이 즉시 사용할 수 있습니다.

* [여기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=ko)에 설명된 대로 SampleRest.war 파일로 tomcat을 설정합니다.Tomcat에 배포된 war 파일에는 지원자의 크레딧 점수를 반환하는 코드가 있습니다. 크레딧 스코어는 200에서 800 사이의 임의의 숫자입니다

* [패키지 관리자를 사용하여 AEM으로 에셋 가져오기](assets/invoke-fdm-as-service-step.zip). 패키지에는 다음이 포함됩니다.

   * FDM 단계를 사용하는 워크플로우 모델.
   * FDM 단계에서 사용되는 양식 데이터 모델
   * 제출 시 워크플로우를 트리거하는 적응형 양식입니다.
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)을 엽니다. 세부 사항을 입력하고 제출하십시오. 양식 제출 시 [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)이(가) 트리거됩니다.

![워크플로](assets/fdm-as-service-step-workflow.PNG).

크레딧 점수가 500점 이상인 경우 워크플로우는 Or 분할 구성 요소를 사용하여 애플리케이션을 관리자로 라우팅합니다. 신용 점수가 500점 미만인 경우 지원서가 cavery로 라우팅됩니다.
