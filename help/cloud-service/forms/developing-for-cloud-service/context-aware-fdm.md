---
title: 양식 데이터 모델에 대한 컨텍스트 인식 구성 무시 지원
description: 환경에 따라 다른 종단점과 통신하도록 양식 데이터 모델 을 설정합니다.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 0%

---

# 컨텍스트 인식 클라우드 구성

로컬 환경에서 클라우드 구성을 만들고 성공적인 테스트 시 업스트림 환경에서 동일한 클라우드 구성을 사용할 수 있지만 엔드포인트, 비밀 키/암호 및 사용자 이름을 변경할 필요가 없습니다. 이 사용 사례를 달성하기 위해 Cloud Service의 AEM Forms에서는 컨텍스트 인식 클라우드 구성을 정의하는 기능을 도입했습니다.
예를 들어, Azure 저장 공간 계정 클라우드 구성은 개발, 스테이지, 프로덕션 환경에서 다양한 연결 문자열 및 키를 사용하여 재사용할 수 있습니다.

컨텍스트 인식 클라우드 구성을 만들려면 다음 단계가 필요합니다

## 환경 변수 만들기

표준 환경 변수는 Cloud Manager를 통해 구성 및 관리할 수 있습니다. 런타임 환경에 제공되며 OSGi 구성에서 사용할 수 있습니다. [환경 변수는 변경되는 항목을 기반으로 환경별 값이나 환경 비밀이 될 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



다음 스크린샷은 정의된 azure_key 및 azure_connection_string 환경 변수를 보여줍니다
![environment_variables](assets/environment-variables.png)

그런 다음 해당 환경에서 사용할 구성 파일에 이러한 환경 변수를 지정할 수 있습니다. 예를 들어, 모든 작성 인스턴스가 이러한 환경 변수를 사용하도록 하려면 아래에 지정된 대로 config.author 폴더에 구성 파일을 정의합니다

## 구성 파일 만들기

IntelliJ에서 프로젝트를 엽니다. config.author로 이동하고 다음 파일을 만듭니다.

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

다음 텍스트를 이전 단계에서 만든 파일에 복사합니다. 이 파일의 코드가 accountName 및 accountKey 속성의 값을 환경 변수로 재정의합니다 **azure_connection_string** 및 **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>이 구성은 클라우드 서비스 인스턴스의 모든 작성자 환경에 적용됩니다. 게시 환경에 구성을 적용하려면 intelliJ 프로젝트의 config.publish 폴더에 동일한 구성 파일을 배치해야 합니다
>[!NOTE]
> 재정의되는 속성이 클라우드 구성의 올바른 속성인지 확인하십시오. 클라우드 구성으로 이동하여 아래와 같이 재정의할 속성을 찾습니다.

![cloud-config-property](assets/cloud-config-properties.png)

기본 인증이 있는 REST 기반 클라우드 구성의 경우 일반적으로 serviceEndPoint, userName 및 암호 속성에 대한 환경 변수를 만들 수 있습니다.
