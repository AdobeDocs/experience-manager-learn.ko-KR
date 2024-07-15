---
title: IMS를 사용하여 AEM Sites과 태그 속성 연결
description: AEM에서 IMS 구성을 사용하여 AEM Sites을 태그 속성과 연결하는 방법에 대해 알아봅니다.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# IMS를 사용하여 AEM Sites과 태그 속성 연결{#connect-aem-and-tag-property-using-ims}

AEM의 IMS(Identity Management 시스템) 구성을 사용하여 AEM을 태그 속성과 연결하는 방법에 대해 알아봅니다. 이 설정은 태그 API로 AEM을 인증하고, AEM이 태그 API를 통해 통신하여 태그 속성에 액세스할 수 있도록 합니다.

## IMS 구성 만들기 또는 재사용

AEM을 새로 만든 태그 속성과 통합하려면 Adobe Developer Console 프로젝트를 사용하는 IMS 구성이 필요합니다. 이 구성을 통해 AEM은 태그 API를 사용하여 Tags 애플리케이션과 통신할 수 있으며 IMS는 이 통합의 보안 측면을 처리합니다.

AEM as Cloud Service 환경이 프로비저닝될 때마다 Asset compute, Adobe Analytics 및 태그와 같은 몇 가지 IMS 구성이 자동으로 생성됩니다. AEM 6.X 환경을 사용하는 경우 Adobe Experience Platform **IMS 구성에서 자동으로 만든**&#x200B;태그를 사용하거나 새 IMS 구성을 만들어야 합니다.

다음 단계를 사용하여 Adobe Experience Platform에서 자동으로 만든 **태그** IMS 구성을 검토하십시오.

1. AEM 작성자에서 **도구** 메뉴를 엽니다.
1. 보안 섹션에서 Adobe IMS 구성을 선택합니다.
1. **Adobe 시작** 카드를 선택하고 **속성**&#x200B;을 클릭한 다음 **인증서** 및 **계정** 탭에서 세부 정보를 검토하십시오. 자동으로 만든 세부 정보를 수정하지 않고 돌아가려면 **취소**&#x200B;를 클릭하십시오.
1. **Adobe 시작** 카드를 선택하고 이번에는 **상태 확인**&#x200B;을 클릭하면 아래와 같이 **성공** 메시지가 표시됩니다.

   ![태그 정상 IMS 구성](assets/adobe-launch-healthy-ims-config.png)

## 다음 단계

[AEM에서 태그 Cloud Service 구성 만들기](create-aem-launch-cloud-service.md)
