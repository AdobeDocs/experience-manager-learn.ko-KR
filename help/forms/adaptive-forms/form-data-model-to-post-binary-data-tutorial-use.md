---
title: 양식 데이터 모델을 사용하여 이진 데이터 게시
description: 양식 데이터 모델을 사용하여 이진 데이터를 AEM DAM에 게시
feature: 워크플로우
version: 6.4,6.5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 1%

---


# 양식 데이터 모델을 사용하여 이진 데이터 게시{#using-form-data-model-to-post-binary-data}

AEM Forms 6.4부터 이제 양식 데이터 모델 서비스를 AEM Workflow의 단계로 호출할 수 있습니다. 이 문서에서는 양식 데이터 모델 서비스를 사용하여 레코드 문서를 게시하기 위한 샘플 사용 사례를 안내합니다.

사용 사례는 다음과 같습니다.

1. 사용자가 적응형 양식을 작성하고 제출합니다.
1. 적응형 양식은 기록 문서를 생성하도록 구성됩니다.
1. 이 적응형 양식 제출 시 AEM Workflow가 트리거되어 양식 데이터 모델 서비스 호출을 사용하여 AEM DAM에 레코드 문서를 POST합니다.

![posttodam](assets/posttodamshot1.png)

양식 데이터 모델 탭 - 속성

서비스 입력 탭에서 다음을 매핑합니다

* 페이로드를 기준으로 DOR.pdf 속성을 사용하는 파일(저장해야 하는 이진 개체)입니다. 즉, 적응형 양식을 제출할 때 생성되는 레코드 문서는 워크플로우 페이로드를 기준으로 DOR.pdf라는 파일에 저장됩니다.**적응형 양식의 제출 속성을 구성할 때 이 DOR.pdf가 제공하는 것과 같은지 확인하십시오.**

* fileName - 이진 개체가 DAM에 저장되는 이름입니다. 따라서 이 속성을 동적으로 생성하여 각 fileName이 제출마다 고유하도록 합니다. 이를 위해 워크플로우의 프로세스 단계를 사용하여 파일 이름이라는 메타데이터 속성을 만들고 해당 값을 양식을 제출하는 사람의 멤버 이름 및 계정 번호 조합으로 설정했습니다. 예를 들어 개인의 멤버 이름이 John Jacobs이고 계정 번호가 9846인 경우 파일 이름은 John Jacobs_9846.pdf입니다.

![fdmserviceinput](assets/fdminputservice.png)

서비스 입력

>[!NOTE]
>
>문제 해결 팁 - 어떤 이유로 DAM에 DOR.pdf가 생성되지 않은 경우 [여기](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)를 클릭하여 데이터 소스 인증 설정을 재설정하십시오. AEM 인증 설정이며, 기본적으로 admin/admin입니다.

서버에서 이 기능을 테스트하려면 아래 설명된 단계를 따르십시오.

1.[Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue 번들을 다운로드하여 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 이 사용자 지정 OSGI 번들은 메타데이터 속성을 만들고 제출된 양식 데이터에서 해당 값을 설정하는 데 사용됩니다.

1. [패키지 관리자](assets/postdortodam.zip) 를 사용하여 이 문서와 연결된 자산을 AEM에 가져옵니다. 다음과 같이 표시됩니다

   1. 워크플로우 모델
   1. AEM 워크플로우에 제출하도록 구성된 적응형 양식
   1. PostToDam.JSON 파일을 사용하도록 구성된 데이터 소스
   1. 데이터 소스를 사용하는 양식 데이터 모델

1. [브라우저를 가리켜 적응형 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 엽니다.
1. 양식을 작성하고 제출하십시오.
1. 레코드 문서가 생성 및 저장된 경우 자산 애플리케이션을 확인합니다.


[데이터 ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 소스를 만드는 데 사용되는 Swagger 파일을 참조할 수 있습니다
