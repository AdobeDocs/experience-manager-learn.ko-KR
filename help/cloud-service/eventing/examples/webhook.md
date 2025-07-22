---
title: Webhooks 및 AEM 이벤트
description: 웹후크에서 AEM 이벤트를 수신하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 1%

---

# Webhooks 및 AEM 이벤트

웹후크에서 AEM 이벤트를 수신하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토하는 방법에 대해 알아봅니다.


>[!VIDEO](https://video.tv.adobe.com/v/3449755?quality=12&learn=on&captions=kor)


>[!IMPORTANT]
>
>이 자습서의 라이브 데모 끝점은 이전에 [Glitch](https://glitch.com/)에서 호스팅되었습니다. 2025년 7월부터 Glitch가 호스팅 서비스를 중단했으며 더 이상 종단점에 액세스할 수 없습니다.
>&#x200B;>데모를 대체 플랫폼으로 마이그레이션하는 데 적극적으로 노력하고 있습니다. 튜토리얼 콘텐츠는 정확하게 유지되며 업데이트된 링크가 곧 제공됩니다.
>&#x200B;>양해와 인내심에 감사드립니다.

라이브 데모 엔드포인트를 다시 사용할 수 있을 때까지 자체 웹후크를 사용하십시오.

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- [AEM 이벤트 사용](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)이 설정된 AEM as a Cloud Service 환경.

- [AEM 이벤트에 대해 Adobe Developer Console 프로젝트가 구성됨](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Webhook 액세스

Adobe에서 제공한 웹후크에 액세스하려면 다음 단계를 따르십시오.

- 새 브라우저 탭에서 [결함 - 호스트된 웹후크](https://lovely-ancient-coaster.glitch.me/)에 액세스할 수 있는지 확인하십시오.

  ![결함 - 호스팅된 webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- 웹후크의 고유 이름(예: `<YOUR_PETS_NAME>-aem-eventing`)을 입력하고 **연결**&#x200B;을 클릭합니다. 화면에 `Connected to: ${YOUR-WEBHOOK-URL}`개의 메시지가 표시됩니다.

  ![결함 - 웹후크 만들기](../assets/examples/webhook/glitch-create-webhook.png)

- **Webhook URL**&#x200B;을 메모하세요. 나중에 이 자습서에서 필요합니다.

## Adobe Developer Console 프로젝트에서 Webhook 구성

위의 웹후크 URL에서 AEM 이벤트를 수신하려면 다음 단계를 따르십시오.

- [Adobe Developer Console](https://developer.adobe.com)에서 프로젝트로 이동한 다음 클릭하여 엽니다.

- **제품 및 서비스** 섹션에서 웹후크로 AEM 이벤트를 보낼 원하는 이벤트 카드 옆에 있는 생략 부호 `...`을(를) 클릭하고 **편집**&#x200B;을(를) 선택합니다.

  ![Adobe Developer Console 프로젝트 편집](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 새로 연 **이벤트 등록 구성** 대화 상자에서 **다음**&#x200B;을 클릭하여 **이벤트를 받는 방법** 단계로 진행합니다.

  ![Adobe Developer Console 프로젝트 구성](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- **이벤트를 받는 방법** 단계에서 **Webhook** 옵션을 선택하고 Glitch 호스팅 Webhook에서 이전에 복사한 **Webhook URL**&#x200B;을(를) 붙여 넣은 다음 **구성된 이벤트 저장**&#x200B;을 클릭합니다.

  ![Adobe Developer Console 프로젝트 Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Glitch webbook 페이지에 GET 요청이 표시되어야 하며, 이 요청은 Adobe I/O Events에서 webhook URL을 확인하기 위해 보낸 챌린지 요청입니다.

  ![결함 - 요청](../assets/examples/webhook/glitch-challenge-request.png)


## AEM 이벤트 트리거

위의 Adobe Developer Console 프로젝트에 등록된 AEM as a Cloud Service 환경에서 AEM 이벤트를 트리거하려면 다음 단계를 따르십시오.

- [Cloud Manager](https://my.cloudmanager.adobe.com/)을(를) 통해 AEM as a Cloud Service 작성자 환경에 액세스하고 로그인합니다.

- **구독한 이벤트**&#x200B;에 따라 콘텐츠 조각을 만들거나, 업데이트하거나, 삭제하거나, 게시하거나, 게시 취소합니다.

## 이벤트 세부 사항 검토

위의 단계를 완료하면 AEM 이벤트가 웹후크로 전달되는 것을 볼 수 있습니다. Glitch webhook 페이지에서 POST 요청을 찾습니다.

![결함 - POST 요청](../assets/examples/webhook/glitch-post-request.png)

다음은 POST 요청의 주요 세부 정보입니다.

- 경로: `/webhook/${YOUR-WEBHOOK-URL}`(예: `/webhook/AdobeTM-aem-eventing`)

- headers: Adobe I/O Events에서 보낸 요청 헤더(예: )

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- body/payload: Adobe I/O Events에서 보낸 요청 본문(예: )

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

AEM 이벤트 세부 사항에 웹후크에서 이벤트를 처리하는 데 필요한 모든 정보가 포함되어 있음을 볼 수 있습니다. 예를 들어 이벤트 유형(`type`), 이벤트 소스(`source`), 이벤트 ID(`event_id`), 이벤트 시간(`time`) 및 이벤트 데이터(`data`)가 있습니다.

## 추가 리소스

- [AEM-Eventing Webhook](../assets/examples/webhook/aemeventing-webhook.tgz) 소스 코드를 참조할 수 있습니다.
