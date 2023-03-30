---
title: IMS를 사용하여 AEM과 태그 속성 연결
description: AEM에서 IMS 구성을 사용하여 AEM과 태그 속성을 연결하는 방법을 알아봅니다. 이 설정은 Launch API를 사용하여 AEM을 인증하고, AEM이 Launch API를 통해 통신하여 태그 속성에 액세스할 수 있도록 합니다.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# IMS를 사용하여 AEM과 태그 속성 연결{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>데이터 수집 기술 세트로 Adobe Experience Platform Launch 이름을 바꾸는 프로세스는 AEM 제품 UI, 콘텐츠 및 설명서에서 구현되고 있으므로 Launch라는 용어가 계속 여기에서 사용됩니다.

AEM에서 IMS(Identity Management System) 구성을 사용하여 AEM과 태그 속성을 연결하는 방법을 알아봅니다. 이 설정은 Launch API를 사용하여 AEM을 인증하고, AEM이 Launch API를 통해 통신하여 태그 속성에 액세스할 수 있도록 합니다.

## IMS 구성 만들기 또는 재사용

AEM을 새로 만든 태그 속성과 통합하려면 Adobe Developer 콘솔 프로젝트를 사용하는 IMS 구성이 필요합니다. 이 구성을 통해 AEM은 Launch API를 사용하여 태그 애플리케이션과 통신하고 IMS는 이 통합의 보안 측면을 처리합니다.

AEM as a Cloud Service 환경에 프로비저닝될 때마다 Asset compute, Adobe Analytics, Adobe Launch와 같은 몇 가지 IMS 구성이 자동으로 생성됩니다. 자동 생성된 **Adobe 실행** AEM 6.X 환경을 사용하는 경우 IMS 구성을 사용하거나 새 IMS 구성을 만들어야 합니다.

자동 생성된 검토 **Adobe 실행** 다음 단계를 사용한 IMS 구성.

1. AEM에서 **도구** 메뉴를 엽니다

1. 보안 섹션에서 Adobe IMS 구성을 선택합니다.

1. 을(를) 선택합니다 **Adobe 실행** 카드를 클릭한 다음 **속성**&#x200B;에서 세부 사항을 검토합니다. **인증서** 및 **계정** 탭. 그런 다음 **취소** 자동으로 생성된 세부 정보를 수정하지 않고 반환합니다.

1. 을(를) 선택합니다 **Adobe 실행** 카드 및 이번에는 클릭 **상태 확인**&#x200B;에 로그인하면 **성공** 아래와 같은 메시지.

   ![Adobe Launch 정상 IMS 구성](assets/adobe-launch-healthy-ims-config.png)


## 다음 단계

[AEM에서 Launch Cloud Service 구성 만들기](create-aem-launch-cloud-service.md)
