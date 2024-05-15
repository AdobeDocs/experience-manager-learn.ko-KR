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
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
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

AEM을 새로 만든 태그 속성과 통합하려면 Adobe Developer 콘솔 프로젝트를 사용하는 IMS 구성이 필요합니다. 이 구성을 통해 AEM은 태그 API를 사용하여 Tags 애플리케이션과 통신할 수 있으며 IMS는 이 통합의 보안 측면을 처리합니다.

AEM as Cloud Service 환경이 프로비저닝될 때마다 Asset compute, Adobe Analytics 및 태그와 같은 몇 가지 IMS 구성이 자동으로 생성됩니다. 자동으로 만들어짐 **Adobe Experience Platform의 태그** AEM 6.X 환경을 사용하는 경우 IMS 구성을 사용하거나 새 IMS 구성을 만들어야 합니다.

검토 자동 생성됨 **Adobe Experience Platform의 태그** 다음 단계를 사용한 IMS 구성.

1. AEM Author에서 **도구** 메뉴
1. 보안 섹션에서 Adobe IMS 구성을 선택합니다.
1. 다음 항목 선택 **Adobe 실행** 카드 및 클릭 **속성**&#x200B;에서 세부 사항을 검토합니다. **인증서** 및 **계정** 탭. 그런 다음 **취소** 자동 생성된 세부 정보를 수정하지 않고 을 반환합니다.
1. 다음 항목 선택 **Adobe 실행** 카드 및 이번에는 클릭 **상태 확인**, 다음이 표시됩니다. **성공** 아래와 같은 메시지.

   ![태그 정상 IMS 구성](assets/adobe-launch-healthy-ims-config.png)

## 다음 단계

[AEM에서 태그 Cloud Service 구성 만들기](create-aem-launch-cloud-service.md)
