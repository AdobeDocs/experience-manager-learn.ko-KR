---
title: Forms Workflow의 이메일 전송 단계 사용
description: 이메일 보내기 단계는 AEM Forms 6.4에서 도입되었습니다. 이 단계를 사용하여 첨부 파일이 있거나 없는 이메일을 보낼 수 있는 비즈니스 프로세스 또는 워크플로우를 빌드할 수 있습니다. 다음 비디오는 이메일 구성 요소 전송을 구성하는 단계를 안내합니다
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# Forms Workflow의 이메일 전송 단계 사용 {#using-send-email-step-of-forms-workflow}

이메일 보내기 단계는 AEM Forms 6.4에서 도입되었습니다. 이 단계를 사용하여 첨부 파일이 있거나 없는 이메일을 보낼 수 있는 비즈니스 프로세스 또는 워크플로우를 빌드할 수 있습니다. 다음 비디오는 이메일 구성 요소 보내기를 구성하는 단계를 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

이 문서의 일부로 다음 사용 사례를 소개합니다.

1. 사용자가 휴가 요청 양식을 작성합니다.
1. 양식 제출 시 AEM 워크플로우가 트리거됩니다
1. AEM 워크플로는 이메일 전송 구성 요소를 사용하여 DoR이 첨부 파일로 포함된 이메일을 전송합니다

이메일 보내기 단계를 사용하기 전에 [configMgr](http://localhost:4502/system/console/configMgr)에서 일별 CQ 메일 서비스를 구성해야 합니다. 사용자 환경에 맞는 값 제공

![일별 CQ 메일 서비스 구성](assets/mailservice.png)

이 문서와 연결된 에셋의 일부로 다음을 얻을 수 있습니다

1. 제출 시 워크플로우를 트리거하는 적응형 양식
1. DOR이 첨부 파일로 포함된 이메일을 전송하는 샘플 워크플로
1. 메타데이터 속성을 만드는 OSGi 번들

시스템에서 샘플을 실행하려면 다음을 수행하십시오.

1. [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)이 번들에는 워크플로의 프로세스 단계의 일부로 메타데이터 속성을 만드는 코드가 포함되어 있습니다.
1. [일별 CQ 메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [패키지 관리자를 사용하여 이 문서와 연결된 에셋을 CRX으로 가져오고 설치합니다](assets/emaildoraemformskt.zip)
1. [적응형 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 시작합니다. 필수 필드를 입력한 다음 제출합니다.
1. DocumentOfRecord가 첨부 파일로 포함된 이메일을 받아야 합니다.

[워크플로 모델](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html) 탐색

워크플로우의 프로세스 단계를 살펴봅니다. 프로세스 단계와 연결된 사용자 지정 코드는 메타데이터 속성 이름을 만들고 제출된 데이터에서 해당 값을 설정합니다. 그런 다음 이메일 전송 구성 요소에서 이 값을 사용합니다.

>[!NOTE]
>
>AEM Forms 6.5 이상에서는 메타데이터 속성을 만드는 데 이 사용자 지정 코드가 필요하지 않습니다. AEM 워크플로우에서 변수 기능을 사용하십시오.

이메일 전송 구성 요소의 첨부 파일 탭이 아래 스크린샷에 따라 구성되어 있는지 확인하십시오
![전자 메일 첨부 파일 보내기 탭](assets/sendemailcomponentconfigure.jpg)&quot;DOR.pdf&quot; 값은 적응형 양식의 제출 옵션에 지정된 기록 문서 경로에 지정된 값과 일치해야 합니다.
