---
title: Forms Workflow의 이메일 단계 보내기 사용
seo-title: Forms Workflow의 이메일 단계 보내기 사용
description: 이메일 보내기 단계는 AEM Forms 6.4에서 도입되었습니다. 이 단계를 통해 첨부 파일을 포함한 이메일 전송 또는 없는 이메일을 전송할 수 있는 비즈니스 프로세스 또는 워크플로우를 구축할 수 있습니다. 다음 비디오에서는 이메일 보내기 구성 요소를 구성하는 절차를 안내합니다
seo-description: 이메일 보내기 단계는 AEM Forms 6.4에서 도입되었습니다. 이 단계를 통해 첨부 파일을 포함한 이메일 전송 또는 없는 이메일을 전송할 수 있는 비즈니스 프로세스 또는 워크플로우를 구축할 수 있습니다. 다음 비디오에서는 이메일 보내기 구성 요소를 구성하는 절차를 안내합니다
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---


# Forms Workflow의 이메일 단계 보내기 사용 {#using-send-email-step-of-forms-workflow}

이메일 보내기 단계는 AEM Forms 6.4에서 도입되었습니다. 이 단계를 통해 첨부 파일을 포함한 이메일 전송 또는 없는 이메일을 전송할 수 있는 비즈니스 프로세스 또는 워크플로우를 구축할 수 있습니다. 다음 비디오에서는 이메일 보내기 구성 요소를 구성하는 절차를 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

본 문서의 일부로 다음 사용 사례를 살펴보십시오.

1. 사용자가 시간 초과 요청 양식을 채웁니다.
1. 양식 제출 시 AEM 워크플로우가 트리거됩니다.
1. AEM Workflow는 이메일 보내기 구성 요소를 사용하여 DoR이 첨부된 이메일을 추가합니다

이메일 보내기 단계를 사용하기 전에 configMgr에서 CQ 메일 서비스에 대한 요일을 [구성해야 합니다](http://localhost:4502/system/console/configMgr). 환경에 맞는 값 제공

![일 CQ 메일 서비스 구성](assets/mailservice.png)

이 아티클과 관련된 자산의 일부로, 다음과 같은 메시지가 표시됩니다

1. 제출 시 워크플로우를 트리거하는 적응형 양식
1. 첨부 파일로 DOR가 포함된 이메일을 보내는 샘플 워크플로우
1. 메타데이터 속성을 만드는 OSGi 번들

시스템에서 샘플을 실행하려면 다음을 수행하십시오.

1. [Developingserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)bundle 다운로드 및 설치이 번들에는 워크플로우의 프로세스 단계의 일부로 메타데이터 속성을 만드는 코드가 포함되어 있습니다.
1. [일 CQ 메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [패키지 관리자를 사용하여 이 아티클과 연결된 에셋을 CRX로 가져오기 및 설치](assets/emaildoraemformskt.zip)
1. 응용 [양식을 시작합니다](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). 필요한 필드를 입력하고 제출합니다.
1. DocumentOfRecord가 첨부된 이메일을 받아야 합니다.

워크플로우 모델 [살펴보기](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

워크플로우의 프로세스 단계를 살펴봅니다. 프로세스 단계와 연결된 사용자 지정 코드는 메타데이터 속성 이름을 만들고 제출된 데이터에서 해당 값을 설정합니다. 그런 다음 이 값은 이메일 전송 구성 요소에서 사용됩니다.

>[!NOTE]
>
>AEM Forms 6.5 이상에서 메타데이터 속성을 만드는 데 이 사용자 지정 코드가 필요하지 않습니다. AEM Workflow에서 변수 기능을 사용하십시오.

이메일 보내기 구성 요소의 첨부 파일 탭이 이메일 첨부 파일 탭 아래의![스크린샷에 따라 구성되었는지](assets/sendemailcomponentconfigure.jpg)확인하십시오.&quot;DOR.pdf&quot; 값은 적응형 양식의 제출 옵션에 지정된 기록 경로 문서에 지정된 값과 일치해야 합니다.

