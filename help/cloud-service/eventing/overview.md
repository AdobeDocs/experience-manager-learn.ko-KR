---
title: AEM 이벤트
description: AEM 이벤트, 정의, 사용 이유 및 시기, 예제에 대해 알아봅니다.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 573
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 5d8ee3b9ab6fb974f7faebb1d0ce42d699e2063c
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 0%

---


# AEM 이벤트

AEM 이벤트, 정의, 사용 이유 및 시기, 예제에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>AEM as a Cloud Service 이벤트는 프리릴리스 모드의 등록된 사용자만 사용할 수 있습니다. AEM as a Cloud Service 환경에서 AEM 이벤트를 활성화하려면 [AEM 이벤트 팀](mailto:grp-aem-events@adobe.com).

## 정의

AEM 이벤트 는 외부 시스템에서 처리하기 위해 AEM 이벤트에 대한 구독을 활성화하는 클라우드 기반 이벤트 시스템입니다. AEM 이벤트는 특정 작업이 발생할 때마다 AEM에서 전송하는 상태 변경 알림입니다. 예를 들어 컨텐츠 조각이 생성, 업데이트 또는 삭제될 때 이벤트가 포함될 수 있습니다.

![AEM 이벤트](./assets/aem-eventing.png)

AEM 위의 다이어그램은 as a Cloud Service으로 이벤트를 생성하여 Adobe I/O 이벤트로 보내는 방법을 시각화하여 이벤트 구독자에게 노출합니다.

요약하면 세 가지 주요 구성 요소가 있습니다.

1. **이벤트 공급자:** AEM as a Cloud Service.
1. **Adobe I/O 이벤트:** Adobe의 제품 및 기술을 기반으로 앱과 경험을 통합, 확장 및 구축하기 위한 개발자 플랫폼입니다.
1. **이벤트 소비자:** AEM 이벤트를 구독하는 고객 소유 시스템입니다. 예를 들어, CRM(고객 관계 관리), PIM(제품 정보 관리), OMS(주문 관리 시스템) 또는 사용자 정의 응용 프로그램이 있습니다.

### 어떻게 다른가요

