---
title: Forms Workflow의 이메일 전송 단계 사용
seo-title: Forms Workflow의 이메일 전송 단계 사용
description: 전자 메일 보내기 단계는 AEM Forms 6.4에 도입되었습니다. 이 단계를 사용하여 첨부 파일이 있거나 없는 전자 메일을 보낼 수 있는 비즈니스 프로세스 또는 워크플로우를 빌드할 수 있습니다. 다음 비디오에서는 이메일 전송 구성 요소를 구성하는 단계를 안내합니다
seo-description: 전자 메일 보내기 단계는 AEM Forms 6.4에 도입되었습니다. 이 단계를 사용하여 첨부 파일이 있거나 없는 전자 메일을 보낼 수 있는 비즈니스 프로세스 또는 워크플로우를 빌드할 수 있습니다. 다음 비디오에서는 이메일 전송 구성 요소를 구성하는 단계를 안내합니다
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: 워크플로우
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 1%

---


# Forms Workflow {#using-send-email-step-of-forms-workflow} 의 이메일 전송 단계 사용

전자 메일 보내기 단계는 AEM Forms 6.4에 도입되었습니다. 이 단계를 사용하여 첨부 파일이 있거나 없는 전자 메일을 보낼 수 있는 비즈니스 프로세스 또는 워크플로우를 빌드할 수 있습니다. 다음 비디오에서는 이메일 전송 구성 요소를 구성하는 단계를 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

이 문서의 일부로, 다음 사용 사례를 안내할 것입니다.

1. 사용자가 요청 양식 시간 초과
1. 양식 제출 시 AEM 워크플로우가 트리거됩니다
1. AEM Workflow는 Send Email 구성 요소를 사용하여 DoR이 첨부된 이메일을 첨부 파일로 보냅니다

이메일 보내기 단계를 사용하기 전에 [configMgr](http://localhost:4502/system/console/configMgr)에서 일 CQ 메일 서비스를 구성해야 합니다. 환경에 맞는 값을 제공합니다

![일 CQ 메일 서비스 구성](assets/mailservice.png)

이 문서와 연결된 자산의 일부로 다음 내용을 보게 됩니다

1. 제출 시 워크플로우를 트리거하는 적응형 양식
1. DOR가 포함된 이메일을 첨부 파일로 보내는 샘플 워크플로우
1. 메타데이터 속성을 만드는 OSGi 번들

시스템에서 실행 중인 샘플을 가져오려면 다음을 수행하십시오.

1. [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)이 번들에는 워크플로우의 프로세스 단계의 일부로 메타데이터 속성을 만드는 코드가 포함되어 있습니다.
1. [일 CQ 메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [패키지 관리자를 사용하여 이 문서와 연결된 자산을 CRX에 가져오고 설치합니다](assets/emaildoraemformskt.zip)
1. [적응형 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 시작합니다. 필요한 필드를 입력하고 제출합니다.
1. 첨부 파일로 DocumentOfRecord가 포함된 전자 메일을 받아야 합니다

[워크플로우 모델 탐색](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

워크플로우의 프로세스 단계를 살펴봅니다. 프로세스 단계와 연결된 사용자 지정 코드는 메타데이터 속성 이름을 만들고 제출된 데이터에서 해당 값을 설정합니다. 그러면 이 값은 전자 메일 전송 구성 요소에서 사용됩니다.

>[!NOTE]
>
>AEM Forms 6.5 이상에서는 메타데이터 속성을 만드는 데 이 사용자 지정 코드가 필요하지 않습니다. AEM Workflow에서 변수 기능을 사용하십시오

이메일 전송 구성 요소의 첨부 파일 탭이 아래 스크린샷에 따라 구성되어 있는지 확인합니다
![전자 메일 첨부 파일 탭 보내기](assets/sendemailcomponentconfigure.jpg)&quot;DOR.pdf&quot; 값은 적응형 양식의 제출 옵션에 지정된 레코드 경로 문서에 지정된 값과 일치해야 합니다.

