---
title: AEM Forms Cloud Service 및 Marketo 통합
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합하는 방법을 알아봅니다.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---

# AEM Forms 및 Marketo 통합

Adobe의 일부인 Marketo은 이메일, 모바일, 소셜, 디지털 광고, 웹 관리 및 분석을 포함하여 계정 기반 마케팅에 중점을 둔 마케팅 자동화 소프트웨어를 제공합니다.

이제 AEM Forms의 양식 데이터 모델을 사용하여 AEM Form을 Marketo과 원활하게 통합할 수 있습니다.

[양식 데이터 모델에 대해 자세히 알아보기](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo은 시스템 기능 중 대부분을 원격으로 실행할 수 있도록 하는 REST API를 노출합니다. 프로그램 제작에서 리드 일괄 가져오기에 이르기까지 Marketo 인스턴스를 세밀하게 제어할 수 있는 많은 옵션이 있습니다. 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 것은 매우 간단합니다.

이 튜토리얼에서는 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합하는 단계를 설명합니다. 자습서를 완료하면 Marketo에 대해 사용자 지정 인증을 수행하는 OSGi 번들이 생깁니다. 또한 제공된 swagger 파일을 사용하여 데이터 소스를 구성하게 됩니다.

시작하려면 사전 요구 사항 섹션에 나열된 다음 주제를 잘 알고 있는 것이 좋습니다.

## 전제 조건

1. AEM Forms Cloud Service 인스턴스에 액세스
1. 양식 데이터 모델에 익숙함
1. Swagger 파일에 대한 기본 지식
1. 적응형 Forms 만들기

**클라이언트 암호 ID 및 클라이언트 암호 키**

Marketo과 AEM Forms을 통합하는 첫 번째 단계는 API를 사용하여 REST를 호출하는 데 필요한 API 자격 증명을 얻는 것입니다. 다음이 필요합니다.

1. client_id
1. 클라이언트 암호
1. identity_endpoint

[위에서 언급한 속성을 가져오려면 공식 Marketo 문서를 따르십시오.](https://developers.marketo.com/rest-api/) 또는 Marketo 인스턴스의 관리자에게 문의할 수도 있습니다.

**시작하기 전에**

* [이 자습서와 관련된 에셋을 다운로드하고 압축 해제합니다.](assets/marketo.zip)

zip 파일에는 다음 항목이 포함되어 있습니다.

1. marketo.json - 데이터 소스를 구성하는 데 사용되는 Swagger 파일입니다.
1. marketo.json에서 marketo 인스턴스를 가리키도록 호스트 속성을 변경해야 합니다

## 다음 단계

[데이터 Source 만들기](./part2.md)