다음 [Apache Sling 이벤트](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), OSGi 이벤트 및 [JCR 관찰](https://jackrabbit.apache.org/oak/docs/features/observation.html) 이벤트를 구독하고 처리하는 모든 오퍼 메커니즘. 그러나 이는 이 설명서에서 설명한 AEM 이벤트와는 다릅니다.

AEM 이벤트의 주요 차이점은 다음과 같습니다.

- 이벤트 소비자 코드는 AEM 외부에서 실행되며 AEM과 동일한 JVM에서 실행되지 않습니다.
- AEM 제품 코드는 이벤트를 정의하고 Adobe I/O 이벤트로 전송합니다.
- 이벤트 정보는 표준화되어 JSON 형식으로 전송됩니다. 자세한 내용은 다음을 참조하십시오. [cloudevents](https://cloudevents.io/).
- AEM으로 다시 전달하기 위해 이벤트 소비자는 AEM as a Cloud Service API를 사용합니다.


## 사용 이유 및 시기

AEM Eventing은 시스템 아키텍처 및 운영 효율성에 대한 다양한 이점을 제공합니다. AEM 이벤트를 사용하는 주요 이유는 다음과 같습니다.

- **이벤트 기반 아키텍처 구축**: 독립적으로 확장할 수 있고 장애에 유연하게 대처할 수 있는 느슨하게 결합된 시스템을 쉽게 만들 수 있습니다.
- **낮은 코드 및 낮은 운영 비용**: AEM의 사용자 정의 기능을 사용하지 않으므로 시스템을 유지 관리하고 확장하기가 쉬워지므로 운영 비용이 절감됩니다.
- **AEM과 외부 시스템 간의 통신 간소화**: Adobe I/O 이벤트에서 특정 시스템 또는 서비스에 제공할 AEM 이벤트를 결정하는 것과 같이 통신을 관리하도록 하여 지점 간 연결을 제거합니다.
- **이벤트의 내구성 향상**: Adobe I/O 이벤트는 대량의 이벤트를 처리하고 구독자에게 안정적으로 전달하도록 설계된 가용성 및 확장성이 뛰어난 시스템입니다.
- **이벤트의 병렬 처리**: 여러 구독자에게 동시에 이벤트를 전달하여 다양한 시스템에서 분산 이벤트 처리를 수행할 수 있습니다.
- **서버를 사용하지 않는 애플리케이션 개발**: 이벤트 소비자 코드를 서버를 사용하지 않는 애플리케이션으로 배포할 수 있도록 지원하여 시스템 유연성과 확장성을 한층 향상시킵니다.

### 제한 사항

AEM 이벤트에는 강력한 기능이지만 다음과 같은 몇 가지 제한 사항을 고려해야 합니다.

- **AEM as a Cloud Service으로 가용성 제한됨**: 현재 AEM AEM 이벤트는 as a Cloud Service에만 사용할 수 있습니다.
- **제한된 이벤트 지원**: 현재 AEM 콘텐츠 조각 이벤트만 지원됩니다. 다만 향후 행사가 추가되면서 그 범위는 확대될 것으로 보인다.

## 활성화 방법

AEM 이벤트는 AEM as a Cloud Service 환경별로 활성화되며 프리릴리스 모드의 환경에서만 사용할 수 있습니다. 연락처 [AEM 이벤트 팀](mailto:grp-aem-events@adobe.com) 를 사용하여 AEM 이벤트를 통해 AEM 환경을 활성화할 수 있습니다.

이미 활성화된 경우 다음을 참조하십시오. [AEM Cloud Service 환경에서 AEM 이벤트 활성화](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) 을 참조하십시오.

## 구독 방법

AEM 이벤트에 가입하려면 AEM에서 코드를 작성할 필요가 없고 [Adobe Developer 콘솔](https://developer.adobe.com/) 프로젝트가 구성되었습니다. Adobe Developer 콘솔은 Adobe API, SDK, 이벤트, 런타임 및 App Builder에 대한 게이트웨이입니다.

이 경우 a _프로젝트_ Adobe Developer AEM 콘솔에서 as a Cloud Service 환경에서 내보내는 이벤트를 구독하고 외부 시스템에 이벤트 전달을 구성할 수 있습니다.

자세한 내용은 [Adobe Developer 콘솔에서 AEM 이벤트에 가입하는 방법](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 사용 방법

AEM 이벤트를 소비하는 기본 방법에는 두 가지가 있습니다. _푸시_ 메서드 및 _가져오기_ 메서드를 사용합니다.

- **푸시 메서드**: 이 접근 방법에서는 이벤트를 사용할 수 있게 되면 Adobe I/O 이벤트를 통해 이벤트 소비자에게 미리 알림을 보냅니다. 통합 옵션에는 Webhooks, Adobe I/O Runtime 및 Amazon EventBridge가 포함됩니다.
- **끌어오기 방법**: 여기에서 이벤트 소비자는 적극적으로 Adobe I/O 이벤트를 폴링하여 새 이벤트를 확인합니다. 이 메서드에 대한 주요 통합 옵션은 Adobe Developer 저널링 API입니다.

자세한 내용은 [Adobe I/O 이벤트를 통한 AEM 이벤트 처리](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## 예

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="웹후크에서 AEM 이벤트 수신" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">웹후크에서 AEM 이벤트 수신</a></strong></div>
        <p>
          Adobe 제공 웹후크를 사용하여 AEM 이벤트를 수신하고 이벤트 세부 사항을 검토합니다.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM 이벤트 저널 로드" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">AEM 이벤트 저널 로드</a></strong></div>
        <p>
          Adobe 제공 웹 애플리케이션을 사용하여 저널에서 AEM 이벤트를 로드하고 이벤트 세부 사항을 검토합니다.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="Adobe I/O Runtime 작업에 대한 AEM 이벤트 수신" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">Adobe I/O Runtime 작업에 대한 AEM 이벤트 수신</a></strong></div>
        <p>
          AEM 이벤트를 수신하고 이벤트 세부 사항을 검토합니다.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="Adobe I/O Runtime 작업을 사용하여 AEM 이벤트 처리" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">Adobe I/O Runtime 작업을 사용하여 AEM 이벤트 처리</a></strong></div>
        <p>
          Adobe I/O Runtime 작업을 사용하여 수신된 AEM 이벤트를 처리하는 방법을 알아봅니다. 이벤트 처리에는 AEM 콜백, 이벤트 데이터 지속성 및 SPA에서의 표시가 포함됩니다.
        </p>
      </td>
  </tr>    
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="PIM 통합을 위한 AEM Assets 이벤트" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">PIM 통합을 위한 AEM Assets 이벤트</a></strong></div>
        <p>
          메타데이터 업데이트를 위해 AEM Assets 및 PIM(제품 정보 관리) 시스템을 통합하는 방법을 알아봅니다.
        </p>
      </td>
  </tr>  
</table>