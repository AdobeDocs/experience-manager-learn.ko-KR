---
title: 양식 데이터 모델을 Post 바이너리 데이터에 사용
description: 양식 데이터 모델을 사용하여 AEM DAM에 이진 데이터 게시
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 양식 데이터 모델을 Post 바이너리 데이터에 사용{#using-form-data-model-to-post-binary-data}

AEM Forms 6.4부터 이제 AEM Workflow의 한 단계로 양식 데이터 모델 서비스를 호출할 수 있습니다. 이 문서에서는 양식 데이터 모델 서비스를 사용하여 기록 문서를 게시하기 위한 샘플 사용 사례를 안내합니다.

사용 사례는 다음과 같습니다.

1. 사용자가 적응형 양식을 작성하여 제출합니다.
1. 적응형 양식은 기록 문서를 생성하도록 구성됩니다.
1. 이 적응형 양식을 제출하면 양식 데이터 모델 서비스 호출을 사용하여 기록 문서를 AEM DAM으로 POST 하는 AEM Workflow가 트리거됩니다.

![posttodam](assets/posttodamshot1.png)

양식 데이터 모델 탭 - 속성

서비스 입력 탭에서 다음을 매핑합니다

* 페이로드에 상대적인 DOR.pdf 속성이 있는 파일(저장해야 하는 이진 개체)입니다. 즉, 적응형 양식이 제출되면 생성된 기록 문서는 워크플로 페이로드와 관련하여 DOR.pdf라는 파일에 저장됩니다.**이 DOR.pdf가 적응형 양식의 제출 속성을 구성할 때 제공한 것과 동일한지 확인하십시오.**

* fileName - DAM에 이진 개체가 저장되는 이름입니다. 각 fileName이 제출마다 고유하도록 이 속성을 동적으로 생성하려는 경우 이를 위해 워크플로우의 프로세스 단계를 사용하여 filename이라는 메타데이터 속성을 만들고 해당 값을 양식을 제출하는 사용자의 멤버 이름과 계정 번호 조합으로 설정했습니다. 예를 들어 개인의 멤버 이름이 John Jacobs이고 계정 번호가 9846인 경우 파일 이름은 John Jacobs_9846.pdf가 됩니다

![fdmserviceinput](assets/fdminputservice.png)

서비스 입력

>[!NOTE]
>
>문제 해결 팁 - 어떤 이유로든 DOR.pdf가 DAM에 만들어지지 않으면 [여기](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)를 클릭하여 데이터 소스 인증 설정을 재설정하십시오. AEM 인증 설정이며 기본적으로 관리자/관리자입니다.

서버에서 이 기능을 테스트하려면 아래 단계를 따르십시오.

1.[Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue 번들을 다운로드하여 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 이 사용자 지정 OSGI 번들은 메타데이터 속성을 만들고 제출된 양식 데이터에서 해당 값을 설정하는 데 사용됩니다.

1. 패키지 관리자를 사용하여 [이 문서와 연결된 자산을 AEM으로 가져오기](assets/postdortodam.zip)합니다.다음이 제공됩니다

   1. 워크플로 모델
   1. AEM Workflow에 제출하도록 구성된 적응형 양식
   1. PostToDam.JSON 파일을 사용하도록 구성된 데이터 소스
   1. 데이터 Source을 사용하는 양식 데이터 모델

1. [브라우저에서 적응형 양식을 열도록 지정](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 양식을 작성하고 제출합니다.
1. 기록 문서가 생성되고 저장되는 경우 Assets 애플리케이션을 확인합니다.


데이터 원본을 만드는 데 사용된 [Swagger 파일](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile)을(를) 참조할 수 있습니다.
