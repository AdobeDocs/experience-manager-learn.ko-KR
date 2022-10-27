---
title: AEM Forms 워크플로우에서 setvalue 사용
description: AEM Forms OSGI에서 제출된 적응형 Forms의 요소 값 설정
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# AEM Forms 워크플로우에서 setvalue 사용

AEM Forms OSGI 워크플로우에서 제출된 적응형 Forms의 XML 요소 값 설정 .

![SetValue](assets/setvalue.png)

XML 요소의 값을 설정할 수 있는 설정 값 구성 요소가 있는 LiveCycle에 사용됩니다.

이 값을 기반으로 양식을 XML로 채울 때는 양식의 특정 필드 또는 패널을 숨기거나 비활성화할 수 있습니다.

AEM Forms OSGI에서는 XML에서 값을 설정하려면 사용자 지정 OSGi 번들을 작성해야 합니다. 이 자습서의 일부로 번들이 제공됩니다.
AEM 워크플로우에서는 프로세스 단계를 사용합니다. &quot;XML에서 요소 값 설정&quot; OSGi 번들을 이 프로세스 단계에 연결합니다.
설정된 값 번들에 두 개의 인수를 전달해야 합니다. 첫 번째 인수는 값을 설정해야 하는 XML 요소의 XPath입니다. 두 번째 인수는 설정해야 하는 값입니다.
예를 들어 위의 스크린샷에서는 초기 단계 요소의 값을 &quot;N&quot;으로 설정하고 있습니다.
이 값을 기반으로 적응형 Forms의 특정 패널이 숨겨지거나 표시됩니다.
이 예제에서는 간단한 휴무 요청 양식이 있습니다. 이 양식의 개시자는 이름 및 시간(일)을 입력합니다. 제출 시 이 양식은 검토를 위해 &quot;관리자&quot;에게 전달됩니다. 관리자가 양식을 열면 첫 번째 패널의 필드가 비활성화됩니다. XML에서 초기 단계 요소의 값을 &quot;N&quot;으로 설정했기 때문입니다.

초기 단계 필드 값에 따라 &quot;관리자&quot;가 요청을 승인하거나 거부할 수 있는 두 번째 패널이 표시됩니다

규칙 편집기를 사용하여 &quot;시간 초과 요청 기준&quot; 필드에 설정된 규칙을 확인하십시오.

로컬 시스템에 자산을 배포하려면 아래 단계를 수행하십시오.

* [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [샘플 번들 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 제출된 xml 데이터에 있는 요소의 값을 설정할 수 있는 사용자 지정 OSGI 번들입니다

* [zip 파일의 컨텐츠를 다운로드하고 추출합니다](assets/setvalueassets.zip)
* 브라우저를 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* setValueWorkflow.zip을 가져와 설치합니다. 여기에는 샘플 워크플로우 모델이 있습니다.
* 브라우저를 [Forms 및 문서](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 만들기 를 클릭합니다 | 파일 업로드
* TimeOfRequestForm.zip 업로드
* 를 엽니다. [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 3개의 필수 필드를 작성하고 제출합니다.
* AEM에 &#39;admin&#39;으로 로그인합니다(아직 로그인하지 않은 경우)
* 이동 [&quot;AEM 받은 편지함&quot;](http://localhost:4502/aem/inbox)
* Review Time Off Request 양식 열기
* 첫 번째 패널의 필드가 비활성화되어 있습니다. 검토자가 양식을 열고 있기 때문입니다. 또한 요청을 승인하거나 거절하는 패널이 표시되는 것을 확인합니다

>[!NOTE]
>
>에 대한 로거를 활성화하여 디버그 로깅을 활성화할 수 있습니다
>com.aemforms.setvalue.core.SetValueinXml
>브라우저를 http://localhost:4502/system/console/slinglog으로 가리키면

>[!NOTE]
>
>적응형 양식의 제출 옵션에 있는 데이터 파일 경로가 &quot;Data.xml&quot;로 설정되어 있는지 확인합니다. 프로세스 단계에서 페이로드 폴더 아래에서 Data.xml이라는 파일을 찾기 때문입니다
