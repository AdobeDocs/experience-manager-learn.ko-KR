---
title: AEM Forms Workflow에서 Json 데이터 요소의 값 설정
description: 적응형 양식이 AEM Workflow의 다른 사용자에게 라우팅되므로 양식을 검토하는 사용자를 기반으로 특정 필드나 패널을 숨기거나 비활성화해야 하는 요구 사항이 있습니다. 이러한 사용 사례를 충족하기 위해 일반적으로 숨김 필드의 값을 설정합니다. 이 숨김 필드의 값 비즈니스 규칙을 기반으로 적절한 패널이나 필드를 숨기거나 비활성화하도록 작성할 수 있습니다.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---

# AEM Forms Workflow에서 JSON 데이터 요소의 값 설정 {#setting-value-of-json-data-element-in-aem-forms-workflow}

적응형 양식이 AEM Workflow의 다른 사용자에게 라우팅되므로 양식을 검토하는 사용자를 기반으로 특정 필드나 패널을 숨기거나 비활성화해야 하는 요구 사항이 있습니다. 이러한 사용 사례를 충족하기 위해 일반적으로 숨김 필드의 값을 설정합니다. 이 숨김 필드의 값 비즈니스 규칙을 기반으로 적절한 패널이나 필드를 숨기거나 비활성화하도록 작성할 수 있습니다.

![json 데이터에 요소의 값 설정](assets/capture-3.gif)

AEM Forms OSGi에서 - JSON 데이터 요소의 값을 설정하려면 사용자 지정 OSGi 번들을 만들어야 합니다. 이 자습서의 일부로 번들이 제공됩니다.

AEM 워크플로우에서는 프로세스 단계를 사용합니다. Adobe는 &quot;Json에서 요소 값 설정&quot; OSGi 번들을 이 프로세스 단계에 연결합니다.

설정된 값 번들에 두 개의 인수를 전달해야 합니다. 첫 번째 인수는 값을 설정해야 하는 요소의 경로입니다. 두 번째 인수는 설정해야 하는 값입니다.

예를 들어 위의 스크린샷에서는 inalStep 요소의 값을 &quot;N&quot;으로 설정하고 있습니다

afData.afUnboundData.data.initialStep,N

이 예제에서는 간단한 휴무 요청 양식이 있습니다. 이 양식의 개시자는 이름 및 시간(일)을 입력합니다. 제출 시 이 양식은 검토를 위해 &quot;관리자&quot;로 이동합니다. 관리자가 양식을 열면 첫 번째 패널의 필드가 비활성화됩니다. JSON 데이터에 있는 초기 단계 요소의 값을 N으로 설정했기 때문입니다.

초기 단계 필드 값에 따라 &quot;관리자&quot;가 요청을 승인하거나 거부할 수 있는 승인자 패널을 표시합니다.

&quot;초기 단계&quot;에 대해 설정된 규칙을 확인하십시오. initialStep 필드의 값에 따라 양식 데이터 모델을 사용하여 사용자 세부 사항을 가져오고 적절한 필드를 채우고 적절한 패널을 숨기기/비활성화합니다.

로컬 시스템에 자산을 배포하려면

* [DevelopingWithServiceUserBundle 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue 번들 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 제출된 json 데이터에 있는 요소의 값을 설정할 수 있는 사용자 지정 OSGI 번들입니다.

* [zip 파일의 컨텐츠를 다운로드하고 추출합니다](assets/set-value-jsondata.zip)
   * 브라우저를 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
      * SetValueOfElementInJSONDataWorkflow.zip을 가져와 설치합니다.이 패키지에는 양식과 연결된 샘플 워크플로우 모델과 양식 데이터 모델이 있습니다.

* 브라우저를 [Forms 및 문서](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 만들기 를 클릭합니다 | 파일 업로드
* TimeOffRequestForm.zip 파일 업로드
   **이 양식은 AEM Forms 6.4를 사용하여 빌드되었습니다. AEM Forms 6.4 이상을 사용하는지 확인하십시오**
* 를 엽니다. [양식](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 시작 및 종료 날짜 를 입력하고 양식을 제출합니다.
* 이동 [&quot;받은 편지함&quot;](http://localhost:4502/aem/inbox)
* 작업과 연결된 양식을 엽니다.
* 첫 번째 패널의 필드가 비활성화되어 있습니다.
* 이제 요청을 승인하거나 거절하는 패널이 표시됩니다.

>[!NOTE]
>
>사용자 프로필을 사용하여 적응형 양식을 미리 채우므로 관리자에게 문의하십시오 [사용자 프로필 정보 ](http://localhost:4502/security/users.html). 최소한으로 FirstName,LastName 및 Email 필드 값을 설정했는지 확인합니다.
>com.aemforms.setvalue.core.SetValueInJson에 대한 로거를 활성화하여 디버그 로깅을 활성화할 수 있습니다 [여기에서](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>JSON 데이터에서 데이터 요소의 값을 설정하는 OSGi 번들은 현재 한 번에 하나의 요소 값을 설정하는 기능을 지원합니다. 여러 요소 값을 설정하려면 프로세스 단계를 여러 번 사용해야 합니다.
>
>적응형 양식의 제출 옵션에 있는 데이터 파일 경로가 &quot;Data.xml&quot;로 설정되어 있는지 확인합니다. 프로세스 단계의 코드가 페이로드 폴더 아래에서 Data.xml이라는 파일을 찾기 때문입니다.
