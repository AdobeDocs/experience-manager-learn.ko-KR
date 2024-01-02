---
title: Webhooks 및 AEM 이벤트
description: 웹후크에서 AEM 이벤트를 수신하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
source-git-commit: 839d552199fe7d10a0cde4011bdfe8cf42cc8ec9
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---


# Webhooks 및 AEM 이벤트

웹후크에서 AEM 이벤트를 수신하고 페이로드, 헤더 및 메타데이터 등 이벤트 세부 사항을 검토하는 방법에 대해 알아봅니다.

이 예에서 Adobe 제공 사용 _호스팅된 webhook_ 에서는 고유한 웹후크를 설정할 필요 없이 AEM 이벤트를 수신할 수 있습니다. 이 Adobe 제공 웹후크는에서 호스팅됩니다. [결함](https://glitch.com/): 웹 애플리케이션을 구축하고 배포하는 데 도움이 되는 웹 기반 환경을 제공하는 것으로 알려진 플랫폼입니다. 그러나 원하는 경우 자체 웹후크를 사용하는 옵션도 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- AEM as a Cloud Service 환경 [AEM 이벤트 활성화됨](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [AEM 이벤트에 대해 구성된 Adobe Developer 콘솔 프로젝트](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Webhook 액세스

Adobe이 제공한 웹후크에 액세스하려면 다음 단계를 따르십시오.

- 다음에 액세스할 수 있는지 확인합니다. [결함 - 호스팅된 웹후크](https://lovely-ancient-coaster.glitch.me/) 새 브라우저 탭에서.

  ![결함 - 호스팅된 웹후크](../assets/examples/webhook/glitch-hosted-webhook.png)

- 웹후크의 고유한 이름을 입력합니다(예: ). `<YOUR_PETS_NAME>-aem-eventing` 및 클릭 **연결**. 다음이 표시됩니다. `Connected to: ${YOUR-WEBHOOK-URL}` 메시지가 화면에 표시됩니다.

  ![결함 - 웹후크 생성](../assets/examples/webhook/glitch-create-webhook.png)

- 다음을 기록합니다. **Webhook URL**. 나중에 이 자습서에서 필요합니다.

## Adobe Developer 콘솔 프로젝트에서 Webhook 구성

위의 웹후크 URL에서 AEM 이벤트를 수신하려면 다음 단계를 따르십시오.

- 다음에서 [Adobe Developer 콘솔](https://developer.adobe.com)로 이동한 다음 를 클릭하여 프로젝트를 엽니다.

- 아래 **제품 및 서비스** 섹션에서 줄임표 클릭 `...` 원하는 이벤트 카드 옆에 있는 은 AEM 이벤트를 웹후크로 보내고 을 선택합니다. **편집**.

  ![Adobe Developer 콘솔 프로젝트 편집](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 새로 오픈한 **이벤트 등록 구성** 대화 상자, 클릭 **다음** 진행하려면 **이벤트 수신 방법** 단계.

  ![Adobe Developer 콘솔 프로젝트 구성](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 다음에서 **이벤트 수신 방법** 단계, 선택 **Webhook** 옵션 및 붙여넣기 **Webhook URL** 이전에 Glitch 호스팅 웹후크에서 복사한 다음 **구성된 이벤트 저장**.

  ![Adobe Developer 콘솔 프로젝트 Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Glitch webbook 페이지에 GET 요청이 표시되어야 하며, 이 요청은 Adobe I/O 이벤트에서 webhook URL을 확인하기 위해 보낸 챌린지 요청입니다.

  ![결함 - 요청](../assets/examples/webhook/glitch-challenge-request.png)


## AEM 이벤트 트리거

위의 Adobe Developer 콘솔 프로젝트에 등록된 AEM as a Cloud Service 환경에서 AEM 이벤트를 트리거하려면 다음 단계를 따르십시오.

- AEM 를 통해 as a Cloud Service 작성자 환경에 액세스하고 로그인합니다. [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 다음에 따라 **구독한 이벤트**, 콘텐츠 조각 만들기, 업데이트, 삭제, 게시 또는 게시 취소.

## 이벤트 세부 사항 검토

위의 단계를 완료하면 AEM 이벤트가 webhook에 전달되는 것을 볼 수 있습니다. Glitch webhook 페이지에서 POST 요청을 찾습니다.

![결함 - POST 요청](../assets/examples/webhook/glitch-post-request.png)

다음은 POST 요청에 대한 주요 세부 정보입니다.

- 경로: `/webhook/${YOUR-WEBHOOK-URL}`, 예 `/webhook/AdobeTM-aem-eventing`

- headers: Adobe I/O 이벤트에서 보낸 요청 헤더(예: )

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

- body/payload: Adobe I/O 이벤트에 의해 전송된 요청 본문(예: )

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

AEM 이벤트 세부 사항에 웹후크에서 이벤트를 처리하는 데 필요한 모든 정보가 포함되어 있음을 알 수 있습니다. 예: 이벤트 유형(`type`), 이벤트 소스(`source`), 이벤트 id(`event_id`), 이벤트 시간(`time`) 및 이벤트 데이터(`data`).

## 추가 리소스

- [결함 웹후크 소스 코드](https://glitch.com/edit/#!/러블리 앤시언트 코스터) 을(를) 참조할 수 있습니다.
