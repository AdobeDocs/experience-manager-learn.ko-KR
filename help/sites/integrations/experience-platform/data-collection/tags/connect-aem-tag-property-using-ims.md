---
title: IMS를 사용하여 AEM과 태그 속성 연결
description: AEM에서 IMS 구성을 사용하여 AEM을 태그 속성과 연결하는 방법에 대해 알아봅니다. 이 설정은 Launch API를 사용하여 AEM을 인증하고, AEM이 Launch API를 통해 통신하여 태그 속성에 액세스할 수 있도록 합니다.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# IMS를 사용하여 AEM과 태그 속성 연결{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Adobe Experience Platform Launch의 이름을 데이터 수집 기술 집합으로 바꾸는 프로세스가 AEM 제품 UI, 콘텐츠 및 설명서에서 구현되고 있으므로 Launch라는 용어는 여전히 여기에서 사용되고 있습니다.

AEM의 IMS(Identity Management 시스템) 구성을 사용하여 AEM을 태그 속성과 연결하는 방법에 대해 알아봅니다. 이 설정은 Launch API를 사용하여 AEM을 인증하고, AEM이 Launch API를 통해 통신하여 태그 속성에 액세스할 수 있도록 합니다.

## IMS 구성 만들기 또는 재사용

AEM을 새로 만든 태그 속성과 통합하려면 Adobe Developer 콘솔 프로젝트를 사용하는 IMS 구성이 필요합니다. 이 구성을 통해 AEM은 Launch API를 사용하여 Tags 애플리케이션과 통신할 수 있으며 IMS는 이 통합의 보안 측면을 처리합니다.

AEM as Cloud Service 환경이 프로비저닝될 때마다 Asset compute, Adobe Analytics 및 Adobe 실행과 같은 몇 가지 IMS 구성이 자동으로 생성됩니다. 자동으로 만들어짐 **Adobe 실행** AEM 6.X 환경을 사용하는 경우 IMS 구성을 사용하거나 새 IMS 구성을 만들어야 합니다.

검토 자동 생성됨 **Adobe 실행** 다음 단계를 사용한 IMS 구성.

1. AEM에서 **도구** 메뉴를 엽니다

1. 보안 섹션에서 Adobe IMS 구성을 선택합니다.

1. 다음 항목 선택 **Adobe 실행** 카드 및 클릭 **속성**&#x200B;에서 세부 사항을 검토합니다. **인증서** 및 **계정** 탭. 그런 다음 **취소** 자동 생성된 세부 정보를 수정하지 않고 을 반환합니다.

1. 다음 항목 선택 **Adobe 실행** 카드 및 이번에는 클릭 **상태 확인**, 다음이 표시됩니다. **성공** 아래와 같은 메시지.

   ![Adobe 시작 정상 IMS 구성](assets/adobe-launch-healthy-ims-config.png)


## 다음 단계

[AEM에서 Launch Cloud Service 구성 만들기](create-aem-launch-cloud-service.md)
