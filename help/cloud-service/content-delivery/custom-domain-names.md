---
title: 사용자 정의 도메인 이름 옵션
description: AEM as a Cloud Service 호스팅 웹 사이트에 대한 사용자 정의 도메인 이름을 관리하고 구현하는 방법을 알아봅니다.
version: Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---

# 사용자 정의 도메인 이름 옵션

AEM as a Cloud Service 호스팅 웹 사이트의 도메인 이름을 관리하고 구현하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## 시작하기에 앞서

사용자 정의 도메인 이름 구현을 시작하기 전에 다음 개념을 이해해야 합니다.

### 도메인 이름이란

도메인 이름은 인터넷에서 특정 위치(170.2.14.16과 같은 IP 주소)를 가리키는 adobe.com과 같은 친숙한 이름 웹 사이트 이름입니다.

### AEM as a Cloud Service의 기본 도메인 이름

기본적으로 AEM as a Cloud Service은 `*.adobeaemcloud.com`(으)로 끝나는 기본 도메인 이름으로 프로비저닝됩니다. `*.adobeaemcloud.com`에 대해 발급된 와일드카드 SSL 인증서는 모든 환경에 자동으로 적용되며 이 와일드카드 인증서는 Adobe의 책임입니다.

기본 도메인 이름은 `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com` 형식입니다.

- `<SERVICE-TYPE>`은(는) **작성자**, **게시** 또는 **미리 보기**&#x200B;일 수 있습니다.
- `<PROGRAM-ID>`은(는) 프로그램의 고유 식별자입니다. 조직에는 여러 프로그램이 있을 수 있습니다.
- `<ENVIRONMENT-ID>`은(는) 환경에 대한 고유 식별자이며 각 프로그램에는 **신속한 개발(RDE)**, **개발**, **단계** 및 **prod**&#x200B;의 네 가지 환경이 포함되어 있습니다. 각 환경에는 미리 보기 환경이 없는 **RDE**&#x200B;을(를) 제외하고 위에서 언급한 세 가지 서비스 유형이 포함되어 있습니다.

요약하면, 모든 AEM as a Cloud Service 환경이 프로비저닝되면 기본 도메인 이름과 결합된 **11**(RDE에 미리 보기 환경이 없음) 고유 URL이 있습니다.

### Adobe 관리 CDN과 고객 관리 CDN 비교

지연 시간을 줄이고 웹 사이트의 성능을 개선하기 위해 AEM as a Cloud Service은 Adobe에서 관리하는 CDN(Content Delivery Network)과 통합됩니다. Adobe 관리 CDN은 모든 환경에 대해 자동으로 활성화됩니다. 자세한 내용은 [AEM as a Cloud Service 캐싱](../caching/overview.md)을 참조하십시오.

그러나 고객은 **고객 관리 CDN**&#x200B;이라고도 하는 자체 CDN을 사용할 수도 있습니다. 꼭 필요한 것은 아니지만 기업 정책이나 다른 이유로 이용하는 고객이 거의 없다. 이 경우 고객은 CDN 구성 및 설정을 관리할 책임이 있습니다.

### 사용자 정의 도메인 이름

사용자 정의 도메인 이름은 브랜딩, 신뢰성 및 비즈니스 개발 목적으로 항상 기본 도메인 이름보다 선호됩니다. 그러나 **publish** 및 **미리보기** 서비스 유형에만 적용할 수 있으며 **author**&#x200B;에는 적용할 수 없습니다.

사용자 정의 도메인 이름을 추가할 때 지정된 사용자 정의 도메인에 대한 유효한 SSL 인증서를 제공해야 합니다. SSL 인증서는 신뢰할 수 있는 CA(인증 기관)에서 서명한 유효한 인증서여야 합니다.

일반적으로 고객은 프로덕션 환경(AEM as a Cloud Service 웹 사이트)에 대해 사용자 정의 도메인 이름을 사용하며 경우에 따라 **stage** 또는 **dev**&#x200B;과 같은 하위 환경에 대해 사용자 정의 도메인 이름을 사용합니다.

| AEM 서비스 유형 | 사용자 정의 도메인이 지원됩니까? |
|---------------------|:-----------------------:|
| 작성자 | ✘ |
| 미리보기 | ✔ |
| 게시 | ✔ |

## 도메인 이름 구현

Adobe 관리 CDN 또는 고객 관리 CDN을 사용하여 도메인 이름을 구현하려면 다음 순서도가 프로세스를 안내합니다.

![도메인 이름 관리 흐름도](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

또한 다음 표는 특정 구성을 관리할 위치를 안내합니다.

| 을 사용한 사용자 정의 도메인 이름 | 에 SSL 인증서 추가 | 에 도메인 이름 추가 | 다음 위치에서 DNS 레코드 구성 | HTTP 헤더 유효성 검사 CDN 규칙이 필요하십니까? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| Adobe 관리 CDN | Cloud Manager Adobe | Cloud Manager Adobe | DNS 호스팅 서비스 | ✘ |
| 고객 관리 CDN | CDN 공급업체 | CDN 공급업체 | DNS 호스팅 서비스 | ✔ |

### 단계별 자습서

도메인 이름 관리 프로세스를 이해했으므로 아래 자습서에 따라 AEM as a Cloud Service 웹 사이트에 대한 사용자 정의 도메인 이름을 구현할 수 있습니다.

**[Adobe 관리 CDN을 사용하는 사용자 지정 도메인 이름](./custom-domain-name-with-adobe-managed-cdn.md)**: 이 자습서에서는 Adobe 관리 CDN을 사용하는 **AEM as a Cloud Service 웹 사이트에 사용자 지정 도메인 이름을 추가하는 방법**을 배웁니다.
**[고객 관리 CDN을 사용한 사용자 지정 도메인 이름](./custom-domain-names-with-customer-managed-cdn.md)**: 이 자습서에서는 고객 관리 CDN을 사용한 **AEM as a Cloud Service 웹 사이트에 사용자 지정 도메인 이름을 추가하는 방법**&#x200B;을 배웁니다.
