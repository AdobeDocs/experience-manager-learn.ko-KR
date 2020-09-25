---
title: 양식 데이터 모델을 사용하여 이진 데이터 게시
seo-title: 양식 데이터 모델을 사용하여 이진 데이터 게시
description: 양식 데이터 모델을 사용하여 이진 데이터를 AEM DAM에 게시
seo-description: 양식 데이터 모델을 사용하여 이진 데이터를 AEM DAM에 게시
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---


# 양식 데이터 모델을 사용하여 이진 데이터 게시{#using-form-data-model-to-post-binary-data}

이제 AEM Forms 6.4부터 AEM 워크플로우의 한 단계로 양식 데이터 모델 서비스를 호출할 수 있습니다. 이 문서에서는 양식 데이터 모델 서비스를 사용하여 기록 문서를 게시하는 샘플 사용 사례를 소개합니다.

활용 사례는 다음과 같습니다.

1. 사용자가 적응형 양식을 채우고 제출합니다.
1. 적응형 양식이 기록 문서를 생성하도록 구성됩니다.
1. 이 적응형 양식의 제출 시, AEM 워크플로우는 양식 데이터 모델 서비스 호출을 사용하여 기록 문서를 AEM DAM에 POST하는 트리거입니다.

![포스트담](assets/posttodamshot1.png)

양식 데이터 모델 탭 - 속성

서비스 입력 탭에서 다음을 매핑합니다.

* file(저장해야 하는 바이너리 객체)과 페이로드 관련 DOR.pdf 속성이 있습니다. 즉, 적응형 양식이 제출되면 생성되는 기록 문서는 워크플로우 페이로드를 기준으로 DOR.pdf 파일에 저장됩니다.**응용 양식의 제출 속성을 구성할 때 이 DOR.pdf가 제공하는 것과 동일한지 확인하십시오.**

* fileName - 바이너리 개체가 DAM에 저장되는 이름입니다. 따라서 각 fileName은 제출 시 고유하도록 이 속성을 동적으로 생성해야 합니다. 이 목적을 위해 워크플로우의 프로세스 단계를 사용하여 파일 이름이라는 메타데이터 속성을 만들고 해당 값을 양식을 제출한 사람의 멤버 이름과 계정 번호의 조합으로 설정합니다. 예를 들어 사람의 멤버 이름이 John Jacobs이고 계정 번호가 9846인 경우 파일 이름은 John Jacobs_9846.pdf입니다.

![fdmserviceinput](assets/fdminputservice.png)

서비스 입력

>[!NOTE]
>
>문제 해결 팁 - 어떤 이유로 DOR.pdf가 DAM에 생성되지 않은 경우 [여기를 클릭하여 데이터 소스 인증 설정을 재설정합니다](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). AEM 인증 설정이며 기본적으로 admin/admin입니다.

서버에서 이 기능을 테스트하려면 아래 단계를 따르십시오.

1.[DeveloperServiceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue 번들](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)다운로드 및 배포. 이 사용자 지정 OSGI 번들은 메타데이터 속성을 만들고 제출된 양식 데이터에서 값을 설정하는 데 사용됩니다.

1. [패키지 관리자를 사용하여 이 아티클과](assets/postdortodam.zip) 관련된 에셋을 AEM으로 가져옵니다. 다음과 같은 결과가 나타납니다

   1. 워크플로우 모델
   1. AEM 워크플로우에 제출하도록 구성된 적응형 양식
   1. PostToDam.JSON 파일을 사용하도록 구성된 데이터 소스
   1. 데이터 소스를 사용하는 양식 데이터 모델

1. 브라우저에서 응용 [양식 열기](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 양식을 작성하고 제출합니다.
1. 기록 문서가 만들어지고 저장된 경우 자산 애플리케이션을 확인합니다.


[데이터 소스 만들기에 사용되는](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) Swagger 파일 참조
