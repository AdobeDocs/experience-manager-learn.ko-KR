---
title: AEM Forms 워크플로우에서 LDAP 사용
description: 제출자의 관리자에게 AEM Forms 워크플로 작업 할당
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 149
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# AEM Forms 워크플로우에서 LDAP 사용

제출자의 관리자에게 AEM Forms 워크플로우 작업 할당

AEM 워크플로우에서 적응형 양식을 사용할 때 양식 제출자의 관리자에게 작업을 동적으로 지정할 수 있습니다. 이 사용 사례를 달성하려면 Ldap를 사용하여 AEM을 구성해야 합니다.

LDAP를 사용하여 AEM을 구성하는 데 필요한 단계는에 설명되어 있습니다 [자세한 내용은 여기 를 참조하십시오.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

이 문서에서는 Adobe Ldap를 사용하여 AEM을 구성하는 데 사용되는 구성 파일을 첨부합니다. 이러한 파일은 패키지 관리자를 사용하여 가져올 수 있는 패키지에 포함됩니다.

아래 스크린샷에서는 특정 비용 센터에 속한 모든 사용자를 가져오고 있습니다. LDAP에서 모든 사용자를 가져오려면 추가 필터를 사용하지 않을 수 있습니다.

![LDAP 구성](assets/costcenterldap.gif)

아래 스크린샷에서는 LDAP에서 AEM으로 가져온 사용자에게 그룹을 할당합니다. 가져온 사용자에게 할당된 forms-users 그룹에 주목합니다. AEM Forms과의 상호 작용을 위해서는 사용자가 이 그룹의 멤버여야 합니다. 또한 AEM의 프로필/관리자 노드 아래에 관리자 속성을 저장합니다.

![싱챈들러](assets/synchandler.gif)

LDAP를 구성하고 사용자를 AEM으로 가져온 다음에는 작업을 제출자의 관리자에게 할당하는 워크플로우를 만들 수 있습니다. 이 문서에서는 간단한 1단계 승인 워크플로를 개발했습니다.

워크플로우의 첫 번째 단계에서는 initialstep 의 값을 No 로 설정합니다. 적응형 양식의 비즈니스 규칙은 &quot;제출자 세부 정보&quot; 패널을 비활성화하고 initialstep 값을 기반으로 &quot;승인자&quot; 패널을 표시합니다.

두 번째 단계에서는 제출자의 관리자에게 작업을 할당합니다. 우리는 사용자 지정 코드를 사용하여 제출자의 관리자를 받습니다.

![작업 할당](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

코드 조각은 관리자 ID를 가져오고 관리자에게 작업을 할당합니다.

워크플로우를 시작한 사람에게 연락합니다. 그런 다음 관리자 속성의 값을 가져옵니다.

Manager 속성이 LDAP에 저장되는 방법에 따라 관리자 ID를 가져오기 위해 문자열을 몇 가지 조작해야 할 수 있습니다.

이 문서를 읽고 직접 구현하십시오. [참가자 선택기 .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

시스템에서 테스트하려면(Adobe 직원의 경우 이 샘플을 즉시 사용할 수 있음)

* [setvalue 번들 다운로드 및 배포](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 관리자 속성을 설정하기 위한 사용자 지정 OSGI 번들입니다.
* [DevelopingWithServiceUserBundle 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [패키지 관리자를 사용하여 이 문서와 연결된 자산을 AEM으로 가져옵니다.](assets/aem-forms-ldap.zip).LDAP 구성 파일, 워크플로우 및 적응형 양식이 이 패키지의 일부로 포함됩니다.
* AEM 적절한 LDAP 자격 증명을 사용하여 LDAP를 구성합니다.
* LDAP 자격 증명을 사용하여 AEM에 로그인합니다.
* 를 엽니다. [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 양식을 작성하고 제출하십시오.
* 제출자의 매니저는 검토용 양식을 받아야 합니다.

>[!NOTE]
>
>관리자 이름을 추출하기 위한 이 사용자 지정 코드는 Adobe LDAP에 대해 테스트되었습니다. 다른 LDAP에 대해 이 코드를 실행하는 경우 관리자의 이름을 가져오기 위해 고유한 getParticipant 구현을 수정하거나 작성해야 합니다.
