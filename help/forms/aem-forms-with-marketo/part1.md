---
title: AEM Forms 및 Marketo 통합
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합하는 방법을 알아봅니다.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# AEM Forms 및 Marketo 통합


Adobe의 일부인 Marketo은 이메일, 모바일, 소셜, 디지털 광고, 웹 관리 및 분석을 포함하여 계정 기반 마케팅에 중점을 둔 마케팅 자동화 소프트웨어를 제공합니다.

이제 AEM Forms의 양식 데이터 모델을 사용하여 AEM Form을 Marketo과 원활하게 통합할 수 있습니다.

[양식 데이터 모델에 대해 자세히 알아보기](https://helpx.adobe.com/kr/experience-manager/6-5/forms/using/data-integration.html)

Marketo은 시스템 기능 중 대부분을 원격으로 실행할 수 있도록 하는 REST API를 노출합니다. 프로그램 제작에서 리드 일괄 가져오기에 이르기까지 Marketo 인스턴스를 세밀하게 제어할 수 있는 많은 옵션이 있습니다. 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 것은 매우 간단합니다.

>[!NOTE]
>
>이 튜토리얼은 특히 AEM Forms 6.5에 맞게 조정되었습니다. AEM Forms as a Cloud Service을 Adobe Marketo Engage과 통합하려는 경우 [해당 통합에 대한 전용 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/forms/integrate/services/integrate-adaptive-form-with-market-engage/integrate-form-to-marketo-engage)를 참조하십시오.

이 튜토리얼에서는 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합하는 단계를 설명합니다. 자습서를 완료하면 Marketo에 대해 사용자 지정 인증을 수행하는 OSGi 번들이 생깁니다. 또한 제공된 swagger 파일을 사용하여 데이터 소스를 구성하게 됩니다.

시작하려면 사전 요구 사항 섹션에 나열된 다음 주제를 잘 알고 있는 것이 좋습니다.

## 전제 조건

1. [AEM Forms 추가 기능 패키지가 설치된 AEM 서버](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 로컬 AEM 개발 환경
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

* [이 자습서와 관련된 에셋을 다운로드하고 압축 해제합니다.](assets/marketo-integration-assets.zip)

zip 파일에는 다음 항목이 포함되어 있습니다.

1. BlankTemplatePackage.zip - 적응형 양식 템플릿입니다. 패키지 관리자를 사용하여 가져오십시오.
1. marketo.json - 데이터 소스를 구성하는 데 사용되는 Swagger 파일입니다.
1. marketo.json에서 marketo 인스턴스를 가리키도록 호스트 속성을 변경해야 합니다

## 다음 단계

[데이터 Source 만들기](./part2.md)
